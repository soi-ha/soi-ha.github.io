---
layout: post
categories:
  - TIL
title: "Unit Test: 비동기 테스트"
tags:
  - TIL
  - Test
---

## __비동기 테스트__

- example.js
  
  ```jsx
  export function asynFn() {
    return new Promise(resolve => {
      setTimeout(() => {
        resolve('Done!')
      }, 2000)
    })
  }
  ```
  
  실제 주는 값: Done!
    

### __example.test.js__

- 비정상적인 테스트 결과
  
  ```jsx
  import { asynFn } from './example';
  
  describe('비동기 테스트', () => {
    test('done', () => {
      asynFn().then(res => {
        expect(res).toBe('Done?')
      })
    })
  })
  ```
  
  일반적으로 우리가 아는 비동기 패턴으로 테스트를 했을 때, 테스트가 정상적으로 동작하지 않는다. 실제 주는 값은 Done!이고 기대되는 값은 Done?인데 테스트가 패스됐다. 즉, 테스트가 정상적으로 동작하지 않는 것이다.
  
  특정한 test 내부에 비동기 코드를 작성했는데, 해당 내용을 테스트 입장에서는 얼마나 기다려야 하는지 알수가 없기 때문에 기다리지 않고 바로 테스트를 진행해서 올바르지 않음에도 테스트가 패스되었다.
    
- 정상적인 비동기 코드: __done__
    
  ```jsx
  import { asynFn } from './example';
  
  describe('비동기 테스트', () => {
    test('done', (done) => {
      asynFn().then(res => {
        expect(res).toBe('Done?')
        done()
      })
    })
  })
  ```
  
  비동기 테스트를 진행할 수 있는 매개변수 done을 사용한다.  
  done은 비동기 테스트가 언제 종료되는지 명시해준다. 마치 함수처럼 실행할 수 있다.  
  test의 콜백의 done이라는 매개변수를 비동기 코드가 종료되는 다음 부분에 함수처럼 작성해준다. 해당 test가 done 매개변수가 호출돼서 동작하기 전까지는 무작정 기다리게 된다. asynFn 함수가 2초뒤에 실행이 되고 그 다음에 done 매개변수가 실행되면서 test가 종료된다.
    
- 정상적인 비동기 코드: __then__
  
  ```jsx
  test('then', () => {
      return asynFn().then(res => {
        expect(res).toBe('Done?')
      })
    })
  ```
  
  하나의 비동기 코드를 return키워드로 반환해준다. 반환해주게 되면서 해당 코드 부분들이 test에서는 비동기로 동작하게 된다는 것을 인지하게 된다.  
  해당 비동기 코드가 동작할때까지 기다리고 test를 동작시킨다.
    
- 정상적인 비동기 코드: __resolves__
  
  ```jsx
  test('resolves', () => {
      return expect(asyncFn()).resolves.toBe('Done!')
    })
  ```
  
  expect 안에 asyncFn 비동기 함수를 실행해서 여기서 나온 값을 바로 toBe로 비교를 한다.  
  expect와 toBe 사이에 resolves 브릿지를 추가한다. 해당 브릿지로 비동기코드가 완료될때까지 기다리게 해준다.
  
  return 없이 사용해도 되지만, 그렇게 되면 테스트 환경에서 얼마나 기다려야하는지 알 수가 없기 때문에 콜백 밖으로 반환해준다.
    
- 정상적인 비동기 코드: __async/await__
    
  ```jsx
  test('async/await', async () => {
      const res = await asyncFn()
      expect(res).toBe('Done!')
    })
  ```
  
  async/await를 사용하여 비동기 코드를 작성한다.  
  비동기 함수 앞에 await를 작성해주고 await 키워드가 붙어있는 함수 부분에 async를 붙여준다.
  
  await 키워드가 있는 부분에서 resolve의 값이 반환된다. 해당 값을 res 변수에 넣는다. res를 expect의 인수로 받아서 사용한다.
  
  async와 await를 사용해서 비동기 코드를 작성하는 것을 가장 추천한다.
    
- test의 세번째 인수
  
  만약 비동기 함수를 2초가 아닌 6초를 기다리게 한 후에 작동하게 만든다면 어떤 일이 발생할까? 테스트 fail이 발생한다.  
  테스트는 최대 5초까지만 가능하다. 따라서 5초가 되어도 테스트가 종료되지 않았기에 test fail이 발생한 것이다.
  
  그렇다면, 6초 뒤에 동작하게는 어떻게 할까? test의 세번째 인수를 추가해준다.
  
  ```jsx
  test('async/await', async () => {
    const res = await asyncFn()
    expect(res).toBe('Done!')
  },7000)
  ```
  
  세번째 인수에는 우리에게는 안 보이게 기본값으로 5000(5초)가 설정되어있다. 따라서 세번째 인수에 원하는 숫자를 작성하면 해당 시간만큼 더 기다릴 수 있게 된다.