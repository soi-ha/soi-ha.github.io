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

computed는 한마디로 말해서 계산된 데이터이다.

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