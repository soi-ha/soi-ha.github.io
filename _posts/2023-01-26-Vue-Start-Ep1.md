---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 시작하기 Ep.1"
tags:
  - TIL
  - Vue.js
  - JS
---

## __Vue CLI, Vetur__
---

- Vue CLI 설치
  
  [Vue CLI Link](https://cli.vuejs.org/)
  
  경로 위치 상관없이 해당 명령어를 실행하면 된다. (-g는 전역설치이기 때문에 경로 위치 상관없음)
  
  ```bash
  npm install -g @vue/cli
  # 또는
  yarn global add @vue/cli
  ```
    
- Vue CLI로 폴더 생성
  
  Vue CLI 설치로 아래와 같은 명령어 사용 가능
  
  ```bash
  # 간단하게 새로운 프로젝트 생성하기
  ## vue create 폴더명
  vue create hello-world
  ```
  
  폴더 생성 후, 다음과 같은 옵션 선택이 뜬다. (화살표로 이동이 가능하며 선택이 가능하다.)
  
  현재는 옵션 선택에 vue2,3버전 중 어떤 걸 사용할 것인지 묻는다.  
  (원하는 버전으로 선택하면 된다.) 
  
  ![https://cli.vuejs.org/cli-new-project.png](https://cli.vuejs.org/cli-new-project.png)
    
- package.json
  
  <img width="289" alt="스크린샷 2023-01-26 14 54 51" src="https://user-images.githubusercontent.com/77609591/214785809-6c349beb-e2cd-49d4-95cc-d0454939c478.png">
  
  - serve는 dev와 동일한 것이다.
    
    개발서버를 열고 싶다면 다음과 같은 명령어 사용
    
    ```bash
    npm run serve
    ```
      
  - 제품화 과정에서는 `npm run build`
  - **lint**
    
    vue.js 코드를 어떤 규칙에 맞게 작동이 잘 되었는지 확인하기 위해서 해당 명령어를 사용한다.
    
    lint를 통해서 코드를 작성하는 규칙을 정할 수 있다.  
    하단에서 **eslintConfig** 에서 기본적인 구성옵션을 확인할 수 있으며, **rules** 부분에 원하는 규칙을 추가할 수 있다.
      
  - vue-cli의 경우에는 내부적으로 webpack을 사용하고 있다.
    
    webpack에 대한 기본적인 구조는 프로젝트 내에서 숨겨져 있다.  
    대신, 숨겨진 부분을 vue-cli-service를 통해서 제공한다.
      
  - browserslist
    
    프로젝트가 지원할 브라우저의 기본적인 종류를 명시한다.  
    vue.js 설치시 자동으로 작성되어져 있으며, babel, postCSS등을 기본적으로 사용할 수 있다.
        
- Vue CLI 장점과 단점
  
  __장점__
  
  기본적인 부분부터 디테일한 부분까지 vue-cli에서 흡수를 했기 때문에 입문자 입장에서는 구성옵션을 최대한 신경쓰지 않고 문법에 집중해서 프로젝트를 만들 수 있어서 편리하다. 
  
  __단점__
  
  vue-cli에서 대부분 관리하기 때문에 세부적인 부분들을 정리하면서 나만의 프로젝트를 관리하기 어렵다.
    
- public과 src 파일 **(Vetur)**
  - html 파일
    
    `<div id=”app”></div>` 에 vue.js를 연결해서 사용한다.
    
  - main.js
    
    ```jsx
    import { createApp } from 'vue'
    import App from './App.vue'
    
    createApp(App).mount('#app')
    ```
    
    createApp을 vue에서 가져와서(import) 사용을 하고 있다.  
    4번째 줄 createApp 앞에는 Vue.이 생략되어져 있으며, 이것을 대신해서 vue 패키지에서 해당하는 내용을 가져와서 직접적으로 실행하는 구조로 되어져 있다.
    
    mount 메소드를 통해서 html에서 app 아이디를 가진 요소를 선택해서 vue.js 프로젝트를 바로 연결한다.
    
    App이라는 이름으로 어떤 내용을 가져와서 연결을 하는데, 해당 내용은 주변에 존재하는 App.vue 파일에서 온다.
      
  - App.vue (확장자 **Vetur**)
      
    npm 프로젝트로 시작하게 되면, vue라는 확장자를 가진 파일을 만들어서 vue 프로젝트를 시작할 수 있다.
    
    App.vue 파일을 열어보면 하이라이팅이 되어있지 않아 내용을 확인하기 어려운데, vscode에서 확장자를 설치하여 하이라이팅을 만들어준다.   
    vscode에서 **Vetur**라는 확장자를 설치해준다.
    
    - template
      
      기본적인 html 내용을 작성한다.
        
    - script
      
      자바스크립트 내용을 작성한다.
        
    - style
      
      css 내용을 작성한다.