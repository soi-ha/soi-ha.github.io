---
layout: post
categories:
  - TIL
title: "Unit Test: Jest Globals, Matcher"
tags:
  - TIL
  - Test
---

## __첫 테스트__
---

- package.json
  
  ```json
  "scripts": {
    "dev": "webpack-dev-server --mode development",
    "build": "webpack --mode production",
    "lint": "eslint --fix --ext .js,.vue",
    "test:unit": "jest --watchAll"
  }
  ```
  
  test 스크립트를 추가한다. 단, 우리가 현재 진행할 테스트는 unit 테스트이기에 뒤에 unit 테스트임을 명시한다.
  
  jest 명령을 사용하는데 테스트가 감시돼서 반복적으로 사용될 수 있도록 한다.  
  모든 테스트 파일의 변경사항을 감시해서 변경사항 발생시, 테스트 환경을 재동작 시킨다.
  
- test 실행
    
  ```bash
  npm run test:unit
  ```
  
  **테스트가 통과됐을 때, 나타나는 화면**

  <img width="385" alt="테스트 통과" src="https://user-images.githubusercontent.com/77609591/224556216-6f5ca399-761a-434a-bda4-dcc966dde392.png">

  
  **테스트가 실패할 때, 나타나는 화면**

  <img width="443" alt="테스트 실패" src="https://user-images.githubusercontent.com/77609591/224556222-9d4c1abf-2bdc-4a38-9dea-1ca6b97aefd0.png">
  
  
  expect는 실제 받는 값(Received), toBe는 받을 것으로 기대되는 값(Expected)이다.  
  기대되는 값은 문자 123인데, 실제 받은 값은 숫자 123이기에, 일치하지 않아서 테스트가 실패한 것이다.

## __Jest Globals__
---

> [Globals · Jest](https://jestjs.io/docs/api)

### **Globals Methods**

```jsx
import { double } from './example'

describe('Group1', () => {
  beforeAll(() => {
    console.log('beforeAll!')
  })
  afterAll(() => {
    console.log('afterAll!')
  })

  beforeEach(() => {
    console.log('beforeEach!')
  })
  afterEach(() => {
    console.log('afterEach!')
  })

  test('첫 테스트', () => {
    console.log('첫 테스트!')
    expect(123).toBe(123)
  })
  
  test('인수가 숫자 데이터입니다', () => {
    console.log('인수가 숫자 데이터입니다!')
    expect(double(3)).toBe(6)
    expect(double(10)).toBe(20)
  })
  
  test('인수가 없습니다', () => {
    console.log('인수가 없습니다!')
    expect(double()).toBe(0)
  })
})
```

- `test()`
  
  우리가 만들어내는 각각의 테스트를 구분해주는 용도로 사용한다.
  
  첫번째 인수로는 테스트의 이름, 두번째 인수로는 콜백 함수를 작성하여 콜백 내부에서 실제 테스트를 진행한다.
  
- `describe()`
    
  여러개의 test 함수들을 describe 함수로 묶을 수 있다. 일종의 test 그룹이라고 생각하면 된다.
  
  첫번째 인수로는 해당 그룹의 이름, 두번째 인수로는 콜백함수를 작성하여 내부에는 여러가지 test 함수들을 넣어줄 수 있다.
  
  그룹으로 묶는 이유는 before나 after 전역함수를 사용하기 위함이다.
    
- `beforeAll()` `afterAll()`
    
  before, after를 가지는 전역함수로는 별도의 이름(첫번째 인수)을 작성해주지 않는다. 대신 첫번째 인수로 콜백을 명시한다.
  
  - `beforeAll()`
    
    그룹안에 있는 모든 테스트가 실행하기 전에, 단 한번만 실행이 된다.
    
  - `afterAll()`
      
    그룹안에 있는 모든 테스트가 실행된 후에, 단 한번만 실행이 된다.
      
- `beforeEach()` `afterEach()`

  - `beforeEach()`
      
    각각의 테스트가 동작하기 직전에, 해당 메소드가 한 번 실행된다. 
    
    테스트가 3개라면, 각 테스트가 동작하기 전에 실행되므로 총 3번 실행된다.
      
  - `afterEach()`
    
    각각의 테스트가 동작한 직후에, 해당 메소드가 한 번 실행된다.
    
    테스트가 3개라면, 각 테스트가 동작한 후에 실행되므로 총 3번 실행된다.

## __Jest Matcher 이해__
---

> [Expect · Jest](https://jestjs.io/docs/expect)

### **Expect Methods**

기대값과 실제 주어진 값을 비교해주는 메소드들을 Matcher라고 부른다.
한글로는 일치도구라고 할 수 있다.

```jsx
const userA = {
  name: 'Soha',
  age: 39
}
const userB = {
  name: 'Momo',
  age: 23
}

test('데이터가 일치해야 합니다', () => {
  expect(userA.age).toBe(39)
  expect(userA).toEqual({
    name: 'Soha',
    age: 39
  })
})

test('데이터가 일치하지 않아야 합니다', () => {
  expect(userB.name).not.toBe('Soha')
  expect(userB).not.toBe(userA)
})
```

- `.toBe()`
  
  값이 원시형데이터(string, number, boolean 등)인 것을 비교한다.
  
  expect와 함께 사용되며, expect의 인수에는 실제 받은 값, toBe의 인수에는 기대되는 값을 작성한다.
  
  두 값이 일치하는지 비교하고 일치하면 pass, 일치하지 않으면 fail이 뜬다.
    
- `.toEqual()`
    
  참조형 데이터(객체, 배열데이터 등)를 비교할 때 사용한다.  
  데이터가 가지는 실제 값만 동일한지 확인 할때 사용한다. 실제 객체 데이터, 배열 데이터의 내부적인 구조만 비교해서 같은지 비교하는 것이다.
  
  사용 방법은 toBe와 동일하다. 
    
- `.not`
  
  expect 함수 뒤에 not 속성을 추가하여 기대값을 일치시키는 메소드들의 반대값을 명시할 수 있다.
  
  중간에 not이라는 키워드를 사용해서 부정된 결과를 확인할 수 있다.  
  `expect(can1).not.toBe(can2)`