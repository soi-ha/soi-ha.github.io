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
  
  ```jsx
  <template>
    <h1> {{ count }}</h1>
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