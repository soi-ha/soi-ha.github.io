---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: 영화검색 (변이 메소드, 검색 추가요청, lodash: uniqBy)"
tags:
  - TIL
  - Vue.js
  - project
  - Vuex
  - lodash
---

## __영화검색__
---

- Store의 Mutations를 실행할 때는 `.commit()` 메소드를, Actions를 실행할 때는 `.dispatch()` 메소드를 사용한다.

- **변이 메소드 (updateState)**
    
    ```jsx
    export default {
      namespaced: true,
      state: () => ({
        movies: [],
        mesaage: '',
        loading: false
      }),
      getters: {},
      mutations: {
        updateState(state, payload) {
          // ['movies', 'message', 'loading']
          Object.keys(payload).forEach(key => {
            state[key] = payload[key]
          })
        }
      },
      actions: {
        async searchMovies(context, payload) {
          const { title, type, number, year } = payload
          const OMDB_API_KEY = '키값입력'
          
          const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=1`)
          const { Search, totalResults } = res.data
          context.commit('updateState', {
            movies: Search,
            message: 'Hello World!',
            loading: true
          })
        }
      }
    }
    ```
    
    actions 부분에서 데이터를 갱신하기 위해서 mutations에 있는 updateState를 commit 메소드를 통해서 실행할 수 있다.  
    실행할 때, 기본적인 객체 데이터(`moives: Search …`)를 담아서 전달을 해줄 수 있다.
    
    이것은 mutations의 payload 변수로 전달이 된다. 이, payload는 하나의 객체 데이터이며, `Object.keys`를 통해서 배열 데이터로 만든다.   
    배열 데이터의 갯수만큼 `forEach()`로 반복한다. 한번 반복할 때마다, 첫번째 key는 movies이고, 이때 state에 있는 movies(`state[’movies’]`)라는 데이터의 payload에 있는 movies (`payload[’movies’]`) 데이터 즉, (actions의 commit 객체 데이터) Search라는 OMDb API에서 가지고 온 데이터를 payload에서 state로 할당해줄 수 있다.
    
    두번째 반복이 될 때는, key가 message이고 `state[’message’]`는 `payload[’message’]` 즉, `‘Hello World!’` 문자 데이터로 할당한다.
    세번째도 마찬가지.
    
    updateState라는 공용화하여 사용할 수 있는 로직을 만들어 둔다면, 어디에서든지 저장소에 있는 데이터를 변이 메소드를 통해서 수정할 수 있다.
    
    - 조금 더 깔끔하게 정리
      
      commit 메소드는 context에서 가져와서 사용한다.  
      이 context를 객체 구조분해를 통해서 commit 메소드만 꺼내서 사용하면 조금 더 깔끔하게 정리가 가능하다.
      
      ```jsx
      actions: {
          async searchMovies({ commit }, payload) {
            const { title, type, number, year } = payload
            const OMDB_API_KEY = '키값입력'
            
            const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=1`)
            const { Search, totalResults } = res.data
            commit('updateState', {
              movies: Search
            })
          }
        }
      ```
      
## __영화 검색 추가 요청__
---
  
첫번째 페이지에는 총 10개의 영화가 검색되어 출력된다.  
우리가 20개, 30개까지 검색이 가능하도록 했는데, 10개가 넘어가면 2페이지, 3페이지로 넘어가면서 검색이 된다. 

우리는 처음 검색한 1페이지에서 추가로 스크롤하면 다음 검색 페이지가 이어서 나오도록 만들 것이다.

```jsx
const total = parseInt(totalResults, 10)
const pageLength = Math.ceil(total / 10)
```

pageLength는 우리가 검색한 키워드를 모두 출력하고자 할때, 한 페이지에 결과를 10개씩 요청했을 때 반복해야 하는 횟수이다. 우리가 검색한 결과의 갯수에서 10을 나누어 올림해준다.

```jsx
// 추가 요청
if (pageLength > 1) {
  for (let page = 2; page <= pageLength; page += 1 ) {
    const res = await axios.get(`https://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${title}&type=${type}&y=${year}&page=${page}`)
    const { Search } = res.data
    commit('updataState', {
      movies: [...state.movies, ...Search]
    })
  }
}
```

pageLength, 즉 우리가 추가 검색을 요청하려는 횟수가 1 이상일때 추가 요청이 발생한다.   
기본은 page가 1부터 시작하니까 우리가 다음로 찾아야 하는 시작 페이지는 2페이지이다. 따라서 변수 page는 2부터 시작한다. page가 pageLength보다 작거나 같을때 까지만 반복하도록 한다. 

추가 요청이 들어왔으니 res 변수를 다시 호출해서 API를 불러온다.  
get 메소드 안의 마지막 줄에 page=1이었으나, 추가 요청을 함에 따라 페이지의 숫자가 증가하니 `${page}`로 변경해준다. 요청이 늘어날때 마다 증가하는 변수(page)의 값이 들어간다.

movies에는 최초의 요청을 통해서 들어온 첫 페이지의 10개의 영화목록이 존재한다. 추가적으로 응답이 온 데이터를 movies 데이터 뒤쪽으로 밀어넣어 줘야 한다.  
commit 메소드를 실행하여 updateState 변이 메소드를 실행해준다. movies 데이터를 수정한다고 선언한다.  
추가적으로 들어온 Search를 바로 movies에 할당하면 기존에 존재했던 데이터들은 덮어져서 사라진다. `movies: Search`

따라서, 빈 배열 데이터를 만들어주고 내부에서 전개 연산자(`…`)를 실행해서 state에 있는 기존의 movies 데이터를 먼저 전개해준다.  
다음에 추가로 가지고온 Search도 전개연산자를 통해서 뒤쪽에 새롭게 데이터를 전개해준다. 이렇게 되면 새로운 배열로 movies에 배열을 할당하게 된다.  
`movies: [...state.movies, ...Search]`  
추가로, state 오류가 발생하는데 searchMovies에 state 사용 선언이 안되어있기 때문에 context 부분에 state 사용을 선언해주면 된다.  
`async searchMovies({ state, commit }, payload)`
  
## __중복 ID 제거 (Lodash: uniqBy)__
---
  
>[Lodash Documentation: uniqBy](https://lodash.com/docs/4.17.15#uniqBy)

lodash의 uniqBy라는 API 명령을 사용한다.

특정한 속성의 이름을 가지고 배열 데이터를 고유화 시켜줄 수 있는 API이다.  
배열 데이터안에 들어있는 각각의 객체 데이터의 특정한 속성 이름을 기준으로 하여 배열 데이터를 고유화 시켜주는 기능을 가진다.

x라는 속성의 값이 중복되고 있는 배열 데이터가 첫번째 인수이며, 두번째 인수는 속성의 이름이다. uniqBy가 두번째 인수의 속성 이름을 확인해서 중복되는 부분들을 제거해준다. 결과적으로 중복이 제거된 새로운 배열데이터가 반환이된다. 

- lodash 설치
    
  ```bash
  npm i lodash
  ```
    
- movie.js
  
  import를 통해서 lodash를 불러온다.  
  uniqBy만 사용할 것이기 때문에 해당 메소드만 호출한다.
  
  ```jsx
  import _uniqBy from 'lodash/uniqBy'
  ```
  
  imdbID는 Search라는 배열 데이터안에 존재하기 때문에 Search를 movies에 할당하기 전에 uniqBy를 통해서 중복을 제거해준다. 
  
  ```jsx
  commit('updateState', {
    movies: _uniqBy(Search, 'imdbID')
  })
  ```