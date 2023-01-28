---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 시작하기 Ep.3"
tags:
  - TIL
  - Vue.js
  - JS
---
## __선언적 렌더링과 입력 핸들링__
---

- SFC (Single File Component)
    
  단일 파일 컴포넌트이다.   
  Vue 확장자로 만들어져 있는 하나의 파일을 말한다.
  
  SFC는 template, script, style 3가지 태그로 구성할 수 있다.  
  template에는 html(Vue 문법), script는 JS(Vue 문법), style에는 CSS(SCSS) 내용을 작성한다.
  
  해당 구조에서 style태그가 필요하지 않으면 삭제해도 된다. 다만, template과 script는 대부분 필요하기 때문에 기본적으로 명시하고 시작하는 것이 좋다.
    
- Vue 문법 적용해보기
  
  ```vue
  <template>
    <h1> {{ count }} </h1>
  </template>
  
  <script>
  export default {
    data() {
      return {
        count: 0
      }
    }
  }
  </script>
  
  <style>
    h1 {
      font-size: 50px;
      color: royalblue;
    }
  </style>
  ```
  
  - {{ }}: 자바스크립트에서 오는 내용이다.
  - `data: function()`을 `data()`로 생략하여 사용할 수 있다.
  - 서버를 열어 해당 내용을 확인할 때 **styel 적용이 안된다면?**
    
    style을 적용하지 않으면 내용이 출력되는데 style을 적용했더니 내용이 출력되지 않는 현상이 발생했다. 
    
    이럴때 vue-style-loader 패키지의 버전을 다운그레이드 해준다.  
    나 같은 경우는 버전이 4.1.3이었는데 해당 버전으로는 style 적용이 되지 않았다. 4.1.2로 다운그레이드를 해주니 출력이 완료되었다.
    
    **패키지 버전 다운그레이드 하는 방법**
    
    ```bash
    # 패키지 삭제
    npm uninstall 다운그레이드_하려는_패키지명
    # 원하는 버전으로 재설치
    npm install 패키지명@원하는_버전
    
    # 예시
    npm uninstall vue-style-loader
    npm install vue-style-loader@4.1.2
    ```
    
    패키지를 다운그레이드를 하려면 현재 가지고 있는 패키지를 삭제한 후, 원하는 버전으로 재설치를 해줘야 한다.
      
  - 클릭하여 숫자 증가시키기
    
    ```jsx
    <template>
      <h1 @click="increase">
        {{ count }}
      </h1>
    </template>
    
    <script>
    export default {
      data() {
        return {
          count: 0
        }
      },
      methods: {
        increase() {
          this.count += 1
        }
      }
    }
    </script>
    ```
    
    - increase는 숫자를 1씩 증가시켜 준다.  
      h1 태그를 클릭하면 increase 함수를 통해 숫자가 1씩 늘어나도록 설정을 해준다.  
      h1태그에 click속성을 넣어 값으로 increase 함수를 넣어준다.
    - 여기서 말하는 this는 App.vue 파일의 script 부분을 지칭한다.  
      이 this로 data 혹은 methods에 접근할 수 있다.
    - data() ⇒ 데이터를 정의한다.  
      methods() ⇒ 함수를 정의한다.
    - **반응성 (Reactivity)**
      
      데이터를 정의( data() )하고 이것을 갱신할 수 있는 핸들러( methods )를 작성했다. 여기서 가장 중요한 것은 count라는 데이터를 갱신하면, 연결되어져 있는 브라우저의 화면도 같이 갱신된다는 것이다. 이것을 반응성이라고 부른다.  
      요약하면, 데이터를 갱신하면 화면도 바뀐다! 이다.

## __조건문과 반복문__
---

- **조건문 (v-if)**
  
  ```jsx
  <template>
    <h1 @click="increase">
      {{ count }}
    </h1>
    <div v-if="count > 4">
      4보다 큽니다!
    </div>
  </template>
  ```
  
  - 조건
    
    클릭하여 숫자를 증가시킬때, 해당 숫자가 4보다 크면 div의 내용(4보다 큽니다!)를 보여준다.
    
  - v-if: if 속성이다. 해당 속성의 값으로 원하는 조건을 작성한다.
  - `v-` 형태를 **디렉티브(Directive)**라고 부른다.

- style을 scss로 읽어들이기
  
  style 태그에 lang 속성의 값으로 scss를 입력해주면 scss로 읽어들인다.
    
