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