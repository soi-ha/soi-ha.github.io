---
layout: post
published: true
categories:
  - TIL
title: 'JavaScript: Number.isNaN vs isNaN '
tags:
  - TIL
  - JS
  - Wooteco
---

## NaN

NaN은 Not-A-Number(숫자가 아님)을 나타내는 전역 객체의 속성이다. Number.NaN의 값과 같다.

### NaN을 반환하는 다섯 가지 연산 종류

- 숫자로 변환 실패

  예시: `parseInt("blabla")`, `Number(undefined)`와 같은 명시적인 것 또는  
  `Math.abs(undefined)`와 같은 암시적인 것

- 결과가 허수인 수학 계산식

  예시: Math.sqrt(-1)

- 정의할 수 없는 계산식

  예시: `0 * Infinity`, `1 ** Infinity`, `Infinity / Infinity`, `Infinity - Infinity`

- 피연산자가 NaN이거나 NaN으로 강제 변환되는 메서드 또는 표현식

  예시: `7 ** NaN`, `7 * "blabla"`  
  이것은 NaN이 전염성 있다는 것을 의미한다.

- 유효하지 않은 값이 숫자로 표시되는 기타 경우

  예시: 잘못된 날짜 `new Date("blabla").getTime()`, `"".charCodeAt(1)`

## isNaN()

`isNaN()`은 값이 숫자가 아니라면 형 변환을 한다.  
즉, 숫자로 변환만 할 수 있다면 숫자가 아닌 문자열이 들어와도 true를 반환한다.

형변환을 진행하기 때문에 `isNaN()` 보다는 `Number.isNaN()`을 사용하는 것이 권장된다.

```js
isNaN(NaN); // true
isNaN(Number.NaN); // true
isNaN(0 / 0); // true

isNaN('NaN'); // true
isNaN(undefined); // true
isNaN({}); // true
isNaN('blabla'); // true

isNaN('2'); //false
isNaN(2); //false
isNaN(true); // false
isNaN(null); // false
```

## Number.isNaN()

`Number.isNaN()`은 엄격한 방식으로 NaN인지만 확인한다.  
`isNaN()`과 달리, `Number.isNaN()`은 강제로 매개변수를 숫자로 변환하지 않는다. 오직 숫자형이고 NaN인 값에만 true를 반환한다.

```js
Number.isNaN(NaN); // true
Number.isNaN(Number.NaN); // true
Number.isNaN(0 / 0); // true

Number.isNaN('NaN'); // false
Number.isNaN(undefined); // false
Number.isNaN({}); // false
Number.isNaN('blabla'); // false

Number.isNaN('2'); //false
Number.isNaN(2); //false
Number.isNaN(true); //false
Number.isNaN(null); //false
```

> 참고자료  
> [[JS] NaN, isNaN()과 Number.isNaN()의 차이](https://velog.io/@pul8219/JS-NaN-isNaN%EA%B3%BC-Number.isNaN%EC%9D%98-%EC%B0%A8%EC%9D%B4)  
> [MDN: NaN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/NaN)  
> [MDN: isNaN()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/isNaN)  
> [MDN: Numeber.isNaN()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN)
