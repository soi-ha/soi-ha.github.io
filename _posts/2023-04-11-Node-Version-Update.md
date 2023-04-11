---
layout: post
categories:
  - TIL
title: 'Node.js 버전 업데이트 및 업데이트 적용 불가 에러'
tags:
  - TIL
  - Node.js
---

## Node.js 버전 업데이트 하기

---

- 현재 버전 확인
  ```bash
  node -v # 12.14.1
  ```
- npm 캐시 제거

  ```bash
  npm cache clean -f
  ```

- node.js 버전 관리 모듈(n) 설치

  ```bash
  npm install -g n
  ```

- 원하는 버전으로 설치

  lts 혹은 stable로 설치했다.

  ```bash
  n stable
  ```

  해당 명령어를 치니 sudo 작성해달라고 뜨길래 다음과 같이 입력했다.

  ```bash
  sudo n stable
  ```

## 업데이트 적용 불가

---

위 방법을 통해 업데이트를 했으나, 해당 버전이 적용되지 않았다.

<img width="655" alt="스크린샷 2023-04-11 13 22 19" src="https://user-images.githubusercontent.com/77609591/231227709-753d3035-06bc-4325-9144-c88608ef4556.png">

다음과 같이 installed와 active 두개의 경로가 존재했다.  
새로 업데이트 한 것은 installed 경로이고 기존은 active 경로이다.  
두 경로를 일치시켜야 적용이 된다.

- 경로 일치시키기

  ```bash
  # 괄호는 생략하고 경로 입력
  ln -sf <installed 경로> <active경로>
  ```

  해당 명령어를 통해서 경로를 일치시켜주고 버전을 다시 확인하면 업데이트가 정상적으로 된 것을 확인할 수 있다.

  <img width="655" alt="스크린샷 2023-04-11 13 22 23" src="https://user-images.githubusercontent.com/77609591/231227719-be45b738-02ee-4108-a370-54bdd203bb0f.png">
