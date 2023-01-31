---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 문법 Ep.3 클래스와 스타일 바인딩"
tags:
  - TIL
  - Vue.js
  - JS
---

## __클래스와 스타일 바인딩__
---

>[Vue.js: 클래스와 스타일 바인딩](https://v3-docs.vuejs-korea.org/guide/essentials/class-and-style.html)
>

### __HTML 클래스 바인딩__

```jsx
<template>
  <h1 
    :class="{ active: isActive }"
    @click="activate">
    Hello?!({{ isActive }})
  </h1>
</template>

<script>
export default {
  data() {
    return {
      isActive: false
    }
  },
  methods: {
    activate() {
      this.isActive = true
    }
  }
}
</script>

<style scoped>
  .active {
    color: red;
    font-weight: bold;
  }
</style>
```

class로 active를 추가하고 싶은데 class의 이름은 isActive의 영향을 받는다. boolean 데이터 true면 class에 active가 추가되고, false이면 class에 active가 추가될 수 없는 구조이다.

현재 isActive default 값이 false로 style이 적용되지 않는다.   
h1태그를 클릭하게 되면 메소드 activate로 인해 isActive의 값이 true로 변경된다. isActive 값이 true가 되면서 class 값 active가 활성화되어 style이 적용된다.

- 바인딩 객체 인라인
  
  바인딩 객체는 인라인`{{ }}`일 필요가 없다!
  
  ```jsx
  <div :class="classObject"></div>
  ```
  
  ```jsx
  data() {
    return {
      classObject: {
        active: true,
        'text-danger': false
      }
    }
  }
  ```
  
  데이터 옵션에 객체 데이터로 데이터를 만들어서 데이터를 연결해서 사용할 수 있다. 연결해야 하는 클래스의 내용이 많은 경우에는 위와 같은 방식이 조금 더 효율적이다.
    
- 계산된 데이터도 바인딩할 수 있다.
- 배열 구문
  
  배열을 `:class`에 전달하여 클래스 목록을 적용할 수 있다.   
  배열로 특정한 클래스들을 추가할 수 있다.
  
  ```jsx
  <div :class="[activeClass, errorClass]"></div>
  ```
  
  ```jsx
  data() {
    retrun {
      activeClass: 'active',
      errorClass: 'text-danger'
    }
  }
  ```
  
  **결과**
  
  ```jsx
  <div class="active text-danger"></div>
  ```
  
  일반적인 경우에 유용하다고 할 수는 없지만 배열구문을 통해서도 클래스 부분의 데이터를 연결해서 사용할 수도 있다.
  

### __인라인 스타일 바인딩__

인라인 방식을 통해서 스타일을 작성할 때, 데이터를 바인딩해서 작성할 수 있는 방식이다.

스타일 속성앞에 콜론기호를 사용해서 데이터를 연결해주고 하나의 객체데이터를 연결한다.

```jsx
<div :style="{ color: activeColor, fontSize: fontSize + 'px'}"></div>
```

```jsx
data() {
	return {
		activeColor: 'red'
		fontSize: 30
	}
}
```

해당 코드의 결과로는 color에는 red가 들어가고 fontSize에는 30px이 들어가게 된다.

CSS 속성 이름(fontSize 등)에는 카멜 케이스 혹은 케밥 케이스(대쉬, 케밥 케이스는 따옴표를 함께 사용)를 사용할 수 있다. 

- 객체에 직접 바인딩
  
  템플릿을 더 깔끔하게 만들귀 위해서는 스타일 객체에 직접 바인딩 하는 것이 좋다.
  
  ```jsx
  <div :style="styleObject"></div>
  ```
  
  ```jsx
  data() {
    return {
      styleObejct: {
        color: 'red',
        fontSize: '13px'
      }
    }
  }
  ```