- **반복문 (v-for)**
  - fruits
    
    script에서 fruits라는 배열 데이터를 생성한다.
    
  - v-for
    
    ```jsx
    <template>
      <ul>
        <li
          v-for="fruit in fruits"
          :key="fruit">
          {{ fruit }}
        </li>
      </ul>
    </template>
    
    <script>
    export default {
      data() {
        return {
          fruits: ["Apple", "Baanan", "Cherry"]
        }
      }
    </script>
    ```
    
    for 속성이다. 해당 속성의 값으로 원하는 반복 조건을 입력한다.
    
    `fruit in fruits`  
    나는 조건으로 fruits 배열 데이터를 불러들여 해당 배열안에 존재하는 데이터 값만큼 반복하도록 했다.  
    그리고 해당 배열 안의 값을 fruit 변수에 할당했다.
    
    `:key=”fruit”`  
    데이터를 반복할 때, 각각의 데이터가 고유한지 증명하기 위해서 `:key=””`형태로 제공해야 한다. 반복을 할 때마다 key안의 값 fruit에 Apple, Banana, Cherry가 들어가게 된다.
    

    > **참고!!**
    > 
    > 
    > vue.js, react, svelt, angular 같은 모던 웹 프론트엔드 프레임워크들은  데이터를 기반으로 해서 화면이 출력되는 것을 고려해야 한다.
    > 

- 별개의 components로 데이터 출력하기
  - components 폴더에 Fruit.vue 파일 생성
  - Fruit.vue
    
    props 옵션을 설정한다.   
    해당 객체 데이터 내부에는 name 이라는 이름으로 어떤 데이터를 받아낸다. name이라는 데이터의 타입은 String이다. 
    
    **노란색 밑줄 발생!**  
    name이라는 props는 필수로 default value를 제공해야 한다.  
    우리는 어떤 데이터를 name에 받아서 사용할 것인데, name에 데이터가 들어오지 않는다면 default 속성에 기본값을 지정해 줄 수 있다.
    
    ```jsx
    <template>
      <li>{{ name }}</li>
    </template>
    
    <script>
    export default {
      props: {
        name: {
          type: String,
          default: ''
        }
      }
    }
    </script>
    ```
      
  - Fruit.vue 파일 App.vue 파일로 가져오기
    - App.vue
        
        ```jsx
        <template>
          <ul>
            <hello
              v-for="fruit in fruits"
              :key="fruit"
              :name="fruit">
              {{ fruit }}
            </hello>
          </ul>
        </template>
        
        <script>
        import Fruit from '~/components/Fruit'
        
        export default {
          components: {
            hello: Fruit
          }
        </script>
        ```
        
        script 부분에 import를 사용하여 Fruit 라는 이름으로 파일을 가져온다.
        
        Fruit 이름으로 가져온 파일을 사용하기 위해서는 components 옵션 부분에 등록을 해줘야 한다. 나는 hello 라는 이름으로 등록해줬다.
        
        template 부분에 li를 hello로 변경한다.  
        key 속성 밑에 name 속성을 작성하고 값은 fruit를 명시한다.
        
      - Fruit라는 컴포넌트를 만들어서 해당 내용을 App.vue에 가져와서 활용을 한다.
      - 이때 우리가 사용한 name 속성은 Fruit의 props라는 옵션에 작성되어 있다.  
      name이라는 이름으로 데이터를 받는데, 해당 데이터 타입이 문자열(String)데이터 이고, 여기에 데이터가 들어오지 않으면 빈 문자를 사용하겠다고 선언했다.  

        이렇게 받아진 name이라는 이름으로 데이터를 출력하게 된다.  
        이때 출력하는 name 뒤에 내용을 추가하면 해당 내용도 같이 출력된다.
        
        ```jsx
        <template>
          <li>{{ name }} ?! </li>
        </template>
        ```
        
  - hello는 직관성이 떨어지니 Fruit로 변경하자
    
    Fruit로 변경하면 컴포넌트 옵션에서 지정할 때 파일 이름과 지정해준 이름이 동일하다. 이럴때는 이름 하나만 작성해도 된다.
    
    ```jsx
    components: {
      // Fruit: Fruit
      Fruit
      }
    ```
      
  - Fruit 파일의 style
    
    ```jsx
    <style lang="scss">
      h1 {
        color: red !important;
      }
    </style>
    ```
    
    해당 코드를 작성하고 브라우저 화면을 확인하면 숫자 색상이 빨간색으로 변경되었다.  
    이것은 App.vue에 만들어져있는 h1의 해당하는 스타일을 다른 컴포넌트에서 제어한 것이다. 이런 방식은 그닥 효율적이지 못하다.
    
    - **scoped**
      
      style 부분에 scoped라는 속성을 추가한다.  
      다시 브라우저를 확인하면 숫자 색상이 파란색으로 변경된 것을 볼 수 있다.
      
      scoped가 작성된 해당하는 vue 파일의 스타일은 다른 컴포넌트에는 영향을 미치지 않는다.  
      현재 파일에 있는 내용에만 영향을 미친다.
      
      h1은 Fruit 파일에는 존재하지 않으니 다른 컴포넌트인 App.vue에는 영향을 미치지 못하게 된다.
      
      즉, 해당 스타일이 가진 유효범위는 해당 파일 내에서만 가진다는 것이다.