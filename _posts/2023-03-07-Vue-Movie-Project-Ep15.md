---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: Vue Router 정리"
tags:
  - TIL
  - Vue.js
  - project
  - Vue
---

> [Vue Router](https://router.vuejs.org/api/)

### __컴포넌트__

- `RouterView`
    
  페이지가 출력(렌더링)되는 **영역** 컴포넌트이다.
  
  달라지는 페이지의 내용들을 어떤 영역에 출력할 것인지를 RouterView 컴포넌트를 사용한 위치에 지정해 줄 수 있다.
    
- `RouterLink`
  
  페이지 이동을 위한 **링크** 컴포넌트이다.
  
  html에서 a태그 대신에 사용했으며, RouterLink가 제공하는 다양한 기능들을 통해서 페이지 이동을 조금 더 쉽고 편리하게 만들수 있다.
    

### __객체 데이터__

- `$route`
  
  페이지(Route)의 여러 정보를 가지는 객체이다.
  
  대표적으로 fullPath, params 속성을 이용했다.  
  fullPath는 접근된 해당 페이지에 대한 전체 경로, params는 접근된 페이지의 파라미터 정보가 들어있다.
  
  즉, $route는 접근된 페이지의 여러 정보를 **조회**하는 용도의 객체이다.
  
  $route에는 여러가지 **속성**이 들어가 있다.
    
- `$router`
  
  페이지(Route)의 조작을 위한 객체이다.
  
  대표적으로 push 메소드가 있는데, 해당 메소드를 사용해서 RouterLink 컴포넌트 대신에 페이지 이동을 만들었다.  
  push를 통해서 페이지를 이동시킨 동작을 만든 것이다.
  
  즉, 이동이라는 건 페이지를 **조작**하는 하나의 행동이다.
  
  $router에는 여러 **메소드**가 들어있다.