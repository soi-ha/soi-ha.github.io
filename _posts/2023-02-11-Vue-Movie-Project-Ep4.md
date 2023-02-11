---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: flex-shrink, axios"
tags:
  - TIL
  - Vue.js
  - project
---

## __flex-shrink: 감소비율 없애기__
---

### __의문__

apply 버튼의 사이즈는 120px인데, 다른 select 사이즈와 동일함에도 더 작아보인다. 이유는 뭘까?

가로의 넓이가 정해져있기 때문에, 마지막 요소인 apply는 정해진 가로 넓이 안에 들어오기 위해서 버튼 본인의 가로 사이즈를 줄였다.

어떤 상황에서도 사이즈를 줄이지 않게 설정하려면 어떻게 할까?

### __해결__

flex-shrink를 통해서 설정해주면 된다.  
기본값은 1로, 사이즈가 유동적으로 변할 수 있도록 되어있다. 해당 값을 0으로 변경하면 어떤 상황에서도 정해둔 사이즈가 변하지 않게 된다.

## __axios: http 요청__
---

```bash
npm i axios
```

npm을 통해서 axios를 설치한다.

script 상단에 import를 통해서 axios를 호출하고, apply 메소드 부분에 axios의 get을 사용하여 API Key를 요청받는다.

```jsx
methods: {
    apply() {
      // 영화검색 기능
      axios.get(`https://www.omdbapi.com/?apikey=${}&`)
    }
  }
```

### __추가__

요청 내용을 비동기로 동작시키기 위해서 async와 await 키워드를 추가해준다.

```jsx
methods: {
    async apply() {
      // 영화검색 기능
			const OMDB_API_KEY = 'API KEY 작성'
      const res = await axios.get(`https://www.omdbapi.com/?apikey=${}&`)
    }
  }
```