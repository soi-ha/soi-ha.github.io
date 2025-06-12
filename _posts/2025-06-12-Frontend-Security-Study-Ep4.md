---
layout: post
published: true
categories:
  - TIL
title: '눈에 보이지 않는 위협: CSRF, 클릭재킹, 오픈 리다이렉트'
tags:
  - TIL
  - Security
---

## CSRF (Cross-Site Request Forgery, 사이트 간 요청 위조)

CSRF는 사용자가 로그인한 상태에서 **의도하지 않은 요청을 다른 사이트를 통해 보내도록 유도**하는 공격이다. 서버는 사용자가 보낸 요청인지 공격자가 보낸 요청인지 구분하지 못하므로, 중요한 작업(예: 비밀번호 변경, 송금 등)이 공격자에 의해 실행될 수 있다.

### 🔍 공격 시나리오

#### 구조 설명

1. 사용자가 `bank.com`에 로그인한 상태로 브라우저를 켜둔다.
2. 그리고 공격자가 만든 악성 사이트 `evil.com`에 접속한다.
3. 이 사이트에는 `img`, `form`, `script` 등을 이용해 자동으로 `bank.com`에 요청을 보내는 코드가 존재한다.
4. 사용자의 브라우저는 로그인 상태 쿠키를 `bank.com` 요청에 자동으로 포함시킨다.

#### 예시 코드

```html
<!-- evil.com에 존재하는 HTML -->
<!-- display: none;으로 인해 보이지 않음 -->
<form action="https://bank.com/transfer" method="POST" style="display: none;">
	<input type="hidden" name="to" value="attacker-account" />
	<input type="hidden" name="amount" value="100000" />
	<input type="submit" />
</form>
<script>
	document.forms[0].submit(); // 자동 제출
</script>
```

#### ⚠️ 왜 위험한가?

- 사용자는 **악성 사이트에서 클릭도 하지 않고** 은행 계좌에서 송금이 일어난다.
- 요청은 정상적인 POST 방식이고, 사용자의 세션 쿠키도 포함되어 있으므로 서버는 정상 요청으로 오해하게 된다.

---

### 🛡 대응 방법

#### 1. SameSite 쿠키 설정

```js
Set-Cookie: sessionId=abc123; SameSite=Strict; Secure; HttpOnly
```

- `SameSite=Strict`: 다른 출처에서 유입된 모든 요청에 쿠키를 포함하지 않음 → 가장 강력
- `SameSite=Lax`: 안전한 메서드(GET 등)만 허용 → 로그인 후 이동 정도는 가능
- `SameSite=None`: 외부 도메인에서도 사용 가능, 단 `Secure` 필수 → CORS와 함께 주의 필요

> 프론트에서는 이 설정이 **제대로 작동하는지** 확인해야 하며, CORS 설정과 함께 연동될 수 있다.

#### 2. CSRF 토큰 사용

**토큰 발급 구조**

로그인 시, 서버가 고유한 CSRF 토큰을 발급해 HTML에 포함하거나 응답으로 내려준다.

```html
<!-- index.html -->
<meta name="csrf-token" content="abc123" />
```

**API 요청 시 토큰을 헤더에 추가**

```js
const token = document.querySelector('meta[name="csrf-token"]').content;

axios.post(
	'/transfer',
	{ to: 'abc', amount: 10000 },
	{
		headers: { 'X-CSRF-Token': token },
	}
);
```

서버는 쿠키 인증 외에도 이 토큰이 있는지 검증해, 공격자가 위조한 요청을 거부한다.

---

## 클릭재킹 (Clickjacking)

클릭재킹은 정상적인 UI를 **`iframe`으로 감싸서 투명하게 만든 뒤,** 사용자가 **의도하지 않은 클릭을 하게 만드는 공격**이다. 사용자는 겉으로는 로그인 버튼을 클릭한 줄 알지만, 실제로는 송금, 결제 등의 위험한 동작을 실행한 것이다.

