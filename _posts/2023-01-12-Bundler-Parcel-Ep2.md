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

## __CLI__
---

구글에 parcel 검색 후, parcel 한국어판에 접속하면 해당 내용을 확인할 수 있다.

[🖥 커맨드 라인 인터페이스(CLI) Link](https://ko.parceljs.org/cli.html)

**CLI (Comman Line Interface)**

대표적으로 터미널에서 어떤 글자에 해당하는 명령어로 여러 내용들을 실행한다. npm i로 패키지 설치라던가 git으로 버전관리라던가. 이런 것들을 CLI라고 부른다.

위 문서는 parcel bundler를 통해서 CLI로 할 수 있는 여러가지 명령어들을 정리해둔 것이다.

### __명령어__
- Serve
    
  개발용 명령어이다. 개발용 서버를 시작한다.
  
  ```bash
  parcel index.html
  ```
    
- Build
  
  제품용 명령어이다.
  
  ```bash
  parcel build index.html
  ```
    

### __옵션__

- 결과물 디렉토리
  
  기본값은 dist 폴더이다.  
  dist 폴더 이름을 다른 이름으로 변경하고 싶다면 아래와 같이 명령어를 실행한다.
  
  ```bash
  parcel build entry.js --out-dir build/output
  
  # entry.js == index.html
  # build/output => dist를 대신하는 폴더 구조. ouput이 변경한 이름
  ```
    
- 포트 번호
  
  기본값은 1234이다.  
  포트 번호를 변경하고 싶다면 serve 명령어를 통해 변경한다.
  
  ```bash
  parcel serve entry.js --port 1111
  
  ### 1111 -> 변경하고자 하는 포트 번호
  ```
    
- 브라우저에서 열기
  
  해당 옵션은 npm run dev를 실행했을 때 직접 브라우저에서 포트번호로 이동을 해줘야 했다. 해당 옵션을 사용하면 우리가 따로 열어줄 필요없이 자동으로 브라우저가 열린다.
  
  기본값을 비활성 되어있다.
  
  ```bash
  parcel entry.js --open
  ```
    
- 빠른 모듈 교체의 비활성화
  
  기본값은 HMR 활성화이다.
  
  HMR은 빠른 모듈 교체(Hot Module Replacement)로, 런타임에 페이지 새로고침 없이 수정된 내용을 자동으로 갱신하는 방식을 말한다.  
  실제 프로젝트에서 어떤 코드를 고치고 저장하면 바로 브라우저에 반영이 되는데 이게 HMR이다.  
  해당 옵션을 비활성화 하고 싶다면 다음과 같이 명령어를 작성한다.
  
  ```bash
  parcel entry.js --no-hmr
  ```
    
- 파일시스템 캐시 비활성화
    
  기본값을 캐시 활성이다.
  
  캐시활성이 되어 있어서 우리가 개발서버를 열거나 제품화를 시킬때 이미 캐시가 되어져 있는 내용때문에 훨씬 빠르게 내용이 처리될 수 있다.  
  다만, 가끔씩 캐시 활성으로 인해 문제가 발생하는 경우가 있다. 그럴때 해당 옵션을 사용하여 파일시스템 캐시를 비활성화 시켜준다.  
  
  캐시 비활성화로 속도는 저하될 수 있지만 매번 새로운 내용으로 결과를 만들어 낼 수 있다.
  
  ```bash
  parcel build entry.js --no-cache
  ```
    

### __옵션 변경 적용해보기__

- 포트 번호 바꾸기
  
  package.json 파일
  
  ```json
  "scripts": {
      "dev": "parcel index.html --port 9876",
      "build": "parcel build index.html"
    },
  ```
    
- 주의!
  
  구성옵션 변경은 꼭 필요에 따라서만 사용하는 것이 좋다.  
  parcel bundler는 개발자가 따로 구성옵션을 제공하지 않고 자동으로 기본적인 내용을 동작 시키는 것이 핵심이기 때문에 필요하지 않다면 구성 옵션 변경은 자제하자.