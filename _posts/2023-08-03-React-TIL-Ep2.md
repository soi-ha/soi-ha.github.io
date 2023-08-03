---
layout: post
categories:
  - TIL
title: 'React 기본: Webpack&Babel , CRA'
tags:
  - TIL
  - React
---

## Webpack & Babel

### Webpack?

---

오픈 소스 자바스크립트 모듈 번들러(여러개를 합쳐주는 것)로, 여러개로 나눠져 있는 파일들을 하나의 자바스크립트 코드로 압축하고 최적화하는 라이브러리이다.

<img alt="Webpack 설명 이미지" src="https://images.velog.io/images/bochodev/post/042cfbd4-8989-4a28-9ba0-849dc024583d/2AC99748-37B7-4931-B7AE-1BE6C28C5768.png">

**Webpack의** **장점**

- 여러 파일의 자바스크립트 코드를 압축하여 최적화 할 수 있어 로딩에 대한 네트워크 비용을 줄일 수 있다.

- 모듈 단위로 개발이 가능하여, 가독성과 유지보수가 쉽다.  
  모듈 단위로 개발의 말은, 한가지 파일에 모든 것들을 넣어두는 것이 아닌 파일을 여러개로 나눠서 Webpack으로 뭉쳐(번들)주는 것을 말한다. 파일을 나눴기 때문에 가독성이 올라가고 유지보수가 쉬워지는 것이다.

### Babel?

---

최신 자바스크립트 문법을 지원하지 않는 브라우저를 위해서 최신 자바스크립트 문법을 구형 브라우저에서도 사용할 수 있게 변환시켜주는 라이브러리이다.

### Creat-React-App

---

CRA는 Webpack이나 Babel을 따로 설치하지 않아도 알아서 설치하고 설정해주는 것이다.

```bash
npx creat-react-app <폴더명>

# 혹은 cra를 설치하고 싶은 폴더로 이동
npm creat-react-app ./
```

### npx??

---

노드 패키지 실행을 도와주는 도구이다. CRA는 npm 레지스트리(registry)에 있는 패키지를 우리의 폴더에서 실행해서 리액트를 설치해준다.

npm 레지스트리(라이브러리가 저장된 곳) ⊃ CRA 패키지 **→** npx를 통해 CRA 설치 `npx creat-react-app ./` **→** 우리가 사용할 폴더