### 🔍 공격 시나리오

#### 구조 설명

1. 공격자가 자신의 사이트 `evil.com`에 `iframe`으로 `your-site.com`의 민감한 페이지(예: 설정, 결제, 회원탈퇴 등)를 삽입한다.
2. `iframe`은 `opacity: 0` 또는 `z-index`를 이용해 투명하게 숨겨진 상태로 배치된다.
3. 사용자는 '플레이' 버튼을 누른다고 생각하지만, 실제로는 `iframe` 안의 '회원 탈퇴' 버튼을 누르게 되는 것이다.

#### 예시 코드

```html
<!-- evil.com -->
<style>
	iframe {
		opacity: 0;
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		z-index: 10;
	}

	button {
		position: relative;
		z-index: 1;
	}
</style>

<iframe src="https://your-site.com/settings/delete-account"></iframe>
<button>플레이 버튼</button>
```

#### ⚠️ 왜 위험한가?

- 사용자는 자신의 의지와 상관없이 민감한 동작을 실행할 수 있다.
- 예를 들어, SNS에서 "좋아요"를 클릭하는 것처럼 보이지만 실제로는 비밀번호 변경이나 탈퇴 등의 작업이 이루어질 수 있다.
- 관리자 페이지에 대한 클릭재킹이 이루어지면, 권한 변경이나 서비스 중단 등의 심각한 결과로 이어질 수 있다.
- 공격이 시각적으로 인지되지 않기 때문에, 피해자가 공격 사실을 오랫동안 인지하지 못할 수 있다.

---

### 🛡 대응 방법

#### 1. `X-Frame-Options` 헤더 설정

서버에서 응답 헤더로 다음을 설정한다.  
어떤 사이트에서도 이 페이지를 `iframe`으로 삽입할 수 없다.

```js
X-Frame-Options: DENY
```

**같은 출처 내에서만** iframe 삽입을 가능 (예: 도메인이 같은 관리자용 프리뷰 등 허용 가능)하게 하고 싶다면 다음과 같이 설정한다.

```js
X-Frame-Options: SAMEORIGIN
```

#### 2. `Content-Security-Policy` 설정

보다 유연하고 정밀한 제어가 필요하다면 다음과 같은 방식으로 설정한다.

```js
Content-Security-Policy: frame-ancestors 'none';
```

- `frame-ancestors 'none'`: **모든 사이트에서 iframe 삽입을 금지**
- `frame-ancestors 'self'`: **같은 출처만 허용**
- `frame-ancestors https://trusted.com`: 특정 도메인만 허용

#### 3. 프론트에서 iframe 감지

보안은 완전히 되지 않지만, 사용자 브라우저에서 감지해 탈출하는 방법이다.

```js
if (window.top !== window.self) {
	// iframe 안에 있음
	window.top.location = window.location; // 부모로 이동
}
```

---

## 오픈 리다이렉트 (Open Redirect)

오픈 리다이렉트는 사용자가 로그인 또는 특정 작업을 마친 후 **외부 악성 사이트로 이동하도록 유도**할 수 있는 취약점이다. 주로 URL 파라미터로 리다이렉트 주소를 받는 로직에서 발생한다.

### 🔍 공격 시나리오

#### 구조 설명

1. 공격자는 사용자에게 다음과 같은 URL을 보낸다.

   ```js
   https://your-site.com/login?redirect=https://evil.com
   ```

2. 사용자는 신뢰할 수 있는 사이트(`your-site.com`)인 줄 알고 로그인하거나 작업을 진행한다.

3. 로그인 후, 프론트에서는 redirect 파라미터를 읽어 `window.location.href`로 이동 처리한다.

   ```js
   const redirect = new URLSearchParams(location.search).get('redirect');
   window.location.href = redirect;
   ```

4. 결과적으로 사용자는 공격자가 제어하는 피싱 사이트(`evil.com`)로 이동하게 된다.

#### ⚠️ 왜 위험한가?

