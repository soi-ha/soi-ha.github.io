---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 문법 Ep.4 조건부 렌더링 "
tags:
  - TIL
  - Vue.js
  - JS
---
## __조건부 렌더링__
---

>[Vue.js 조건부 렌더링](https://v3-docs.vuejs-korea.org/guide/essentials/conditional.html)

- v-if, v-else
  
  조건에 따라 블록을 렌더링할 때 사용한다. 블록은 디렉티브의 표현식이 true 값을 반환할 때만 렌더링된다.
  
  v-else와 함께 else 블록을 추가할 수 있다.
  
  ```jsx
  <h1 v-if="awesome">Vue is awesome</h1>
  <h1 v-else>Oh no</h1>
  ```
  
  awesome 데이터가 참으로 해석될 수 있는 값(Truthy)이면, 해당 디렉티브가 있는 h1 요소가 화면에 렌더링된다.  
  awesome 데이터가 true가 아닌 false로 해석된다면 첫번째 h1 태그는 화면에 렌더링되지 않고, v-else가 있는 두번째 h1 태그가 렌더링된다.
  
  - 예시
    
    ```jsx
    <template>
      <button @click="handler">
        Click!
      </button>
      <h1 v-if="isShow">
        Hello!
      </h1>
      <h1 v-else>
        Good~
      </h1>
    </template>
    
    <script>
    export default {
      data() {
        return {
          isShow: true
        }
      },
      methods: {
        handler() {
          this.isShow = !this.isShow
        }
      }
    }
    </script>
    ```
    
    handler는 isShow가 true인 경우에는 false가 할당, false인 경우에는 true가 할당되는 구조이다.
    
    결과적으로는 버튼을 누르면 Hello 문구가 사라지고 Good 문구가 나타나며 다시 버튼을 누르게 되면 Hello 문구가 나타난다. (즉 토글 만들기!)  
    
- v-else-if
  
  ```jsx
  <template>
    <h1 v-if="isShow">
      Hello!
    </h1>
    <h1 v-else-if="count > 3">
      Count > 3
    </h1>
    <h1 v-else>
      Good~
    </h1>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isShow: true,
        count: 0
      }
    },
    methods: {
      handler() {
        this.isShow = !this.isShow
        this.count += 1
      }
    }
  }
  </script>
  ```
  
  v-if가 false이고 v-else-if에 작성해둔 조건을 만족하게 되면 v-else-if가 화면에 출력된다.  
  조건 count 값이 3 이상을 만족하게 되면 true일 때는 Hello가 출력, false일 때는 Count > 3이 출력된다.  
  Good은 count 값이 3 이하일 때, isShow 값이 false일 시에만 출력된다.
  
- template에서 v-if
  
  v-if 조건을 통해서 특정한 요소들을 그룹으로 보이거나 보이지 않게 할 때, 일반 요소 대신에 template 요소를 사용한다.  
  단, v-if 디렉티브를 해당 파일의 최상위 template에는 사용하면 안된다.
  최상위 template에 사용하면 따로 적용되지 않는다.
  
  template의 내용 전체를 없애고 싶다면 최상위 template 안에 새로운 template 태그를 생성하여 해당 내용을 전체 감싸주면 된다.
  
  - div를 사용하여 감싸기
    
    개발자도구에서 보면 div 태그에 h1, p태그들이 감싸져있는 것을 볼 수 있다.
    
    ```jsx
    <template>
      <button @click="handler">
        Click!
      </button>
      <div v-if="isShow">
        <h1>Title</h1>
        <p>Paragraph 1</p>
        <p>Paragraph 2</p>
      </div>
    </template>
    ```
      
  - template로 감싸기
    
    개발자도구에서 확인해보면 template 태그는 보이지 않고 h1, p태그들만 보이게 된다. 즉, 감싼 template 태그는 보이지 않는다!  
    template는 렌더링이 되지 않는 것!
    
    ```jsx
    <template>
      <button @click="handler">
        Click!
      </button>
      <template v-if="isShow">
        <h1>Title</h1>
        <p>Paragraph 1</p>
        <p>Paragraph 2</p>
      </template>
    </template>
    ```
    
  
- v-show
  
  연결되어있는 데이터의 값의 상관없이 렌더링한다.   
  v-if의 경우 데이터 값에 따라 렌더링을 진행한다.
  
  ```jsx
  <template>
    <button @click="handler">
      Click!
    </button>
    <h1 v-show="isShow">
      Hello?
    </h1>
  </template>
  ```
  
  h1 태그가 v-if일 때에는 isShow가 false이기 때문에 화면에 Hello가 출력되지 않고 개발자도구를 열어도 해당 h1태그가 보이지 않는다.  
  하지만 v-show로 하면 화면상에는 출력되지 않지만 개발자도구에는 해당 태그가 보이게 된다. 또한 h1 태그의 style 속성이 display이 none으로 되어있다.
  
  v-show는 template 요소를 지원하지 않으며, v-else와 함께 사용할 수 없다.
    
- v-if 와 v-show 비교
  - v-if
    
    “실제”조건부 렌더링이다. 초기 렌더링 시, 조건이 false이면 아무 작업도 하지 않는다. 조건부 블록또한 조건이 처음으로 true가 될 때까지 렌더링되지 않는다.
    
    즉, 개발자도구에서 해당 내용이 출력되지 않는다.
      
  - v-show
      
    CSS 기반 전환으로 초기 조건과 관계없이 항상 렌더링된다.
    
    조건과 상관없이 개발자도구를 열어서 확인하면 내용은 항상 렌더링되어있다. 다만, false일 경우 display가 none이 된다.
    
  - 비교
    
    v-if는 전환 비용이 높은 반면에 v-show는 초기 렌더링 비용이 높다.
    
    따라서 자주 전환이 일어나야 한다면 v-show를 사용하는 것이 좋으며, 화면이 동작하고 있을 때 적용해 놓은 데이터의 조건이 자주 변경되지 않는다면 v-if를 사용하는 것이 더 좋다.