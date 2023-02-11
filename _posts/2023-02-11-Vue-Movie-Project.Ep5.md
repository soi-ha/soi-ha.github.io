---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: Vuex (중앙 집중식 상태 관리)"
tags:
  - TIL
  - Vue.js
  - project
  - Vuex
---

## __Vuex: 데이터를 한 곳에서 처리하기__
---

Search 부분과 MovieList 부분의 형제컴포넌트 관계이다.  
Home.vue에서 사용한 컴포넌트를 확인하면 Search 컴포넌트와 MovieList 컴포넌트는 같은 라인에 존재하는 형제 컴포넌트임을 확인할 수 있다.

부모와 자식관계에서는 props와 emit을 사용하고, 상위와 하위 컴포넌트 관계(부모,자식을 넘어선)에서는 provide와 inject 기능을 통해 데이터를 통신한다.    
그렇다면, 형제 컴포넌트의 경우에는 어떻게 데이터를 통신할까?

- Vuex
  
  데이터를 한 곳에 몰아둔 다음에 데이터를 통신하도록 한다. 해당 방법은 **Vuex**를 사용한다.
  
  Vuex는 **중앙 집중식 상태관리 라이브러리**이다.  
  통상적으로 해당 방법들을 **store**라고 부른다.
    
- 현재 프로젝트 상태
    
  <img width="1000" alt="프로젝트 상태" src="https://user-images.githubusercontent.com/77609591/218268491-30d2cbb5-af96-4f35-8967-882acd621ccb.png">
  
  해당 이미지는 영화 검색 사이트의 상태를 나타내고 있다.  
  우리는 Search 컴포넌트를, Movie 컴포넌트와 MovieList, MovieItem 컴포넌트에서 활용을 할 것이다.  
  Search는 형제 컴포넌트인 MovieList와 데이터 통신을 해야 할 뿐 아니라, 복잡한 관계인 Movie 컴포넌트와도 데이터를 통신해야 한다.  
  이런 복잡한 관계들에서 데이터 통신을 보다 편하게 사용하도록 store(Vuex)를 사용한다.
    
- Store를 사용한 상태
  
  <img width="516" alt="스토어를 사용한 상태" src="https://user-images.githubusercontent.com/77609591/218268496-2e62ccc9-29c2-48e3-9078-24a3435394a1.png">
  
  프로젝트에, 데이터를 중앙집중화(store)할 수 있는 장소를 만든다.  
  스토어에 다이렉트로 해당하는 컴포넌트들의 데이터를 연결해서 사용한다.  
  관계가 복잡해지더라도 중간 매개체(스토어)를 단 하나만 사용하면 된다는 편리함이 있다. 또, 하나의 매개체를 통해서 프로젝트 내부에 있는 어떤 곳의 컴포넌트에도 데이터를 전달할 수 있다는 장점이 있다.
    
- 모듈
    
  우리는 스토어에 영화검색만 할 것이 아니라 about 페이지에서 사용할 기본적인 개발자 정보도 스토어에서 관리할 것이다.  
  스토어에서 관리할 데이터의 종류는 영화의 목록(movie), 영화의 상세 정보(movie), 개발자 정보(about)가 있다. 해당 정보들은 결이 다른 정보들이기 때문에 별도의 모듈(movie, about)로 분리해서 스토어로 관리할 것이다.
  
  모듈로 분리하는 이유는 프로젝트가 커질 수록 취급해야 하는 데이터의 종류들이 달라질 수 있고 훨씬 많은 종류들이 붙을 수 있기 때문에 모듈화를 한다.
    
- 상태
  
  스토어의 개념에서는 데이터를 상태라고 부른다.  
  따라서 중앙 집중식 상태 관리, 데이터를 관리해주는 하나의 개념이며 패턴이라고 부른다.  
  정리하면 중앙 집중식 상태 관리 패턴 라이브러리이며, 우리가 사용할 라이브러리가 Vuex인 것이다.

## __Vuex: store 구성__
---

