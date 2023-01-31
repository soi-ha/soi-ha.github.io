---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 문법 Ep.2"
tags:
  - TIL
  - Vue.js
  - JS
---

## __Computed__
---

```jsx
<template>
  <section v-if="hasFruits">
    <h1>Fruits</h1>
    <ul>
      <li
        v-for="fruit in fruits"
        :key="fruit">
        {{ fruit }}
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      fruits: [
        'Apple', 'Lemon', 'Grape'
      ]
    }
  },
  computed: {
    hasFruit() {
      return this.fruits.length > 0
    }
  }
}
</script>
```

computed는 한마디로 말해서 **계산된 데이터**이다.

계산된 데이터(computed)는 데이터 옵션에 정의해둔 특정한 데이터(fruits)를 추가적으로 어떤 연산(this.fruits.length > 0)을 통해서 정의를 한다. 이 정의된 값을 반환해서 사용할 수 있는 새로운 데이터(hasFruit)이다.

hasFruit은 fruits 배열 데이터의 길이가 0보다 커야만 반복문을 사용하여 데이터 값을 출력하도록 한다.  
여기서, fruits 배열 데이터에 아무 값도 존재하지 않다면 h1 태그(제목) 또한 출력되지 않는다.

### __computed 사용 예시__

```jsx
<template>
  <section>
    <h1>Reverse Fruits</h1>
    <ul>
      <li
        v-for="fruit in reverseFruits"
        :key="fruit">
        {{ fruit }}
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    return {
      fruits: [
        'Apple', 'Lemon', 'Grape'
      ]
    }
  },
  computed: {
    reverseFruits() {
      return this.fruits.map(fruit => {
        return fruit.split('').reverse().join('')
      })
    }
  }
}
</script>
```

reverseFruits의 계산은 다음과 같다.  
'Apple' ⇒ ['A','p','p','l','e'] ⇒ ['e','l','p','p','A'] ⇒ 'elppA'  
배열 데이터 fruit를 각 알파벳으로 쪼개고 해당 배열을 뒤짚은 다음에 다시 이어준다.

## __Computed 캐싱__
---

- 캐싱하지 않은 코드
  
  메소드를 불러올 때 마다 계산을 한다.  
  즉, 같은 값을 계속 불러와도 같은 연산을 계속 반복하여 값을 출력하는 것이다.
  
  ```jsx
  <template>
    <h1>{{ reverseMessage() }}</h1>
    <h1>{{ reverseMessage() }}</h1>
    <h1>{{ reverseMessage() }}</h1>
  </template>
  
  <script>
  export default {
    data() {
      return {
        msg: 'Hello Computed!'
      }
    },
    methods: {
      reverseMessage() {
        return this.msg.split('').reverse().join('') 
      }
    }
  }
  </script>
  ```
    
- 계산된 데이터 (캐싱)
  
  마치 데이터처럼 적용을 하면 된다.
  
  computed 옵션 부분에 만들어 둔 계산된 데이터는 캐싱이라는 기능이 있기 때문에 한번 연산을 해 놓은 값이 있으면 반복적으로 화면에 데이터처럼 출력할 때, 다시 한번 연산하지 않는다.
  
  캐싱이 되어져 있기 때문에 저장된(캐싱된) 값으로 해당하는 내용을 출력해준다.  
  첫번째만 연산을 실행하고 그 이후의 두번째, 세번째, … 100번째는 연산을 진행하지 않고 첫번째 연산에서 찾은 값을 저장해 두었다가 출력한다.
  
  ```jsx
  <template>
    <h1>{{ reversedMessage }}</h1>
    <h1>{{ reversedMessage }}</h1>
    <h1>{{ reversedMessage }}</h1>
  </template>
  
  <script>
  export default {
    data() {
      return {
        msg: 'Hello Computed!'
      }
    },
    computed: {
      reversedMessage() {
        return this.msg.split('').reverse().join('') 
      }
    }
  }
  </script>
  ```