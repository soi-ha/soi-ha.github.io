---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: 시작하기, VueRouter, Bootstrap"
tags:
  - TIL
  - Vue.js
  - project
---
## __사이트 소개__
---

OMDb API 활용한 영화를 검색해주는 사이트이다.

페이지는 총 3페이지로 구성되어 있다.

- 영화 검색 페이지 (Search, 메인)
  
  영화를 검색할 수 있는 페이지이다. 
  원하는 키워드로 영화 검색이 가능하며, 3가지 조건을 설정하여 영화를 검색할 수 있다.
    
- 영화 페이지 (Movie)
    
  검색을 통해 나타난 영화를 클릭하면 해당 영화의 상세 설명을 확인할 수 있는 페이지이다.
  
- 개발자 소개 페이지 (About)
  
  해당 페이지를 만든 개발자를 소개한 페이지이다.
    
- 기술

  비동기, 백엔드 API, Vuex, Vue Router 등의 기술등을 적용해서 페이지를 제작할 것이다.

## __Vue Router 구성__
---

**Router**

특정한 페이지를 만들때, 페이지를 구분하기 위해서 사용한다.

>[Vue Router: 소개](https://router.vuejs.kr/introduction.html)

```bash
npm i vue-router@4
```

vue router은 실제 브라우저에서 페이지 관리를 해줘야 하기 때문에 개발의존성이 아닌 일반의존성 모드로 설치한다.

- src/main.js
    
  ```jsx
  import { createApp } from 'vue'
  import App from './App.vue'
  import router from './routes/index.js'
  
  createApp(App)
    .use(router) 
    .mount('#app')
  ```
  
  use()는 플러그인을 연결할때 사용한다.
    
- index.js
  
  페이지를 관리하는 구성파일이 된다.
  
  ```jsx
  import { createRouter, createWebHashHistory } from 'vue-router'
  import Home from './Home'
  import About from './About'
  
  export default createRouter ({
    // Hash
    // https://google.com/#/search
    history: createWebHashHistory(),
    // pages
    // https://google.com/
    routes: [
      {
        path: '/',
        component: Home
      },
      {
        path: '/about',
        component: About
      }
    ]
  })
  ```
  
  - routes
    
    routes 옵션은 페이지를 구분해주는 개념이다.
      
  - history
    
    history 옵션은 hash 모드와 history 모드로 설정이 가능하다.  
    우리는 hash 모드를 사용할 건데, hash 모드는 ‘#’이라는 기호와 함께 슬래쉬를 붙여서 해당 페이지에 접근하는 모드이다.  
    `https://google.com/#/search`  
    해쉬모드를 사용해야지만 특정 페이지에서 새로고침을 했을 때, 페이지를 찾을 수 없다는 메세지를 방지할 수 있다.  
    history 모드는 서버에 세팅을 해야하기 때문에 이번 프로젝트에서 사용하는 것은 보류한다.
      
  - path
    
    path는 페이지를 구분해주는 경로를 의미한다.  
    path 부분의 슬래쉬는 해당 페이지의 가장 메인 페이지에 접근을 하겠다는 의미이다.
      
  - component
    
    메인페이지에 접근했을 때, 어떤 vue.js의 컴포넌트를 사용할 것인지 명시한다.
      
  
  ### 유용한 컴포넌트
  
  - RouterLink
    
    a태그를 대신하여 사용할 수 있는 컴포넌트이다.   
    ’to’를 사용하여 이동하길 원하는 링크 작성한다.

## __Bootstrap 구성__
---

```bash
npm i bootstrap
```

[Sass](https://getbootstrap.com/docs/5.2/customize/sass/)

**!default**

!default 플래그는 scss에서 제공하는 기능으로, 새롭게 지정되는 값이 있으면 기존 값을 무시한다는 의미이다.   
bootstrap에서 지정한 파란색의 Primary 색상을 우리가 외부에서 수정하면 수정한 내용이 출력된다는 (즉, 커스터마이징) 것이다.

- 변수 재정의
    
  재정의 하려는 색상(스타일)은 꼭, required보다 위에 적용해야 한다. 
  해당 import가 실행되기 전에 정의를 해야 bootstrap에서 기존에 정의해둔 기본값을 무시할 수 있기 때문이다.
  
  ```scss
  // Default variable overrides
  $primary: #FDC000;
  
  // Required
  @import "../../node_modules/bootstrap/scss/functions";
  @import "../../node_modules/bootstrap/scss/variables";
  @import "../../node_modules/bootstrap/scss/maps";
  @import "../../node_modules/bootstrap/scss/mixins";
  @import "../../node_modules/bootstrap/scss/root";
  
  @import "../../node_modules/bootstrap/scss/bootstrap";
  ```
    
- $theme-colors
    
  해당 변수 안에 있는 값들을 모두 변경했다면, 위의 $primary와 같이 상단에 작성해준다.  
  만약, 일부만 변경하고 다른 info, warning 등은 변경하지 않았다면, required 부분 뒤에 정의해야 한다. 따로 재정의를 하지 않았기에 앞에 사용하면 에러가 발생하게 된다.