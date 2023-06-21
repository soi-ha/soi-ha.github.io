---
layout: post
categories:
  - TIL
title: '기본 CSS 속성 초기화를 NPM으로: styled-reset'
tags:
  - TIL
---

내가 styled-reset을 알기 전까지는 cdn 링크나 css 코드로 직접 속성을 초기화했었다. 하지만 이번에 react를 공부하면서 간단하게 할 수 있는 방법이 없을까? 찾던 중에 `styled-reset`을 알게되었다.  
해당 방법을 통해 쓸모없는 코드 길이를 대폭 줄일 수 있었다.

## **styled-rest**

---

- 설치

  `npm i styled-reset` 명령어를 작성하여 styled-reset을 설치

- 적용

  App.js 파일에 다음과 같이 작성하여 사용한다.

  ```jsx
  import { Reset } from 'styled-reset';

  const App = () => (
  	<React.Fragment>
  		<Reset />
  		<div>Hello, Soha!</div>
  	</React.Fragment>
  );
  ```

  import를 통해 Reset을 불러오고, 작성할 내용들의 상단 (즉, 루트 태그 바로 아래)에 Reset을 불러오면 된다.

> [styled-reset 공식 링크](https://www.npmjs.com/package/styled-reset)
