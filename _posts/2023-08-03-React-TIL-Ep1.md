---
layout: post
categories:
  - TIL
title: 'React 기본: React & Virtual DOM'
tags:
  - TIL
  - React
---

## React?

사용자 인터페이스(보이는 것)를 만들기 위한 **라이브러리**이다.

### 프레임워크 vs 라이브러리

---

- 프레임워크

  Ex) Vue.js, Angular  
  어떠한 앱을 만들기 위해 필요한 대부분의 것을 가지고 있다.  
  즉, 프레임워크는 라이브러리를 포함한다. (프레임워크 안에는 많은 라이브러리가 존재)  
  프레임워크는 라이브러리를 포함하고 작성한 소스코드를 호출한다.  
  소스코드는 어떤 기능을 구현하기 위해서 라이브러리를 호출하게 된다.

- 라이브러리

  Ex) React.js  
  어떠한 특정 기능을 모듈화 한 것이다.

### React가 프레임워크가 아닌 라이브러리인 이유

---

React는 전적으로 UI를 랜더링하는데 관여하기 때문이다. (View, Model, Controller 중에서 View만 담당)

**React 생태계**

- 라우팅(페이지 이동): react-router-dom
- 상태관리: redux, recoil, mobx
- 테스트: Jest, Mocha

React는 UI 담당이다. 화면을 바꾸는 라우팅은 react-router-dom 모듈을 사용하며, 상태 관리는 redux, mobx 등 여러 모듈을 사용한다. 빌드를 위해서는 webpack, npm 등 과 테스팅을 위해서는 Eslint, Mocha 등을 이용하기 때문에 React는 프레임워크가 아닌 라이브러리이다.

### 브라우저가 그려지는 원리 및 가상돔

---

React의 주요 특징 중 하나는 가상DOM을 사용한다는 것이다.  
가상DOM이 무엇인지 알기에 앞서서 왜 사용해야 하는지 **브라우저가 렌더링하는 과정을** 살펴보자.

**웹 페이지 빌드 과정(Critical Rendering Path, CRP)**

CRP는 웹 브라우저가 서버로부터 HTML 문서를 받아서 읽고, 스타일을 입히고 뷰포트에 표시하는 과정이다.

브라우저가 서버에서 페이지에 대한 HTML 응답을 받고 화면에 표시하기 전까지 여러 단계가 존재한다. 단계는 아래 그림과 같다.

<img alt="CRP 이미지" src="https://dimension85.com/images/critical-render-path-large.jpg">

- DOM tree 생성

  렌더 엔진이 문서를 읽어들여서 파싱하고 어떤 내용을 페이지에 렌더링할 지 결정한다. (문서를 읽어서 DOM tree 생성)

- Render tree 생성

  브라우저가 DOM과 CSSOM을 결합하는 곳이며, 화면에 보이는 모든 콘텐츠의 콘텐츠와 스타일 정보를 모두 포함하는 최종 렌더링 트리를 출력한다. 화면에 표시되는 모든 노드의 콘텐츠 및 스타일 정보를 포함한다.

- Layout

  브라우저가 페이지에 표시되는 각 요소의 크기와 위치를 계산한다.

- Paint

  실제 화면에 그린다.

CRP에는 문제가 존재하는데, 어떤 인터렉션에 의해서 DOM에 변화가 발생하면 매번 Render Tree가 재생성된다. 즉, 모든 요소의 스타일을 다시 계산하고 Layout하고 Repaint 하는 과정을 다시 한다.

인터렉션이 적은 웹이라면 괜찮겠지만 엄청나게 많다면? 불필요하게 DOM을 조작하게 되기에 비용이 너무 커진다.

이러한 문제를 해결하기 위해서 나온 것이 **가상DOM(Virtual DOM)**이다.

### 가상DOM(Virtual DOM)

---

가상DOM? 실제 DOM을 메모리에 복사해준 것이다.

<img alt="Virtul DOM 이미지" src="https://blog.kakaocdn.net/dn/usvvn/btqDMG4hRaF/FCh9fauyvaEziMci906GXK/img.png">

데이터가 바뀌면 가상DOM이 렌더링되고 이전에 생긴 가상DOM과 비교해서 바뀐 부분만 실제 DOM에 적용시켜준다. 바뀐 부분을 찾는 과정을 Diffing이라고 부르며, 바뀐 부분만 실제 DOm에 적용시켜주는 것을 재조정(reconciliation)이라고 부른다.

가상DOM은 요소가 30개가 변하더라도 한번에 묶어서 실제 DOM에 한번만 수정을 하기 때문에 DOM을 조작하는 비용이 줄어들게 된다.
