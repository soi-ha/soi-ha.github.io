---
layout: post
categories:
  - TIL
title: "React: React Router"
tags:
  - TIL
  - React
---

## **React 라우팅**

> [React Router: Home v6.9.0](https://reactrouter.com/en/main)

### **SPA 라우팅 과정**

- 브라우저에서 최초에 ‘/’ 경로로 요청
- React Web App을 내려줌
- 내려받은 React App에서 ‘/’경로에 맞는 컴포넌트를 보여줌
- React App에서 다른 페이지로 이동하는 동작 수행
- 새로운 경로에 맞는 컴포넌트를 보여줌

### **React Router 설치**

```bash
npm i react-router-dom
```

- CRA에 긱본 내장된 패키지가 아니다.
- react-router-dom은 페이스북 공식 패키지가 아니지만 가장 대표적인 라우팅 패키지이다.

### **Router Component**

- `<BrowserRouter>`

  브라우저 History API를 사용해 현재 위치의 URL을 저장해주는 역할이다.  
  웹에 HTML5 History API를 사용하여 페이지를 새로고침 하지 않아도 주소를 변경하고, 현재 주소에 관련된 정보를 props로 쉽게 조회하거나 사용할 수 있도록 한다.

  <Route>와 <Link> 컴포넌트가 함께 유기적으로 동작할 수 있도록 묶어주는데 사용한다.

- `<Route>`

  컴포넌트의 속성에 설정된 URL과 현재 경로가 일치하면 해당하는 컴포넌트, 함수를 렌더링 한다.

  path를 통해 URL을 분기시킨다. 중첩이 가능하다.  
  exact는 path 속성에 넣은 경로값이 **정확히** 일치해야 랜더링되도록 한다. exact를 사용하지 않으면 “/profile”경로가 “/”경로에도 반응하게 된다.

- `<Link>`

  HTML의 `<a>`태그와 유사한 기능을 한다.  
  to 속성에 설정된 링크로 이동한다.

  react router에서는 `<a>`태그를 사용할 수 없기에 해당 컴포넌트를 사용한다. `<a>`태그는 모두 새로 가져오기 때문에 해당 태그는 사용할 수가 없다.

  Link를 통해 페이지를 전환하면 새로운 페이지를 부르지 않아도 애플리케이션은 그대로 유지한 채, HTML5 History API를 사용하여 페이지의 주소만 변경한다.

- `<NavLink>`
  activeClassName, activeStyle 처럼 active 상태에 대한 스타일 지정이 가능하다.  
  Route의 path처럼 동작하기 때문에 exact가 있다.
- `<Switch>`

  자식 컴포넌트 <route> 또는 <redirect>중 매치되는 첫번째 요소를 렌더링한다. 작성할때 순서는 좁은 범위 ~ 넓은 범위로 작성한다. (순서가 중요하니 유의하도록 하자)  
  Switch를 사용하면 BrowserRouter만 사용할 때와 다르게 **하나**의 매칭되는 요소만 렌더링한다.  
  Switch로 감싸고 있기 때문에 Route가 중복되는 것이 있으면 첫번째 요소만 렌더링한다.  
  가장 마지막에 어느 path에도 맞지 않으면 보여지는 컴포넌트를 설정해서 “Not Found” 페이지를 만들 수 있다.

- `<Redirect>`

  `<Redirect>`가 렌더링되면 to가 가리키는 링크로 바로 연결된다.

### **Router 적용 순서**

BrowserRouter 연결 → 페이지 컴포넌트 만들기 → Route path 연결 → Link 컴포넌트 연결

> 참고 자료 링크  
> [react-router-dom 설치, 라우팅하기](https://goddino.tistory.com/160)  
> [리액트 라우터 기본 사용방법](https://jinyisland.kr/post/react-router/)  
> [react-router-dom 시작하기](https://velog.io/@kwonh/React-react-router-dom-시작하기)
