---
layout: post
published: true
categories:
  - TIL
title: 'JavaScript: `Object.freeze` vs `Object.deepFreeze` '
tags:
  - TIL
  - JS
  - Wooteco
---

프리코스때 처음 알게 되었고, 지금까지도 항상 사용해왔던 `Object.freeze`...  
이번에 온보딩 미션인 자동차경주 페어 덕분에 `Object.deepFreeze`라는 것을 새롭게 알게 되었다.  
비슷하게 생긴 둘은 도대체 무슨 차이가 있는지 알아보자.

## freeze

freeze는 객체를 얕게 얼린다. 그렇기에 객체 프로퍼티는 얼리지만 객체 안의 객체를 얼리지 않아, 변경이 가능하게 된다.

```js
const person = {
	name: 'soha',
	hobby: ['drawing', 'swimming'],
};
```

위의 예시에서는 name은 변경이 불가능하지만, hobby는 변경이 가능하다.  
hobby의 값은 객체 안의 객체이기 때문이다.

### 그렇다면 freeze는 왜 중첩된 객체들까지 얼리지 못하는 걸까?

JavaScript라는 언어의 유연성을 유지하면서 성능과 복잡성을 고려하여 설계했기 때문이다.

동적인 언어이기 때문에 객체의 구조가 런타임 중에도 동적으로 변경될 수 있다. 그렇기에 객체의 중첩된 프로퍼티를 얼리는 것이 복잡하고 예측하기 어렵다.

따라서 JavaScript는 성능, 유연성 및 설계 복잡성 등을 고려하여 중첩된 객체까지 얼리지 않는 설계를 채택했다. 필요한 경우 사용자 정의 함수를 작성하여 깊은 얼림을 구현할 수 있다.

## deepFreeze

위에서 `사용자 정의 함수를 작성하여 깊은 얼림을 구현할 수 있다.`라고 했다. 그렇다. `deepFreeze`는 사용자가 정의하여 사용해야 하는 함수이다.

`deepFreeze`는 `freeze`를 재귀적으로 돌면서 모든 객체와 그 하위 객체까지 깊게 얼린다. 따라서 `deepFreeze`는 객체 안의 객체도 변경이 불가능하다.

### 그렇다면 deepFreeze가 무조건 좋은거 아니야?

꼭 그렇지만은 않다.  
`deepFreeze`는 재귀를 사용하여 구현하기 때문에 많은 메모리와 처리 시간을 소비할 수 있기에 비용이 더 커질 수 있다.

그렇기에 프로젝트에 따라서 `deepFreeze` 사용이 좋지 않을 수도 있다.

`freeze`와 `deepFreeze` 중에서 어떤 것을 사용할 지는 프로젝트의 규모에 따라서 달라질 것 같다!

> 참고자료  
> [객체의 확장을 막는 방법](https://velog.io/@ansrjsdn/%EA%B0%9D%EC%B2%B4%EC%9D%98-%ED%99%95%EC%9E%A5%EC%9D%84-%EB%A7%89%EB%8A%94-%EB%B0%A9%EB%B2%95)
