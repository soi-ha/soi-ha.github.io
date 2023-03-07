---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: 검색 정보 초기화 및 페이지 전환 스크롤 위치 복구"
tags:
  - TIL
  - Vue.js
  - project
---

## __페이지 전환 스크롤 위치 복구__
---

> [Vue Router: scrollBehavior](https://router.vuejs.org/guide/advanced/scroll-behavior.html#scroll-behavior)

scrollBehavior을 사용하여 페에지 전환시 스크롤 위치를 원하는 곳으로 옮겨지도록 한다.   
기존에는 홈 화면에서 스크롤을 최하단으로 내린 후, 영화를 선택하면 movie 페이지를 처음 보여줄때 홈에서 스크롤을 내렸던 만큼 화면을 내려서 보여줬다.  
그러나, scrollBehavior을 통해 스크롤을 최상단으로 바꿔준다.

- index.js
  
  ```jsx
  scrollBehavior() {
    return { top: 0 }
  }
  ```
  
  top: 0 설정을 통해 페이지 전환시 가장 최상단으로 스크롤의 위치가 복구된다.
  
  ```jsx
  resetMovies(state) {
    state.movies = [] // 빈 배열로 초기화
    state.message = _defaultMessage
    state.loading = false
  }
  ```
    
## __검색 정보 초기화__
---

movie.js 파일에 기존에 존재하던 resetMovies 함수에 기능을 추가해준다.  
기존에는 홈에서 movie 페이지로 전환 후, 다시 홈 화면으로 뒤로가기를 했을 때 변수 _defaultMessage 문구가 뜨지 않았다.   
state.message를 통해서 문구를 나오게 한다. loading 이모션 또한 false로 변경해준다.

```jsx
resetMovies(state) {
  state.movies = [] // 빈 배열로 초기화
  state.message = _defaultMessage
  state.loading = false
}
```

Home.vue 파일에서 created라는 컴포넌트가 생성된 직후에 동작하는 라이프사이클에서 commit 메소드를 통해서 store안에 있는  movie 파일의 resetMovies라는 변이 메소드를 실행해준다.

```jsx
created() {
  this.$store.commit('movie/resetMovies')
}
```

메소드 실행을 통해서 화면을 리셋할때 배열이 초기화되면서 기본 메세지도 출력된다.