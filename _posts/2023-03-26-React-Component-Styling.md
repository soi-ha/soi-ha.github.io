---
layout: post
categories:
  - TIL
title: "React: classNames, styled-component, react-shadow, ant-design "
tags:
  - TIL
  - React
---

## **classNames**

---

복잡하게 작성해야 했던 것을 해당 라이브러리 설치만으로 간단하게 만들 수 있다.

```bash
npm i classnames
```

> 참고 자료  
> [React - classnames](https://velog.io/@jinhengxi/React-classnames)  
> [리액트에서 조건부 스타일을 줄때 classNames 라이브러리를 활용해보자!](https://velog.io/@dooreplay/classNamesCSS-Modules)

- css-module **bind**

  [classnames/bind 함수와 classnames 함수의 차이](https://uncertainty.oopy.io/43ef0ac4-cddc-460d-90d8-6a418f5f42ab)

## **Styled Components**

---

동일한 컴포넌트에서 컴포넌트 이름을 쓰듯 스타일을 지정하는 것을 styled-components라고 부른다.  
css 파일을 밖에 두지 않고, 컴포넌트 내부에 넣기 때문에 css가 전역으로 중첩되지 않도록 만들어준다는 장점이 있다.

- styled component 설치

  ```bash
  npm i styled-components
  ```

- 사용하기

  `const 컴포넌트명 = styled.태그명`스타일``

  만들고자하는 컴포넌트의 render 함수 밖에서 만든다.

  ```jsx
  import styled from "styled-components";

  const StyledButton = styled.button`
    // 여기에 원하는 css 작성
  `;

  export default StyledButton;
  ```

  스타일 컴포넌트가 자동으로 class를 만들어준다.

  단점으로는 css 문법 에러가 발생했을 때, 틀렸다는 것을 알려주지 않는다. 문자열이기 때문이다..! 그렇기에 추가적인 플러그인을 설치해야 한다.

> [styled-components 개념 및 예시, react, redux](https://kyounghwan01.github.io/blog/React/styled-components/basic/#예시)

## **React Shadow**

---

HTML에서 shadow DOM을 이용하여 오염될 수 없도록 만들어준다.  
HTML안에 본래의 HTML과 영향을 주지 않는 별도의 HTML 덩어리를 의미한다. 리액트에서 컴포넌트를 사용할 때, 컴포넌트가 스타일이 오염되는 문제가 있다. 이런 문제를 해결하고자 css 모듈, 스타일 컴포넌트가 나왔다.  
shadow DOM은 HTML에서 shadow DOM을 이용하여 오염될 수 없도록 만들어준다.

- 설치

  react-shadow 라이브러리 설치

  ```bash
  npm i react-shadow
  ```

- `<root.div>` 안의 태그들만 별도의 스타일 속성을 지정한다.
- **단점**

  공통적인 스타일인 경우에도 반복적으로 추가해야 하는 번거로움이 있다.

  외부와 내부가 차단되어 있어 document에 지정되어 있는 값을 상대적으로 표현할 때 제약이 발생한다.

> [React : react-shadow 사용법 , style 독립적으로 넣고 싶을때, App.js 와 index.css 스타일 분리](https://velog.io/@whdms3368/React-react-shadow-사용법-style-독립적으로-넣고-싶을때-App.js-와-index.css-스타일-분리)

## **Ant Design**

---

ant design은 하나의 컴포넌트 모음이며, 이미 디자인된 라이브러리이다.

> [Ant Design](https://ant.design/docs/spec/overview)

- 설치

  ```bash
  npm i antd
  ```

- 사용방법

  사용하고자 하는 컴포넌트를 Ant Design 페이지에서 찾아, 컴포넌트를 사용한다.

  ```jsx
  import { Calendar } from "antd";

  function App() {
    return <Calendar />;
  }
  ```

- Ant Design Icon 사용하기
  - 설치
    ```bash
    npm i --save @ant-design/icons
    ```
  - 사용

    홈페이지에서 사용하고자 하는 아이콘을 클릭하면 컴포넌트가 복사된다. 해당 컴포넌트를 사용하고 싶은 곳에 넣어주면 끝

    ```jsx
    import { GithubOutlined } from "@ant-design/icons";

    function App() {
      return <GithubOutlined />;
    }
    ```