>[Vuex4: What is Vuex?](https://vuex.vuejs.org/)  
>[Vuex3: Vuex가 무엇인가요?(한글)](https://v3.vuex.vuejs.org/kr/)

- vuex 설치
  
  ```bash
  npm i vuex@next
  ```
    
- store 생성
    
  src 폴더 내부에 store 폴더 생성, 내부에 index.js 파일 생성
  
  - modules
    
    movie, about에 대한 데이터 타입들의 모듈이 연결되는 구조이다.
      
- main.js에 index.js 파일 연결
    
  ```jsx
  import { createApp } from 'vue'
  import App from './App.vue'
  import router from './routes/index.js'
  import store from './store/index.js'
  
  createApp(App)
    .use(router) 
    .use(store)
    .mount('#app')
  ```
  
  **추가 Tip!!**  
  특정한 폴더에 있는 index라는 이름을 가진 파일은 생략이 가능하다!!  
  폴더 이름만 작성하면 index라는 이름의(특히 자바스크립트 파일)의 파일을 우선 적용하는 구조이기 때문이다.  
  따라서, 폴더에서 기본적으로 사용하는 파일의 이름을 정할때는 index 이름을 사용하는 것이 좋다.
  
  ```jsx
  import { createApp } from 'vue'
  import App from './App.vue'
  import router from './routes'
  import store from './store'
  
  createApp(App)
    .use(router) 
    .use(store)
    .mount('#app')
  ```
  
- store 폴더에 movie, about 파일 생성
  - movie.js 파일
    
    영화 검색과 관련된 데이터를 취급한다.
    
    ```jsx
    export default {
      // module
      namespaced: true,
      // data
      state: () => ({
        movies: []
      }),
      // computed
      getters: {
        movieIds(state) {
          return state.movies.map(m => m.imdbID)
        }
      },
      // methods
      // 변이
      mutations: {
        resetMovies(state) {
          state.movies = [] // 빈 배열로 초기화
        }
      },
      // 비동기
      actions: {
        searchMovies() {
        }
      }
    }
    ```
    
    - namespaced
      
      movie.js가 하나의 스토어에서 모듈화돼서 사용될 수 있다는 것을 나타내는 옵션이다.  
      값으로 true를 넣게되면, moive.js를 index.js의 modules 부분에 명시해서 별개의 개념으로 활용할 수 있다.
        
    - state
        
      실제로 취급해야 하는 각각의 데이터들을 의미한다.  
      데이터를 상태(state)라고 부른다!
      
    - getters
      
      계산된 상태를 만들어낸다.  
      계산된 데이터를 만들어내는 computed와 동일하다고 보면 된다.  
      즉, 데이터는 상태(state)이고 comptued는 getters.
        
    - mutations, actions

      methods와 유사하다.  
      해당 옵션 부분에 함수를 만들어서 movie.js에 있는 여러 데이터들을 활용할 수 있다.
        - **mutations**
          
          변이라는 뜻을 가진다. (돌연변이의 변이)  
          mutaions를 통해서 관리하는 데이터를 변경해줄 수 있다.
          
          movie.js에서 관리하는 state에 있는 각각의 데이터들은 mutations로 변경하는 것을 제외하고는 getters나 actions나 다른 컴포넌트에서 데이터가 수정되는 것을 허용하지 않는다는 것을 의미한다.
          
          **즉, 오직 mutations에서만 데이터를 변경할 수 있다!**  
          각 컴포넌트들에서 데이터를 수정할 수 있게 되면 관리가 어려워지기 때문에 mutations에 정의된 개념만을 이용하게 한 것이다.
            
        - actions
          
          데이터 변경을 제외한 메소드를 작성한다.  
          데이터 변경은 mutations에서만 진행한다.
          
          **주의사항! actions는 비동기로 동작한다.**  
          actions에 만들어내는 함수들은 비동기로 처리되도록 구성되어져 있다. 따라서 async, await를 작성하지 않아도 비동기로 처리가 된다!
          
  - about.js 파일
    
    개발자의 정보, 사용자의 정보를 취급한다.