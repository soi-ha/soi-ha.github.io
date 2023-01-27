---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 시작하기 Ep.2 (ESLint)"
tags:
  - TIL
  - Vue.js
  - JS
  - ESLint
---

### __ESLint 패키지 설치__
---
  
  ```bash
  npm i -D eslint eslint-plugin-vue @babel/eslint-parser
  ```
  
  - eslint
  - eslint-plugin-vue
  - @babel/eslint-parser

### __.eslintrc.js 파일 생성__
---
  
  ```jsx
  module.exports = {
    env: {
      browser: true,
      node: true
    },
    extends: [
      // vue
      // 'plugin: vue/vue3-essential', // Lv1
      'plugin: vue/vue3-strongly-recommended', //Lv2
      // 'plugin: vue/vue3-recommended', // Lv3
  
      // js
      'eslint: recommended'
    ],
    parserOptions: {
      parser: '@babel/eslint-parser'
    },
    rules: {
      
    }
  }
  ```
  
  - env
    
    브라우저 환경에서 동작하는 전역 개념.  
    node.js 환경에서 동작하는 전역 개념들을 모두 코드 검사를 할 것인지 설정한다.
      
  - extends
    
    **vue.js에 대한 기본적인  코드 규칙**
    
    eslint-plugin-vue 패키지에서 제공하는 기본적인 vue.js의 코드 규칙들이다.  
    이 중에서 가장 엄격하게 vue.js 문법을 권장하는 것이 세번째 (Lv3)이다. 우리는 이 3가지중 두번째 규칙을 사용한다.
    
    **js에 대한 기본적인 코드 규칙**
    
    eslint에서 권장하는 코드 규칙을 통해서 js 문법을 검사한다. 
    
    [Available rules | eslint-plugin-vue Link](https://eslint.vuejs.org/rules/)  
    해당 페이지의 내용을 참고하여 vue.js 규칙들을 원하는 것으로 수정해서 프로젝트를 진행하면 좋다.
    
    [Rules - ESLint - Pluggable JavaScript Linter Link](https://eslint.org/docs/latest/rules/)  
    해당 페이지는 권장되는 규칙들을 명시해 둔 것이다. 만약 해당 규칙이 마음에 들지 않는다면, 위 페이지를 참고해서 원하는 규칙으로 수정해서 프로젝트를 진행하도록 하자.
    
  - parserOptions
    
    기본적인 코드를 분석할 수 있는 분석기를 지정해준다.  
    우리는 parser 속성에 babel-eslint 패키지를 입력해서 해당 패키지를 분석기로 지정해준다.
      
  - rules
    
    두번째 옵션인 extends 옵션에서 기본적으로 제공하는 코드의 규칙들을 사용했을 때는 따로 작성하지 않아도 된다.  
    하지만, 해당 규칙을 다르게 변경해야 할 때 해당 옵션에 규칙을 추가한다. 
      
  - 기본 설정 규칙
    
    ```jsx
    <template>
      <img 
        src="~assets/logo.png" 
        alt="soha"
      >
    </template>
    ```
      
### __규칙 수정하기__
---
  - html-closing-bracket-newline
    
    기본 설정
    
    ```jsx
    {
    "vue/html-closing-bracket-newline": ["error", {
        "singleline": "never",
        "multiline": "always" // never로 변경
      }]
    }
    ```
    
    “singline”: “never” ⇒ 한 줄로 작성하는 것에는 닫히는 괄호를 줄 바꿈 처리 하지 않아도 된다.
    
    “multiline”: “always” ⇒ 여러 줄로 작성할 때는 닫히는 괄호를 꼭 줄바꿈 처리 해야 한다.  
    만약, 여러 줄일때 닫는 괄호를 줄바꿈 처리하고 싶지 않다면 never로 변경하면 된다.
    
    ```jsx
    <template>
      <img 
          src="~assets/logo.png" 
          alt="soha">
    </template>
    ```
      
  - html-self-closing
    
    __기본설정__
    
    ```jsx
    {
    "vue/html-self-closing": ["error", {
        "html": {
          "void": "never", // always로 변경
          "normal": "always", // never로 변경
          "component": "always"
        },
        "svg": "always",
        "math": "always"
      }]
    }
    ```
    
    void: `<img>` 같이 빈 태그를 지칭한다. 빈 태그 닫는 괄호 앞에 슬래쉬(/)를 넣고 싶다면 never에서 always로 변경하면 된다.
    
    normal: `<div></div>` 와 같이 일반적으로 열리고 닫히는 태그를 말한다. 해당 태그 사이에 어떤 content가 존재하지 않다면 해당 태그로 슬래쉬 기호로 종료 `<div />` 를 하면 된다고 설정되어 있다.   
    해당 설정을 원하지 않다면 never로 변경하면 된다. 그러면 `<div />` 로 작성하게 되면 규칙을 벗어났다고 알려준다.
    
    component: 이전에 `<HelloWorld />` 라고 컴포넌트를 작성했던 것처럼(빈 태그처럼) 작성하라고 규칙이 설정되어 있다. 
    
    svg, math: svg와 math 태그도 내용이 없으면 빈 태그로 작성하도록 규칙이 설정되어져 있다.
    
    ```jsx
    <template>
      <img 
        src="~assets/logo.png" 
        alt="soha" />
      <div></div>
    </template>
    ```
      
  - 에러 문법 규칙 자동 수정
    
    vscode에서 settings 검색 한후, settings.json 파일을 열어 해당 내용을 추가한다.
    
    ```json
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
      }
    ```
    
    vscode의 editor에서 파일을 저장하게 되면, 저장된 파일의 소스코드를 eslint 규칙에 맞게 수정한다는 의미이다.
    
    - `<div />`가 규칙에 맞지 않음
      
      ```jsx
      <template>
        <img 
          src="~assets/logo.png" 
          alt="soha" />
        <div />
      </template>
      ```
        
    - 저장 후, 자동으로 올바른 규칙으로 수정됨
        
      ```jsx
      <template>
        <img 
          src="~assets/logo.png" 
          alt="soha" />
        <div></div> 
      </template>
      ```