---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 문법 Ep.5 리스트 렌더링 "
tags:
  - TIL
  - Vue.js
  - JS
---
## __리스트 렌더링__
---

>[Vue.js 리스트 렌더링](https://v3-docs.vuejs-korea.org/guide/essentials/list.html)

- v-for
  
  v-for을 사용하여 배열을 리스트로 렌더링할 수 있다.  
  item in items 형식에서 (해당 코드에서는 fruit in fruits) fruit 부분을 괄호로 묶고 뒤에 index 변수를 생성한다.  
  index 변수를 내부에서 이중 중괄호 구문을 추가해서 (앞에 하이픈기호 추가) 해당 변수를 넣어준다.
  
  ```jsx
  <template>
    <ul>
      <li
        v-for="(fruit, index) in fruits"
        :key="fruit">
        {{ fruit }}-{{ index + 1 }}
      </li>
    </ul>
  </template>
  ```
  
  해당 코드의 결과로는 fruit에 들어온 fruits 배열의 데이터들, fruits 내부에서 반복될때마다 index의 값이 1씩 증가되어 출력된다.
    
- 객체에 v-for
  
  원본 데이터(fruits 배열데이터)를 가지고 계산된 데이터 (newFruits 배열 데이터)를 만들었다.
  
  ```jsx
  <script>
  export default {
    data() {
      return {
        fruits: ['Apple', 'Banana', 'Cherry'],
        // newFruits: [
        //   { id: 0, name: 'Apple' },
        //   { id: 1, name: 'Banana' },
        //   { id: 2, name: 'Cherry' }
        // ]
      }
    },
    computed: {
      newFruits() {
        return this.fruits.map((fruit, index) => {
          return {
            id: index,
            name: fruit
          }
        })
      }
    }
  }
  </script>
  ```
  
  v-for 디렉티브로 배열데이터를 출력할 때, 각각의 배열의 아이템들을 고유하게 구별해 줄 수 있는 (여기서는 id) 특정한 속성이 존재해야 한다.   
  해당 속성을 통해서 각각의 배열 아이템을 고유하게 구분해줄 수 있어야만 화면에 출력되는 내용을 vue.js를 통해서 최적화를 시켜줄 수 있다.
  
- id를 고유하게 만들어주는 패키지
  
  ```bash
  npm i -D shortid
  ```
  
  ```jsx
  <template>
    <ul>
      <li
        v-for="fruit in newFruits"
        :key="fruit.id">
        {{ fruit.name }}-{{ fruit.id }}
      </li>
    </ul>
  </template>
  
  <script>
  import shortid from 'shortid'
  
  export default {
    data() {
      return {
        fruits: ['Apple', 'Banana', 'Cherry'],
      }
    },
    computed: {
      newFruits() {
        return this.fruits.map(fruit => ({
          id: shortid.generate(),
          name: fruit
        }))
      }
    }
  }
  </script>
  ```
  
  id 값에 shortid를 넣고 generate 메소드를 실행한다. 간단한 id(shortid)를 생성(generate)해주는 것이다.  
  그러면 비교적 짧은 고유한 값의 문자 데이터가 반환된다.
  
  id 값을 확인하기 위해서 fruit.id를 출력해보면 문자 데이터로 된 id값을 확인할 수 있다.
  
  - 추가 간소화
    
    객체 구조 분해를 통해서 fruit 대신에 id와 name을 넣어 바로 작성할 수 있도록 할 수 있다.
    
    ```jsx
    <template>
      <ul>
        <li
          v-for="{ id, name } in newFruits"
          :key="id">
          {{ name }}-{{ id }}
        </li>
      </ul>
    </template>
    ```
        
- 배열 변경 감지
  - 수정 메소드
    
    감시중인 배열의 변이 메소드를 래핑하여 뷰 갱신을 트리거한다.  
    즉, 배열의 데이터를 바꿔두면 실제 화면에 갱신되어 출력이 되는 반응성을 가진다는 의미이다. 
    
    데이터를 수정했을 때, 반응성을 통해서 화면에 갱신되어 출력된다.
    
    - push()
      
      배열의 가장 마지막에 데이터를 밀어 넣는다.
      
    - pop()
      
      배열의 가장 뒤쪽의 데이터를 꺼내서 반환해준다.
      
    - shift()
      
      배열의 가장 앞쪽의 데이터를 꺼내서 반환한다.
        
    - unshift()
      
      배열의 가장 앞쪽에 데이터를 밀어 넣는다.
        
    - splice()
      
      인덱스를 통해서 데이터를 넣거나 빼고 삭제할 때 사용한다.
      
    - sort()
      
      배열을 정렬시켜준다.
      
    - reverse()
      
      배열을 뒤짚어준다.
          
  - 배열 교체
    
    변이 메소드는 실행이 되면 원본의 배열 자체를 변경한다.
    
    이에 비해 filter(), concat(), slice() 같은 메소드는 원본의 데이터는 변하지 않고 해당 메소드가 동작한 이후에 새로운 배열 데이터를 반환한다. 이것을 비-변이 메소드라고 말한다.
    
    비-변이 메소드는 이전 배열을 새로운 배열로 바꿔서 반응성을 가지고 화면을 갱신할 수 있다.
    
    ```jsx
    this.items = this.items.filter((item) => item.message.match(/Foo/))
    ```
    
    새로운 배열 아이템으로 기존의 배열 아이템을 대체하면 이로 인해 Vue가 기존의 DOM(html 구조)을 버리고 전체목록을 다시 렌더링할 것이라고 생각 할 수도 있지만 그렇지 않다.
    
    Vue는 DOM 요소 재사용을 최대한 효율적으로 관리하기 위해서 스마트 휴리스틱(smart heuristics)을 사용하고 있다.  
    그래서 기존의 배열 데이터에서 약간 수정된 데이터를 다시 할당하게 되면 고유한 값(대표적으로 key 값)을 기준으로 해서 필요한 내용들만 다시 그린다. 필요하지 않는 내용들은 화면에 새롭게 그리지 않는다.   
    즉, 성능적인 자원을 최소화하여 화면에 내용을 다시 그려낼 수 있다.