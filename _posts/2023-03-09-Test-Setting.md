---
layout: post
categories:
  - TIL
title: "테스트 환경 설정"
tags:
  - TIL
  - Test
---

- 패키지 설치
  
  ```bash
  npm i -D jest @vue/test-utils@next vue-jest@next babel-jest
  ```
    
- jest.config.js
  
  ```jsx
  module.exports = {
    moduleFileExtensions: [
      'js',
      'vue'
    ],
    moduleNameMapper: {
      '^~/(.*)$': '<rootDir>/src/$1'
    },
    modulePathIgnorePatterns: [
      '<rootDir>/node_modules',
      '<rootDir>/dist'
    ],
    testURL: 'http://localhost/',
    transform: {
      '^.+\\.vue$': 'vue-jest',
      '^.+\\.js$': 'babel-jest'
    }
  }
  ```
  
  - `moduleFileExtensions`
    
    파일 확장자를 지정하지 않은 경우 Jest가 검색할 확장자 목록이다.  
    일반적으로 많이 사용되는 모듈의 확장자를 지정한다.
      
  - `moduleNameMapper`
    
    틸드(~) 같은 경로 별칭을 매핑한다.  
    `<rootDir>` 토큰을 사용해 루토 경로를 참조할 수 있다.
    
    `'^~/(.*)$': '<rootDir>/src/$1’`  
    틸드(~/)의 슬래쉬로 시작하는 경로별칭은 뒤쪽에 있는 값으로 매핑한다. 뒤쪽의 값은 `<rootDir>`은 무시하고 src 디렉토리 안에 있는 모든 경로($1)를 매핑한다는 의미이다.
      
  - `modulePathIgnorePatterns`
    
    테스트 환경에서 무시할 경로들을 불러온다. 즉, 테스틀 하지 않는 것들이다.
      
  - `testURL`
    
    jsdom은 html 환경이라고 이해하면 된다. 환경에서 문제가 생기지 않도록 localhost의 주소를 명시하여 해결한다.
      
  - `transform`
    
    정규 표현식을 통해서 vue,js 확장자를 가지는 파일을 발견하면 테스트 환경에서 동작할 수 있는 새로운 코드로 변환해준다.  
    vue 확장자는 vue-jest 패키지를 통해 변환, js 확장자는 babel-jest 패키지를 통해 변환한다.
      
- example.test.js
  
  해당 파일에 test() 코드를 작성해준다. 그러면 에러가 발생한다.  
  해당 함수는 전역이라 오류가 발생하면 안되는데 eslint로 인해 에러가 발생한다.
  
  - .eslintrc.js 수정
    
    ```jsx
    env: {
      browser: true,
      node: true,
      jest: true
    }
    ```
    
    eslint를 통해서 jest라는 테스트 환경에서 기본적인 문법들이 lint 에러가 발생하지 않도록 옵션을 추가해준 것이다.
    
    해당 수정으로 인해 test 전역 함수의 에러가 발생하지 않는다.