---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: 비동기"
tags:
  - TIL
  - Vue.js
  - project
---

## __비동기__
---

### __콜백 (callback)__
  
  ```jsx
  function a(callback) {
    setTimeout(() => {
      console.log('A')
      callback()
    }, 1000)
  }
  
  function b() {
    console.log('B')
  }
  
  a(function () {
    b()
  })
  ```
  
  callback, 호출 뒤에 실행한다는 것으로. a함수의 로직이 끝난 다음에 b함수 로직을 실행하도록 콜백함수를 사용하였다.
  
  콜백함수를 사용하지 않고 단순히, a와 b를 호출했다면, a의 setTimeout으로  1초뒤에 출력되기 때문에, b가 먼저 출력되고 a가 출력되었을 것이다.  
  하지만, 콜백함수 사용으로 a가 호출되고 나면 b가 실행되는 구조가 되었다. 
  
  어떤 상황에서든 우리가 실행한 함수 순서대로 출력이 되는 것이다. 
  
  - 콜백지옥
    
    ```jsx
    a (function () {
      b (function () {
        c (function () {
          d (function () {
            console.log('Done!')
          })
        })
      })
    })
    ```
    
    기본적인 실행을 보장하고 그 다음에 처리해야 하는 내용이 많아지면, 해당 코드가 점점 깊어진다. 따라서 관리하기가 어려워지게 된다.  
    콜백이 실행의 다음 순서를 보장하는 장점을 가지고 있지만 사용 패턴이 불편하다. 따라서 해당 부분을 보완하기 위해서 Promise 객체를 사용한다.
      
### __Promise 객체__
  
  promise는 약속의 객체이다.
  
  ```jsx
  function a() {
    return new Promise(function (resolve) {
      setTimeout(function () {
        console.log('A')
        resolve()
      }, 1000)
    })
  }
  
  function b() {
    console.log('B')
  }
  
  async function test() {
    await a()
    b()
  }
  
  test()
  ```
  
  promise 객체의 생성자(new)를 통해서 내부에 1초뒤에 ‘A’가 출력되도록 작성했다.   
  a 함수가 실행되고 내부에 있는 console.log의 A를 출력하는 로직이 동작한 다음에 다른 코드가 실행되는 것을 보장하려면 콜백에서 callback()을 호출한 것 처럼 resolve라는 매개변수를 호출하면 된다.  
  즉, 단순하게 설명하자면 callback 자리에 resolve를 작성하면 된다.
  
  test 함수 내부에서 a 함수를 기다린(async await)다음에 b함수를 실행해줄 수 있다.  
  promise라는 약속의 객체가 반환이 되면, 해당 자리에  await 키워드를 붙일 수 있다. 해당 부분에서 resolve 매개변수가 실행될 때까지 기다려(await) 줄 수 있다.  
  a 함수가 실행되기 시작해서 마지막에 resolve 매개변수가 호출되기 전까지 기다렸다가, resolve가 호출되면 그 다음 코드로(b 호출) 넘어갈 수 있다.
  
  즉, 실행 순서는 a함수 → resolve → b함수
  
  - 콜백 지옥을 promise 객체로 벗어나기
    
    ```jsx
    async function test() {
      await a()
      await b()
      await c()
      await d()
      console.log('Done!')
    }
    test()
    ```
        
