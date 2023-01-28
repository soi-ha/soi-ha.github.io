---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 문법 Ep.1"
tags:
  - TIL
  - Vue.js
  - JS
---
## __라이프사이클__
---

>[Vue.js 문법 참고 링크](https://v3-docs.vuejs-korea.org/guide/essentials/application.html)
>
>해당 페이지의 내용을 꼭 참고하여 공부할 것
>

```jsx
<template>
  <h1> {{ count }} </h1>
</template>

<script>
export default {
  data() {
    return {
      count: 2
    }
  },
  beforeCreate() {
    console.log("Before Create!!", this.count)
  },
  created() {
    console.log("Created!!", this.count)
    console.log(document.querySelector('h1'))
  },
  beforeMount() {
    console.log("Before Mount!!")
    console.log(document.querySelector('h1'))
  },
  mounted() {
    console.log("Mounted!!")
    console.log(document.querySelector('h1'))
  }
}
</script>
```

- beforeCreate
  
  컴포넌트가 생성되기 직전에 실행 된다. (데이터 정의 전)
  그닥 활용도가 활용도가 높지 않다.
    
- created
    
  컴포넌트가 생성된 직후에 실행 된다.
  가장 많이 사용되는 라이프사이클 훅(라이프사이클)이다.
  컴포넌트가 생성된 직후기 때문에 html 연결되어 있지 않아서 조회 불가능하다.
    
- beforeMount
  
  html과 연결되기 전이기 때문에 html 구조 조회 불가능하다.
  실제 사용하는 활용도 떨어진다.
    
- mounted
  
  실제 컴포넌트가 html 구조에 연결되어져 있다.
  따라서 html 구조를 조회해서 출력 가능하다.
  가장 많이 사용되는 라이프사이클 훅(라이프사이클)이다.

## __템플릿 문법__
---

>[Vue.js 템플릿 문법](https://v3-docs.vuejs-korea.org/guide/essentials/template-syntax.html#text-interpolation)
>
> 해당 페이지 내용 참고하기
>

- __텍스트 보간법__
  
  텍스트 바인딩의 가장 기본적인 형태, 이중 괄호 문법을 사용한 텍스트 보간법이다.
  
  - **v-once**
    
    html의 속성처럼 사용한다.  
    데이터를 화면에 단 한번만 랜더링한다. 데이터가 변경되더라도 갱신하지 않는다.  
    즉, 반응성을 가지지 않는다.
    
    ```jsx
    <template>
      <h1 
        v-once
        @click="add">
        {{ msg }}
      </h1>
    </template>
    ```
        
- __HTML 출력__
  
  이중 중괄호는 데이터를 HTML이 아닌 일반 텍스트로 해석한다. 실제 HTML을 출력하려면 v-html 디렉티브를 사용해야 한다.
  
  이중 중괄호를 사용해서 데이터를 출력하는 것이 아닌, v-html 디렉티브의 속성이 값으로 데이터를 넣으면 이 부분이 실제 html 문법으로 출력이 된다.
  
  ```jsx
  <template>
    <h1 v-html="msg"></h1>
  </template>
  ```
    
- __속성 바인딩__
  
  이중 중괄호 구문을 html 속성에 사용할 수 없다.   
  대신 우리는 v-bind 디렉티브를 사용하여 html 속성 부분에 데이터를 넣는다.
  
  ```jsx
  <template>
    <h1 v-bind:class="msg">
      {{ msg }}
    </h1>
  </template>
  
  <script>
  ...
  </script>
  
  <style scoped>
    .active {
      color: royalblue;
      font-size: 80px;
    }
  </style>
  ```
  
  - v-bind 단축
    
    ```jsx
    <template>
      <h1 v-bind:class="msg">
        {{ msg }}
      </h1>
    </template>
    ```
    
    위 코드처럼 작성하고 저장을 누르면 다음과 같이 v-bind가 사라지고 콜론만 남으며 코드가 단축된다. 
    
    ```jsx
    <template>
      <h1 :class="msg">
        {{ msg }}
      </h1>
    </template>
    ```
      
  - v-on 단축
      
    v-on 디렉티브의 약어는 @로, v-on이 사라지고 @가 생긴다.
    
    ```jsx
    <template>
      <h1 v-on:click="msg">
        {{ msg }}
      </h1>
    </template>
    ```
    
    ```jsx
    <template>
      <h1 @click="msg">
        {{ msg }}
      </h1>
    </template>
    ```
      
  
- __동적 전달 인자__
  
  ```jsx
  <template>
    <h1
      :[attr]="active"
      @[event]="add">
      {{ msg }}
    </h1>
  </template>
  
  <script>
  export default {
    data() {
      return {
        msg: 'active',
        attr: 'class',
        event: 'click'
      }
    }
  }
  ```
  
  - attr에 class
    
    data에 attr을 만들어 값은 ‘class’를 넣어준다.  
    template 부분에서 h1 태그 속성의 class 부분에 attr을 넣어준다.  
    해당 태그에 클래스로 msg가 잘 들어가게 되는 것을 볼 수 있다.
    
    `:class=”active”` ⇒ `:[attr]=”active”` 
    
  - event에 click
    
    클릭을 했을 때 느낌표가 추가되는 동작에서 template 부분의 click에 event를 넣어준다.  
    해당 부분에 click 문자가 잘 들어가게 되면서 클릭할 때 느낌표가 추가되는 것을 볼 수 있다.
    
    `@click=”add”` ⇒ `@[event]=”add”`
      
  - **브라우저, active 클릭시 style 적용 취소**
      
    파란색 active 글자를 눌러서 느낌표를 추가하려 할 때, 해당 스타일 적용이 취소되면서 검정 글자에 느낌표가 추가되는 것을 볼 수 있다.
    
    스타일이 취소되는 이유는 class 이름이 active인데, 이 active가 순수 문자 데이터로 읽혀지지 않게 되기 때문이다. active 글자를 클릭시 class 이름 active에도 느낌표가 추가된다.   
    active!!!… 의 클래스의 스타일은 존재하지 않기 때문에 스타일 적용이 취소되는 것이다.
    
    해당 문제점의 해결 방법은 간단하다. class 이름의 active를 문자열로 인식하게 하는 것이다.   
    그렇게 되면, 글자를 클릭해도 active에 느낌표가 추가되지 않고 스타일이 잘 적용되는 것을 볼 수 있다.  
    문자열로 바꿔주는 것은 class의 active 부분에 작은 따옴표만 추가해주면 된다. 
    
    `:[attr]="active"` ⇒ `:[attr]="’active’"`