- 사용자는 URL이 `your-site.com`으로 시작되기 때문에 신뢰하고 클릭하지만, 결국엔 공격자가 만든 외부 사이트로 리다이렉트된다.

- 피싱 사이트에서 사용자에게 다시 로그인 또는 개인정보 입력을 요구함으로써, 계정 탈취나 금융 정보 유출로 이어질 수 있다.

- OAuth 로그인 방식(SNS 로그인)과 함께 쓰일 경우, 공격자는 토큰 탈취, 로그인 세션 가로채기 등의 추가적인 피해를 유발할 수 있다.

- 공격 URL을 이메일, 메신저 등에 퍼뜨리기 쉬워 사회공학적 공격(Social Engineering)에 자주 활용된다.

> **사회공학적 공격?**  
> 사람의 심리나 습관을 이용해 정보를 탈취하는 공격 방식이다. 기술보다 사람을 속여서 보안을 우회하는 것이 핵심이다.

---

### 🛡 대응 방법

#### 1. 리다이렉트 경로 화이트리스트 제한

**사전에 허용된 경로만 리다이렉트 대상으로 허용하는 화이트리스트 방식**이다.  
예를 들어 사용자가 로그인 후 `/dashboard`나 `/mypage`로 이동하도록 할 수는 있지만, `https://evil.com`처럼 **외부 URL은 강제로 거부되거나 기본 경로(`/`)로 이동**하게 된다.

단순하지만 **가장 확실하게 오픈 리다이렉트 취약점을 차단할 수 있는 방법**이다.
단점은 화이트리스트가 늘어날수록 관리가 번거로워질 수 있다..

```js
const redirectParam = new URLSearchParams(location.search).get('redirect');
const allowList = ['/dashboard', '/mypage', '/account'];

// 절대 URL 사용 금지, 내부 경로만 허용
if (redirectParam && allowList.includes(redirectParam)) {
	window.location.href = redirectParam;
} else {
	window.location.href = '/';
}
```

#### 2. 외부 URL 탐지 및 경고

리다이렉트 대상이 **외부 URL인지 여부만 판단**하여, 외부 URL일 경우 **즉시 이동하지 않고 사용자에게 경고**를 표시하는 방식이다.  
내부 경로뿐 아니라 외부 경로도 일부 허용하되, 사용자의 주의를 유도하여 **피싱이나 사칭 피해를 방지**할 수 있다. 다만, 경고만으로 모든 위험을 완전히 차단할 수는 없으며 보안에 민감한 서비스에서는 가능한 한 **외부 URL 리다이렉트를 아예 막는 것이 더 안전**하다.

```js
function isExternalUrl(url) {
	return /^https?:\/\//.test(url); // 절대경로인지 확인
}

const target = new URLSearchParams(location.search).get('redirect');

if (isExternalUrl(target)) {
	alert('외부 사이트로 이동합니다. 주소를 확인하세요.');
} else {
	window.location.href = target;
}
```

---

## 마무리

이러한 보안 위협은 프론트엔드 코드 한 줄, 조건문 한 줄에서 발생할 수 있으므로 의도를 정확히 파악하고 사용자 입력을 검증하며 항상 방어적인 코딩을 염두에 두는 것이 중요하다.

해당 도서를 통해 다양한 보안 위협에 대해 알게 되면서, 앞으로 개발을 진행할 때 어떤 부분을 유의해서 코드를 작성해야 할지 알 수 있게 되어 좋았다. 당장 쉽게 적용할 수 있는게 오픈 리다이렉트를 방지하기 위해 화이트리스트를 작성하는 방법 등..!

## 📚 참고

- 도서: 프런트엔드 개발을 위한 보안입문
- [MDN Web Docs: X-Frame-Options](https://developer.mozilla.org/ko/docs/Web/HTTP/Reference/Headers/X-Frame-Options)
- [MDN Web Docs: Set-Cookie](https://developer.mozilla.org/ko/docs/Web/HTTP/Reference/Headers/Set-Cookie)
