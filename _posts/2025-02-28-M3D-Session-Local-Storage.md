---
layout: post
published: true
categories:
  - TIL
title: '매3개 | 브라우저 저장소 (Session Storage, Local Storage, Cookie)'
tags:
  - TIL
  - 매3개
---

## 📄 Session Storage

**Session Storage(세션 스토리지)** 는 탭(세션) 단위로 데이터를 저장하는 저장소이다. 브라우저 탭을 닫으면 데이터가 사라진다.

- 데이터 지속성: **브라우저 탭을 닫으면 삭제.** (세션 동안만 유지)
- 저장 용량: 약 5MB
- 도메인 단위 저장: 같은 도메인 내에서만 접근 가능하지만, **탭별로 구분됨.**
- 보안: 서버와 자동으로 주고받지 않음.
- 사용 예시: 임시 로그인 정보, 폼 데이터 임시 저장 (탭을 닫으면 사라지는 데이터)

### ⚒️ 예제 코드

```js
// 데이터 저장
sessionStorage.setItem('sessionKey', '123456');

// 데이터 가져오기
const sessionData = sessionStorage.getItem('sessionKey');
console.log(sessionData); // "123456"

// 데이터 삭제
sessionStorage.removeItem('sessionKey');

// 전체 삭제
sessionStorage.clear();
```

---

## 🏠 Local Storage

**Local Storage(로컬 스토리지)** 는 브라우저에 데이터를 영구적으로 저장하는 저장소이다. 사용자가 브라우저를 닫고 다시 열어도 데이터가 유지된다.

- 데이터 지속성: 브라우저를 닫아도 유지. (영구 저장)
- 저장 용량: 약 5MB
- 도메인 단위 저장: 같은 도메인 내에서는 모든 페이지에서 접근 가능, **탭 별로 공유 가능.**
- 보안: 서버와 자동으로 주고받지 않음. → 보안이 비교적 높음
- 사용 예시: 사용자의 다크 모드 설정, 자동 로그인 유지

### ⚒️ 예제 코드

```js
// 데이터 저장
localStorage.setItem('username', 'soha');

// 데이터 가져오기
const username = localStorage.getItem('username');
console.log(username); // "soha"

// 데이터 삭제
localStorage.removeItem('username');

// 전체 삭제
localStorage.clear();
```

---

## 🍪 Cookie

**Cookie(쿠키)** 는 작은 데이터를 클라이언트에 저장하고 서버와 자동으로 주고받을 수 있는 저장소이다. 주로 사용자 인증과 세션 관리를 위해 사용된다.

- 데이터 지속성: 유효기간 설정 가능. (기본적으로 브라우저 종료 시 삭제)
- 저장 용량: 약 4KB (가장 작음)
- 도메인 단위 저장: 특정 도메인 및 경로에서만 접근 가능, **탭 별로 공유 가능.**
- 보안: 기본적으로 HTTP 요청마다 서버와 자동으로 주고받음. → 보안 설정이 중요함 (`Secure`, `HttpOnly`, `SameSite` 등 옵션 활용)
- 사용 예시: 세션 유지, 사용자 인증 (JWT 토큰), 방문 기록

### ⚒️ 예제 코드 (프론트엔드에서 쿠키 저장)

```js
// 쿠키 저장 (expires로 유효기간 설정 가능)
document.cookie = 'username=soha; expires=Fri, 31 Dec 2025 23:59:59 GMT; path=/';

// 쿠키 가져오기
console.log(document.cookie); // "username=soha"

// 쿠키 삭제 (유효기간을 과거로 설정)
document.cookie = 'username=; expires=Thu, 01 Jan 1970 00:00:00 GMT; path=/';
```

이 쿠키는 `example.com(현재 실행한 도메인)` 내 모든 페이지에서 접근 가능하다. 다른 탭에서 `document.cookie`를 실행해도 같은 `username=soha` 값을 얻을 수 있다.

`expires(유효 일자)`나 `max-age(만료 기간)` 옵션이 지정되어있지 않으면 브라우저가 닫힐 때 쿠키도 함께 삭제된다. 이런 쿠키를 **세션 쿠키(session cookie)** 라고 한다.

⛔️ **도메인 제한 (domain 속성)**

- **서브 도메인까지 공유하기**

  ```js
  document.cookie = 'user=soha; domain=example.com';
  ```

  `example.com`뿐만 아니라 `sub.example.com`에서도 이 쿠키를 사용할 수 있다.

- **서브도메인에서는 쿠키를 못 쓰게 하기**

  ```js
  document.cookie = 'user=soha; domain=example.com';
  // 혹은
  document.cookie = 'user=soha;'; // 실행한 도메인(example.com)에서만 사용 가능
  ```

  `example.com`에서만 쿠키를 사용할 수 있고, `sub.example.com`에서는 사용할 수 없다.

⛔️ **경로 제한 (path 속성)**

