---
layout: post
categories:
  - TIL
title: "Unit Test: 모의(Mock)함수"
tags:
  - TIL
  - Test
---

## __모의(Mock) 함수__
---

테스트에 방해되는 것(소요시간 등)을 가짜의 개념으로 만들어서 테스트를 진행하는 함수를 모의함수라고 한다.

모의로, 임시로 테스트를 하기 위해 모의함수를 만들어서 테스트를 진행한다.  
테스트가 잘 되는지 단순하게 확인할 때, 모든 시간을 기다리는 것은 비효율적이기 때문에 모의함수를 만들어서 실제 걸리는 시간을 단축하여 테스트가 올바른지만 확인한다.

**Mocking**: 실제 함수를 가짜 함수로 만들어서 원하는 데이터만 반환하도록 만들어주는 행위

```jsx
import * as example from './example';

describe('비동기 테스트', () => {
  test('async/await', async () => {
    jest.spyOn(example, 'asyncFn')
      .mockResolvedValue('Done?')
    const res = await example.asyncFn()
    expect(res).toBe('Done!')
  },7000)
})
```

asyncFn 비동기 함수를 모의함수로 만들었다. mockResolvedValue를 통해서 비동기로 실행이 되면, 문자데이터를 반환하도록 설계했다.  
example이라는 객체데이터 내부에서 asyncFn 함수를 실행했을 때, 실제로 반환되는 값이 “Done?” 문자 데이터이다. 테스트 코드를 통해서 “Done!”와 일치하는지 테스트를 하면 일치하지 않기 때문에 실패한다.

example.js 파일에서 asyncFn이 실행되는데 6초가 걸리지만, 해당 시간을 무시하고 내가 원하는 특정 값을 반환하게 가짜로 함수(모의함수)를 만들어서 테스트 로직에서 동작시킨다.

기존에는 7초정도의 시간이 소요됐지만, 모의 함수 생성으로 인해 실제 테스트 시간은 2초가 걸리지 않았다.

- 쓸만한(?) 로직
  
  ```jsx
  import axios from '../node_modules/axios/lib/axios.js';
  import _upperFirst from 'lodash/upperFirst'
  import _toLower from 'lodash/toLower'
  
  export async function fetchMovieTitle() {
    const res = await axios.get('https://omdbapi.com?apikey=7035c60c&i=tt4520988')
    return _upperFirst(_toLower(res.data.Title)) // Frozen ii
  }
  ```
  
  ```jsx
  import axios from 'axios';
  import { fetchMovieTitle } from './example';
  
  describe('비동기 테스트', () => {
    test('영화 제목 반환', async () => {
      axios.get = jest.fn(() => {
        return new Promise(resolve => {
          resolve({
            data: {
              Title: 'Frozen II'
            }
          })
        })
      })
      const title = await fetchMovieTitle()
      expect(title).toBe('Frozen ii')
    })
  })
  ```
  
  해당 코드에서는 모의함수를 작성하지 않아도 테스트가 잘 작동한다. 그렇다면 왜 모의함수를 만들어 준 걸까?
  
  이유는 외부 요인으로 인해 테스트가 불가한 상황을 피하기 위해서이다.  
  axios로 omdbAPI를 불러오는데, 인터넷 연결이 끊기게 된다면 해당 테스트는 통과임에도 실패하게 된다.
  
  모의함수로 데이터를 임의로 만들어주어 테스트함으로, 외부 요인으로 인해 테스트가 불가해지는 상황을 막을 수 있다.