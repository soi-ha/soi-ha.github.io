---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: Vue Router, 404 Page Not Found"
tags:
  - TIL
  - Vue.js
  - project
---

## __Vue Router: 404 Page Not Found__
---

> [Vue Router: 404 not found route](https://router.vuejs.org/guide/essentials/dynamic-matching.html#catch-all-404-not-found-route)

특정한 경로 부분에 정규표현식을 제공해서, 정규표현식과 매칭되는 주소는 NotFound라는 component를 제공해서 메세지를 보여줄 수 있다. 

```jsx
const routes = [
	{ path: '/:pathMatch(.**)**', name: 'NotFound', component: NotFound },
	{ path: '/user-:afterUser(.*)', component: UserGeneric },
]
```

주소부분에 슬래쉬(/) 하나만 작성하게 되면 메인페이지가 된다.
/moive/:id 는 movie페이지에 id라는 파라미터를 가지고 페이지를 동적으로 제어할 수 있다.

404 not found의 경우에는 슬래쉬 뒤에 pathMatch라는 파라미터를 추가해서 뒤에 소괄호 안에 정규식을 작성해 줄 수 있다.  
소괄호 안의 정규표현식은 마침표(.)는 임의의 하나의 문자와 일치, 별표(*) 최대한 많은 문자와 일치이다.  
즉, 우리가 지정한 것 이외에 모든 것들을 매칭해서 notfound 페이지를 보여주게 된다.

위 코드에서는 파라미터 이름이 pathMatch라고 되어있는데 해당 이름으로 사용 안하고 원하는 이름으로 변경해도 된다.