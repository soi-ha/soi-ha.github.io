---
layout: post
categories:
  - TIL
title: 'UMC: 마켓컬리 클론코딩, 카카오 소셜로그인 구현'
tags:
  - TIL
  - UMC
---

## **카카오 소셜로그인 구현하기**

---

### 1. 인가코드 받기 및 플랫폼 추가

kakao develpoers에서 인가 코드를 받을 수 있으며 플랫폼을 추가할 수 있다.

> [kakao developers 링크](https://developers.kakao.com/docs/latest/ko/kakaosync/design-guide)

### 2. Redirect URI 설정

로그인 성공시 Redirect할 URI의 주소를 작성한다.

<img width="896" alt="스크린샷 2023-05-18 01 22 13" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/a218727b-4689-42f2-be24-92abf0af0d4d">

### 3. 인가코드 받기

인가코드는 Redirect URI를 통해서 받을 수 있다.  
KAKAO_AUTH_URL의 경로로 요청을 보내면, Redirect URI로 이동하면서 인가 코드를 발급 받는다.

이때, client_id, redirect_uri, response_type이 필수로 담겨야 한다.
client_id는 REST API키, redirect_uri에는 **2. Redirect URI 설정**에서 작성한 URI, reponse_type에는 code 고정 값을 담으면 된다.

<img width="1033" alt="스크린샷 2023-05-18 01 06 38" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/8ecdef86-4fff-4ec8-915f-04ddc906143a">

Key는 공개되지 않는 것이 보안에 좋으므로 따로 파일을 생성하여 관리한다. 해당 파일은 `.gitignore`에 작성하여 깃허브에 업로드되지 않도록 한다.

<img width="587" alt="스크린샷 2023-05-18 01 03 51" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/db150b2d-e686-4117-86d4-4f68bbda5fba">

카카오로 시작하기 버튼을 클릭하면 지정한 Redirect URI로 이동하여 인가코드를 얻을 수 있다.

### 4. Redirect 주소 Router 설정

Redirect URI에 작성했던 successlogin으로 이동할 수 있도록 라우터를 설정해준다. 라우터를 설정하지 않으면 페이지가 존재하지 않아서 이동할 수 없다.

<img width="437" alt="스크린샷 2023-05-18 01 06 08" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/8f2282c8-79c7-44e0-8aa4-fc80a3d9a89f">

### 5. 사용자 토큰 받기

버튼을 클릭하면 `localhost:3000/successogin?code=-4Wr…`로 이동하는데 -4Wr…부분은 카카오에서 제공한 인가코드이다. 해당 인가코드는 매번 변경된다. 따라서 인가코드를 사용하여 토큰을 가져온다.

토큰은 POST 방식으로 카카오에서 얻을 수 있다. 파라미터에 인가 코드가 필요한데, 이 인가 코드를 사용하기 위해서 URL에 담겨있는 뒷부분을 추출한다.

```jsx
const location = useLocation();
const KAKAO_CODE = location.search.split('=')[1];
```

fetch 함수를 이용하여 POST 방식으로 요청을 보내 localstorage에 토큰을 저장한다.

토큰을 정상적으로 잘 받아서 로그인이 되었다면 자동으로 Redirect로 설정한 페이지로 이동한다.

로그인에 실패했다면 else 부분의 `navigate(’주소’)`로 이동한다.

<img width="919" alt="스크린샷 2023-05-18 01 26 58" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/0b8dd2bb-552b-42df-b5e7-944f3192f5d8">

### 4. 여러 페이지 생성

- 로그인 실패 페이지

  <img width="426" alt="스크린샷 2023-05-18 01 05 28" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/befa2568-a7c6-4a41-a730-3ac9dcd8af2a">

- 로그인 성공 페이지

  <img width="432" alt="스크린샷 2023-05-18 01 05 48" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/7147faf4-c525-4f33-9c71-3a64390fd279">

### 5. 실제 구현된 페이지

<img width="1509" alt="스크린샷 2023-05-24 15 14 43" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/6efc6654-5402-4c6e-af9f-5da055fb47c9">

_로그인 페이지_

<img width="1510" alt="스크린샷 2023-05-24 15 15 03" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/e49d8fb1-24b0-499f-b0f9-84653be3ddeb">

_카카오 소셜 로그인_

<img width="1509" alt="스크린샷 2023-05-24 15 16 00" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/b08c5ba7-913d-478a-8288-b251b08dbef5">

_카카오 소셜 로그인 정보 제공 동의_

<img width="1496" alt="스크린샷 2023-05-24 15 16 21" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/32d725d0-a60e-4ecf-be04-56a1c7fe734f">

_로그인 성공 페이지_

<img width="1501" alt="스크린샷 2023-05-24 15 16 51" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/3f80d981-d12e-4857-8555-c621aaa6febe">

_로그인 실패 페이지_

## **트러블 슈팅**

---

#### **localStorage에 토큰 저장 안됨**

- 문제

  localStorage에 토큰이 저장 안된다…  
  에러 코드도 따로 발생하지 않아서 어떤게 원인인지는 잘 모르겠다…
  ERR_SSL_PROTOCOL_ERROR를 검색했을때, 크롬 문제일 수도 있다고 해서 사파리로 접속했으나 똑같이 에러 발생. 모바일로 접속해도 똑같이 에러 발생..  
  캐시 삭제 및 강한 새로고침을 해도 똑같이 에러가 발생한다.  
  KakaoLoginData.js에 담아둔 값들도 정상적으로 넘어온다. 코드도 참고 자료를 바탕으로 작성했으나, 토큰이 localStorage에 저장이 안된다.

- 해결

  `REDIRECT_URI`의 주소를 https로 설정해둬서 안됐다.. localhost라 http로 했어야 했는데..ㅋ

## **종합소감**

카카오에서 REST API KEY 발급받아 카카오 소셜 로그인을 구현해보았다. 이전에, MOA WEBTOON 프로젝트를 진행했을 때, 네이버와 카카오 소셜 로그인을 구현했었다. 하지만, 나는 그저 버튼만 만들었을 뿐 실제로 기능 구현은 백엔드 분이 했었다. 그때 옆에서 보았던 것을 되새기고 참고 자료의 글을 읽고 공부하면서 구현해 보았다.  
간단하게 생각했지만.. 전혀 간단하지만은 않았다 ^^.. 물론 다 하고 느낀 것은 내가 능숙해지면 이건 쉽게 할 수 있을 것이다! 라는 건 느껴졌다. 다음 번에는 더욱 빠르게 구현 할 수 있을 것 같다!

> **참고자료**  
> [카카오 소셜로그인 프론트엔드 (리액트) 파트](https://velog.io/@milmilkim/카카오-소셜로그인-프론트엔드-리액트-파트)  
> [Kakao Developers](https://developers.kakao.com/docs/latest/ko/kakaosync/design-guide)  
> [[React] 카카오 소셜 로그인 구현하기](https://hymndev.tistory.com/72)
