---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 문법 Ep.9 컴포넌트2 "
tags:
  - TIL
  - Vue.js
  - JS
---
## __컴포넌트: Slot__
---
> [Vue.js: 슬롯](https://v3-docs.vuejs-korea.org/guide/components/slots.html)
- Fallback contents
  
  App.vue(부모 컴포넌트) 파일에서 MyBtn 컴포넌트 태그 사이에 컨텐츠가 없을 때, MyBtn.vue(자식 컴포넌트) 파일의 slot 태그 사이에 있는 컨텐츠가 출력되는 것을 말한다.
  
- 이름을 갖는 슬롯 (Named Slots)
  
  slot에 name 속성을 부여해서 순서가 어떻게 되었던 간에 해당 slot의 이름 순서에 맞게 화면에 출력하도록 만들어 준다.
  
  ```jsx
  <template>
    <MyBtn>
      <template
        v-slot:
        icon>
        <span>(B)</span>
      </template>
      <template #text>
        <span>Banana</span>
      </template>
    </MyBtn>
  </template>
  ```
  
  v-slot을 통해서 원하는 컨텐츠에 이름을 지정해준다.   
  이때, v-slot을 축약하여 ‘#’ 으로 작성할 수 있다.
  
  ```jsx
  <template>
    <div class="btn">
      <slot name="icon"></slot>
      <slot name="text"></slot>
    </div>
  </template>
  ```
  
  App.vue 파일에서 icon과 text의 순서가 뒤바뀌어도 MyBtn에서는 slot의 순서가 icon이름을 갖는 것이 가장 먼저이기 때문에 icon 이름을 갖는 컨텐츠가 먼저 출력된다.

## __컴포넌트: Provide, Inject__
---
> [Vue.js: Provide(제공),Inject(주입)](https://v3-docs.vuejs-korea.org/guide/components/provide-inject.html)

- Prop 드릴링
  
  일반적으로 부모 컴포넌트에서 자식 컴포넌트로 데이터를 전달해야 할 때 props를 사용한다.   
  그러나 큰 컴포넌트 트리가 존재하고 저 아래에 있는 컴포넌트에서 상위 컴포넌트의 무언가가 필요할 때, 아래 컴포넌트에서 위로 올라가기 위해 다른 부모 컴포넌트에도 동일한 props를 전달해야 한다.
  
  ![Prop 드릴링](https://v3-docs.vuejs-korea.org/assets/prop-drilling.5af51664.png)
  
  예를 들어, Footer 컴포넌트는 이 prop가 전혀 필요하지 않더라도 DeepChild가 접근할 수 있도록 선언하고 전달해야 한다. 더 긴 상위 체인이 있다면 해당 과정을 통해 더 많은 컴포넌트가 영향을 받게 된다. 
  이것을 prop 드릴링이라고 한다.
    
- Prop 드릴링 해결방법
  
  우리는 provide와 inject로 props 드릴링을 해결할 수 있다. 부모 컴포넌트는 모든 자식 컴포넌트에 대한 **의존성 제공자** 역할을 할 수 있다. 하위 트리의 모든 컴포넌트는 깊이에 관계없이 상위 체인의 컴포넌트에서 제공(provide)하는 의존성을 **주입**(inject)할 수 있다.
  
  ![Provide와Inject](https://v3-docs.vuejs-korea.org/assets/provide-inject.2d638840.png)
    
- Provide와 Inject
  - Provide
    
    provide 옵션을 추가하여 컴포넌트 하위 항목에 데이터를 제공한다.
    
    ```jsx
    export default {  
      provide: {    
        message: '안녕!'  
      }
    }
    ```
    
    provide 객체의 각 속성에 대해 키는 주입한 값을 올바르게 찾기 위해 사용되고, 값은 주입된다.
    
  - Inject
    
    inject 옵션을 통해 부모 컴포넌트에서 제공하는 데이터를 주입한다.
    
    ```jsx
    export default {
      inject: ['message'],
      created() {
        console.log(this.message) // 주입된 값
      }
    }
    ```
    
    inject는 컴포넌트 자체 상태보다 먼저 구성되므로, data()에서 주입된 속성에 접근할 수 있다.
      
  - 반응형으로 만들기
    
    provide를 사용할 때는, 일반적으로 반응성을 제공할 수 없다.  
    따라서, 데이터를 전달해서 한번 출력하는 용도로 사용하거나 반응성을 유지하게 하려면 추가 작업을 해야 한다.
    
    제공자로부터 반응형으로 연결된 주입을 만들기 위해, computed() 함수를 사용하여 계산된 속성을 제공한다.
    
    ```jsx
    import { computed } from 'vue'
    
    export default {
      data() {
        return {
          message: '안녕!'
        }
      },
      provide() {
        return {
          // 계산된 속성을 명시적으로 제공
          message: computed(() => this.message)
        }
      }
    }
    ```
    
    기존에 있었던 this.message 부분을 지우고 computed 함수를 실행한다. 함수의 콜백으로 화살표 함수를 작성하고 반응성을 가지고 싶은 데이터를 반환하면 된다.

## __컴포넌트: Refs__
---

### ref
해당하는 특정한 요소를 지정한 이름으로 참조하겠다는 의미이다.  
참조된 내용은 $refs 객체에 지정한 이름으로 들어가서 저장이 된다.

```jsx
<template>
  <h1 ref="hello">
    Hello World!
  </h1>
</template>

<script>

export default {
  mounted() {
    console.log(this.$refs.hello)
  }
}
</script>
```

- 주의할 점
  
  해당하는 요소들은 해당 컴포넌트가 html 구조와 연결이 된 직후에만 사용이 가능하다. 따라서 created()라는 라이프사이클에서는 해당 내용을 제대로 사용할 수 없다.