### __예외처리__
  
  **Promise**
  
  비동기를 통해서 로직을 기다렸다가 처리가 끝나면 처리가 온전히 완료되거나 실패될 수 있는데 그것에 대한 결과를 반환하는 것을 말한다.
  
  > [MDN: Promise - JavaScript](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
  
  - 예외처리
    
    async await를 사용할 수 없는 상황에서 해당 방법을 통해 비동기 처리를 한다.
    
    ```jsx
    function a() {
      return new Promise(function (resolve) {
        setTimeout(function () {
          console.log('A')
          resolve()
        }, 1000)
      })
    }
    
    async function test() {
      await a()
      console.log('B')
    }
    
    test()
    ```
    
    - __then__
      
      ```jsx
      function a() {
        return new Promise(function (resolve) {
          setTimeout(function () {
            console.log('A')
            resolve()
          }, 1000)
        })
      }
      
      function test() {
        a().then(() => {
          console.log('B')
        })
      }
      
      test()
      ```
      
      a가 실행된 다음에 b가 실행하는 것을 보장해준다.
      
      ```jsx
      function test() {
        a()
          .then(() => b())
          .then(() => c())
          .then(() => d())
          .then(() => {
            console.log('Done!')
          })
      }
      test()
      ```
      
      비동기 패턴(async await)을 사용할 수 없을 때는, then메소드를 체이닝 형식으로 작성해나가면서 다음 실행을 보장해 나갈 수 있다.  
      이때, then 메소드를 체이닝으로 붙이려면 메소드의 콜백 내부에 새로운 promise 객체가 반환이 꼭 되어야 한다.
        
    - __catch__
      
      reject, promise의 콜백 내부에서 로직이 정상적으로 실행될 수 없을 때, resolve처럼 호출해줄 수 있다.
      
      ```jsx
      function a(number) {
        return new Promise(function (resolve, reject) {
          if (number > 4) {
            reject()
            return
          }
          setTimeout(function () {
            console.log('A')
            resolve()
          }, 1000)
        })
      }
      
      async function test() {
        await a()
        console.log('B')
      }
      
      test()
      ```
      
      number 매개변수가 숫자 4보다 큰 경우에는 로직을 정상적으로 처리하지 않겠다(거부하겠다)는 의미로 reject 매개변수를 호출할 수 있다.  
      reject 매개변수가 실행되면 promise의 콜백에서는 더이상 resolve 매개변수는 동작하지 않는다.
      
      reject 매개변수가 실행됐다는 것은 로직이 완료됐다는 일종의 **이행**으로 이해할 수 있다. reject이 실행되고 resolve가 실행될 수 없는 부분은 일종의 **거부**되었다고 할 수 있다.  
      즉! 간단하게 말하면 reject이 실행되면 resolve는 실행이 되지 않는다는 것이다.
      
      해당 부분을 테스트하기 위해 다음과 같은 코드를 작성한다.
      
      ```jsx
      function test() {
        a()
        .then(() => {
          console.log('Resolve!')
        })
        .catch(() => {
          console.log('Reject!')
        })
      }
      ```
      
      then 메소드가 실행이 되면, resolve 매개변수가 실행이 됐다는 것이다. catch 메소드가 실행이 되면, reject 매개변수가 실행이 됐다는 것이다.
      
      a에 숫자 2를 넣으면 resolve가 실행되고, 5를 넣으면 rejcet이 실행된다. 2를 넣으면 A, Resolve!가 출력되고, 5를 넣으면 Reject!가 출력된다.
      
      - **then 과 catch**
        
        특정함수에서 조건에 맞게 reject와 resolve 매개변수가 실행돼서, 해당 함수가 **잘 처리가 되고 실행할 내용은 then**, 해당 함수가 문제가 있어서 함수의 실행을 **거부했을 때 실행할 내용은 catch**를 사용하여 이후 로직을 만들어 줄 수 있다.
          
        
    - __finally__
      
      ```jsx
      function test() {
        a()
        .then(() => {
          console.log('Resolve!')
        })
        .catch(() => {
          console.log('Reject!')
        })
        .finally(() => {
          console.log('Done!')
        })
      }
      ```
      
      이름 그대로 **마침내**이다.   
      finally는 resolve가 실행되던 reject가 실행이 되던간에 항상 가장 마지막에 실행되는 로직이다.
        
    - try catch 구문 (비동기 패턴)
      
      then, catch, finally로 작성했던 코드를 비동기 패턴으로 다시 작성해 본다.
      
      ```jsx
      async function test() {
        try {
          await a()
          console.log('Resolve!')
        } catch (error) {
          console.log('Reject!')
        } finally {
          console.log('Done!')
        }
      }
      test()
      ```
      
      then과 catch를 try와 catch 구문을 통해 작성이 가능하다.
      
      error가 발생할 수 있는 구문을 try에 작성하고, error가 발생했을 때는 내용을 catch 구문으로 넘겨서 처리할 수 있도록 한다.  
      어떤 상황에서든 항상 마지막에 실행되도록 하는 것은 finally 부분에 작성하여 로직이 실행되도록 한다.
        
### __대기, 이행, 거부__
  - 대기(pending)
    
    이행하지도, 거부하지도 않은 초기 상태.
    
    비동기 함수가 실행되기 시작해서 이행이나 거부가 되기 직전까지의 내용을 전부 **대기**라고 한다.
      
  - 이행(fulfilled)
      
    연산이 성공적으로 완료됨.
    
    기본적인 로직이 완료되면 resolve 매개변수를 실행한다.  
    이것은, 로직이 정상적으로 처리됐다는 것을 의미하기 때문에 resolve 매개변수가 실행된다는 것이 성공적으로 연산이 완료됐다는 **이행**을 의미한다.
      
  - 거부(rejected)
    
    연산이 실패함.
    
    비동기 패턴에서 기본적인 로직에 문제가 발생해서 연산이 실패하면 비동기에 대한 거부를 할 수 있고 reject가 실행됨으로 rejcet이 실행됐다는 것은 **거부**를 의미한다.