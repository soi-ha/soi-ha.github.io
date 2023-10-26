---
layout: post
published: true
categories:
  - TIL
title: 'JavaScript: 예외 던지기(throw), 예외처리(try-catch-finally) '
tags:
  - TIL
  - WoowaCourse
  - JS
---

우테코 프리코스를 진행하면서 다음과 같은 요구사항이 존재했다.

> 사용자가 잘못된 값을 입력한 경우 throw문을 사용해 예외를 발생시킨후 애플리케이션은 종료되어야 한다.

throw..?? 이번에 처음 보는 친구였기 때문에 이 기회를 삼아 학습해보기로 했다!

### 예외처리

---

throw를 배우기에 앞서 try, catch, finally에 대해 알아보자.

**예외(exception)** 란 실행 중인 프로그램에서 예기치 못한 상황이 발생하여 더 이상 진행할 수 없는 상황을 말한다. 매우 대표적으로는 SyntaxError나 TypeError 등이 있다.
예외가 발생하면 프로그램을 실행할 수 없기 때문에 예외를 처리할 수 있는 방법을 마련해야 한다.

이런 예외처리는 try-catch 문을 사용하여 처리할 수 있다.

#### try

---

`try`는 에러가 날수도 있는 코드를 담은 것이다.  
해당 블록안에 있는 코드를 실행하여 에러가 발생한다면 `catch` 블록으로 옮겨가고, 에러가 발생하지 않는다면 정상적으로 실행된다.  
예외는 `throw`나 예외를 발생시키는 메서드에 의해 발생된다.

#### catch

---

해당 블록은 `try` 블록에서 예외가 발생한 경우에만 실행된다.  
에러에 대한 정보를 담은 객체(매개변수)를 사용할 수 있다.  
`catch` 블록에서는 예외를 처리하거나, 예외를 무시하거나 (아무것도 하지 않음), `throw `를 사용하여 예외를 다시 발생시킬 수 있다.

- `e.name` : 에러 이름
- `e.message` : 에러 상세 내용을 담고 있는 메시지
- `e.stack` : 현재 호출 스택. 에러를 유발한 중첩 호출들의 순서 정보를 가진 문자열이다. 디버깅을 목적으로 사용한다.

#### finally

---

`try` 블록에서 일어난 일에 상관없이 해당 블록에 있는 코드는 무조건 실행된다. 따라서 `try` 블록이 어떤 형태로든 종료되면 실행된다.  
return, break, continue등으로 코드의 실행 흐름이 즉시 이동되더라도 무조건 실행된다.

에러가 발생하지 않은 경우: try - finally  
에러가 발생한 경우: try - 에러발생 - catch - finally

**try 블록이 종료되는 상황**

- 정상적으로 블록의 끝에 도달했을 때
- break, continue 또는 return 문에 의해서
- 예외가 발생했지만 catch 절에서 처리했을 때
- 예외가 발생했고 그것이 잡히지 않은 채 퍼져나갈 때

### 예외 던지기

---

개발자가 직접 에러를 유발시킨다. 에러나 예외 상황을 알리기 위해서이다.  
따라서 예외를 강제로 발생시켜야 할 경우에는 `throw`를 사용한다. 이를 **예외 던지기**라고 한다.

개발자가 예외를 던지면, 예외 객체가 생성되고, 이 객체는 프로그램 실행 중에 catch 블록에서 처리된다.

예외를 던질 때는 `new Error('에러 메세지')` 와 같이 예외 객체를 생성하여 던진다.

```js
try {
	const even = parseInt(prompt('짝수를 입력하세요'));
	if (even % 2 !== 0) {
		throw new Error('짝수가 아닙니다.');
	}
} catch (e) {
	alert(e.message);
}

// 홀수를 입력할 경우
// Error: 짝수가 아닙니다.
```

> 참고  
> [자바스크립트의 예외 처리(Exception)](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-%EC%98%88%EC%99%B8-%EC%B2%98%EB%A6%AC#%EC%98%88%EC%99%B8_%EB%8D%98%EC%A7%80%EA%B8%B0_throw)  
> [자바스크립트 예외처리](https://ktko.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC)
