---
layout: post
categories:
  - TIL
title: "BootStrap: CSS 프레임워크 Ep.2"
tags:
  - TIL
  - Bootstrap
  - CSS
  - Framework
---
## __NPM 프로젝트 생성__
---
BootStrap을 NPM 프로젝트에서 관리하기

기존에는 link 태그와 script 태그로 손쉽게 bootstrap을 가져왔다. 하지만 해당 방법은 약간의 기능 제한이 존재한다.  
외부에서 완성되어져 있는 파일만 연결하는 것이기 때문에 필요에 따라서 세부적으로 정리를 하는 것이 불가능하다.

그렇기에 우리는 NPM으로 관리를 할 것이다.

```bash
$ npm init -y
$ npm i -D parcel-bundler
```

~ **package.json 파일** ~

```json
"scripts": {
    "dev": "parcel index.html",
    "build": "parcel build index.html"
  },
```

```bash
npm install bootstrap@5.3.0-alpha1
```

해당 부트스트랩은 개발할때만 사용하는 것이 아니고 실제 브라우저에도 동작해야 하기 때문에 일반 의존성 모드로 설치해야 한다.

~ **main.js** ~

```jsx
*import* bootstrap *from* 'bootstrap/dist/js/bootstrap.bundle'
```

bootstrap의 dist 폴더에서 js 폴더 선택 후 bootstrap.bundle 파일을 선택하면 popper JS 패키지까지 합쳐서 번들로 프로젝트에 적용이 된다.

~ **main.scss** ~

```scss
*@import* "../node_modules/bootstrap/scss/bootstrap.scss";
@import "../node_modules/bootstrap/scss/bootstrap";
```

두 코드중에 하나를 선택해서 사용하면 된다. (scss 확장자 입력 유무차이 / scss는 확장자를 적지 않아도 동작한다.)

### **js파일과 scss(css)파일의 차이점**

main.js 에서는 node_modules에 설치되어져 있는 bootstrap 폴더로 바로 접근이 가능하다.  
그러나, scss(css)는 바로 node_modules 폴더로 접근이 불가능하기 때문에 상대경로로 정확하게 node_modules 먼저 찾아주고 안에 있는 bootstrap으로 접근해야 한다. 

### __NPM 관리의 장점__

NPM으로 bootstrap을 가져오는 방법이 더 복잡하고 귀찮다. 그런데도 우리가 NPM을 통해 관리해야 하는 이유는 무엇일까?

해당 방법을 통해 bootstrap을 가져오면 우리가 필요로 하는 기능만을 가져와서 사용할 수 있다.  
그리고 bootstrap에서 제공하는 기본적인 테마들을 우리가 원하는 대로 커스터마이징을 할 수 있다.