- **`/admin` 경로에서만 쿠키 사용하기**

  ```js
  document.cookie = 'role=admin; path=/admin';
  ```

  이 쿠키는 `https://example.com/admin` 페이지에서만 접근 가능하고,
  `https://example.com/user`에서는 `document.cookie`로 가져올 수 없다.

- **`/(루트)`에서 설정하면 모든 경로에서 사용 가능**

  ```js
  document.cookie = 'theme=dark; path=/';
  ```

  `example.com`의 모든 경로에서 쿠키를 사용할 수 있다.

⛔️ **https 통신 제한 (secure 속성)**

- **https로 통신하는 경우에만 쿠키 전송**

  ```js
  // (https:// 로 통신하고 있다고 가정 중)
  document.cookie = 'user=soha; secure';
  ```

  `secure` 옵션을 설정한 경우 `https://example.com`에서 설정한 쿠키는` http://example.com`에서 접근할 수 없다. 쿠키에 민감한 내용이 저장되어 암호화되지 않은 HTTP 연결을 통해 전달되는 걸 원치 않을 경우 사용하면 좋다.

- **프로토콜 상관 없이 쿠키 전송**

  ```js
  document.cookie = 'user=soha;';
  ```

  `secure` 옵션이 없으면 `http://example.com`에서 생성한 쿠키를 `https://example.com`에서 읽을 수 있다. 즉, 서로 쿠키를 공유하게 된다. 쿠키는 도메인만 확인하고 프로토콜을 따지지 않기 때문이다.

````
### 🚨 주의

쿠키 저장은 프론트엔드에서도 가능하고, 백엔드에서도 가능하다.
하지만 **보안이 중요한 쿠키 (예: 로그인 토큰) 는 보통 백엔드에서 설정**하는게 안전하다. 브라우저에서 `document.cookie`로 쉽게 접근할 수 있기 때문에, **악성 스크립트(XSS 공격)에 노출될 위험이 있기 때문**이다.

### ⚒️ 예제 코드 (백엔드에서 쿠키 저장 예제 / Express.js)

백엔드에서는 응답 헤더에 Set-Cookie를 추가해서 쿠키를 저장한다.

```js
res.cookie('token', 'secureToken123', {
	httpOnly: true,
	secure: true,
	sameSite: 'Strict',
	maxAge: 1000 * 60 * 60 * 24, // 1일 동안 유지
});
````

- `httpOnly: true` → JS에서 접근 불가능 (XSS 공격 방지)
- `secure: true` → HTTPS에서만 쿠키 전송
- `sameSite: "Strict"` → CSRF 공격 방지
- `maxAge` → 쿠키 유효 시간 설정 (밀리초 단위)

백엔드에서 이렇게 설정하면 클라이언트에서는 `document.cookie`로 접근할 수 없고, HTTP 요청 시 자동으로 쿠키가 전송된다.

---

## 📝 정리

| 특징                  | Session Storage                | Local Storage               | Cookie                           |
| --------------------- | ------------------------------ | --------------------------- | -------------------------------- |
| **데이터 지속성**     | 탭을 닫으면 삭제               | 브라우저 닫아도 유지        | 설정한 유효기간까지 유지         |
| **저장 용량**         | 약 5MB                         | 약 5MB                      | 약 4KB (가장 작음)               |
| **브라우저에서 접근** | `sessionStorage` API 사용      | `localStorage` API 사용     | `document.cookie` 사용           |
| **서버와의 통신**     | 서버로 전송되지 않음           | 서버로 전송되지 않음        | HTTP 요청 시 자동 전송 가능      |
| **보안**              | 비교적 안전                    | 비교적 안전                 | `HttpOnly`, `Secure` 설정 필요   |
| **사용 예시**         | 임시 데이터 저장, 입력 폼 보존 | 사용자 설정 저장, 다크 모드 | 로그인 세션 유지, 인증 토큰 저장 |

### 🤔 어떤 걸 써야 할까?

- **탭을 닫으면 사라져야 하는 데이터** → `Session Storage`
- **장기적인 데이터 저장** → `Local Storage`
- **서버와 자동으로 데이터를 주고받아야 하는 경우** → `Cookie`

## 📚 참고

- [localStorage와 sessionStorage](https://ko.javascript.info/localstorage)
- [쿠키와 document.cookie](https://ko.javascript.info/cookie)
- [기술면접: 로컬스토리지, 세션스토리지, 쿠키](https://velog.io/@abcwockd95/%EA%B8%B0%EC%88%A0%EB%A9%B4%EC%A0%91-%EB%A1%9C%EC%BB%AC%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%84%B8%EC%85%98%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%BF%A0%ED%82%A4)
- [쿠키 vs 로컬스토리지: 차이점은 무엇일까?](https://erwinousy.medium.com/%EC%BF%A0%ED%82%A4-vs-%EB%A1%9C%EC%BB%AC%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-28b8db2ca7b2)
