---
layout: post
categories:
  - TIL
title: "Bundler: Parcel Bundler Ep.2"
tags:
  - TIL
  - Bundler
  - Parcel
---
## __babel__
---

자바스크립트의 표준은 ECMA Script라고 하며 간단하게 ES라고 부른다.
이게 2016년에 새로 나왔는데 ES6라고 부른다. 2015년을 기준으로 이전에 사용했던 것은 ES5 혹은 ES4이다.   
우리가 요즘 사용하는 것들은  ES6, ES7, ES8의 최신의 문법들이다.
이런 최신의 문법들은 구형 브라우저에서 동작하지 않을 수 있다.

그래서 우리는 최신의 자바스크립트 코드를 babel을 통해서 구형 브라우저에서도 동작할 수 있는 ES5로 변환 시켜줄 수 있다.  
변환을 **컴파일**이라고도 부르며 babel을 **컴파일러**라고 부른다.

- babel 설치하기
  
  ```bash
  # 두가지 패키지 설치
  npm i -D @babel/core @babel/preset-env
  ```
    
- .babelrc.js 파일 생성
  
  구형 브라우저로 변환하는 코드 작성
  
  ```jsx
  module.exports = {
    presets: ["@babel/preset-env"]
  }
  ```
  
  이제부터 우리가 작성하는 자바스크립트는 babel을 통해서 ES5의 문법으로 변환이 되어 구형 브라우저에서도 동작을 하게 된다.
    
- package.json, browserslist 옵션 추가하기
  
  ```json
  "browserslist": [
      "> 1%",
      "last 2 versions"
    ]
  ```
  
  기존에 postcss를 적용한 적이 있다면, 해당 옵션이 잘 적혀있는지 확인만 하고 넘어가면 된다.  
  작성한 적이 없다면 해당 내용 추가 작성을 해준다.
    
- 오류 없이 잘 작동하는지 확인하기 npm run dev

### __비동기 문법 동작하게 하기__

- 비동기 함수 만들기
  
  함수 앞에 async를 붙여 비동기 함수로 만들어준다.
  
  async를 통해서 test라는 이름의 비동기 함수를 만들었고 내부에서 await 키워드를 통해 실제로 무엇인가를 기다리는(promise) 내용을 만들었다.
    
- regeneratorRuntime is not defined 에러가 발생했다면
  
  ```bash
  npm i -D @babel/plugin-transform-runtime
  ```
  
  - .babelrc.js
    
    ```jsx
    module.exports = {
      presets: ["@babel/preset-env"],
      plugins: [
        ["@babel/plugin-transform-runtime"]
      ]
    }
    ```
    해당 플러그인을 통해서 async, await가 동작하게 된다.
