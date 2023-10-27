---
layout: post
published: true
categories:
  - TIL
title: 'JavaScript: 함수 선언식, 함수 표현식, 기명 함수 표현식'
tags:
  - TIL
  - WoowaCourse
  - JS
---

프리코스 1주차를 하면서 코드 컨벤션 가이드에 '함수 선언식 대신에 기명 함수표현식을 사용하세요' 라는 문구가 있었다.  
이때, 함수 선언식과 표현식의 차이가 뭐지...?라는 생각이 들었고 찾아보았다.

### 함수 선언식

---

함수 선언식은 호이스팅의 영향을 받는다. 따라서 블록문 밖에서 호출이 가능하다.

> 호이스팅(hoisting): 코드가 실행되기 전, 변수 및 함수선언이 해당 스코프의 최상단으로 끌어 올려지는 것

```js
name(); // 함수가 선언되지 않았어도 정상적으로 작동한다.
function name() {
	// 내용
}
```

### 함수 표현식

---

함수 표현식은 호이스팅되지 않는다. 따라서, 정의된 범위에서 로컬 변수의 복사본을 유지할 수 있다.

```js
name(); // 함수가 선언되지 않았으므로 에러가 발생한다.
let name = function () {
	// 내용
};
```

- 클로저

  클로저는 함수를 실행하기 전에 해당 함수에 변수를 넘기고 싶을 때 사용된다.

  **사용예시**

  ```js
  function tabsHandler(index) {
  	return function tabClickEvent(event) {
  		// 바깥 함수인 tabsHandler() 의 index 인자를 여기서 접근할 수 있다.
  		console.log(index); // 탭을 클릭할 때 마다 해당 탭의 index 값을 표시
  	};
  }

  let tabs = document.querySelectorAll('.tab');
  let i;

  for (i = 0; i < tabs.length; i += 1) {
  	tabs[i].onclick = tabsHandler(i);
  }
  ```

- 인자 전달

  일반적으로 함수 표현식은 임시변수에 저장하여 사용한다. 그러나 임시 변수에 할당 할 필요없이 함수에 직접 전달할 수 있다.  
  아래와 같이 콜백함수로 사용할 수 있다.

  **사용예시**

  ```js
  let arr = ['a', 'b', 'c'];
  arr.forEach(function () {
  	// ...
  });
  ```

### 기명 함수 표현식

---

함수 표현식으로 함수를 정의한 것에 이름이 존재한다면 이를 기명 함수 표현식이라고 부른다.

```js
const name = function information() {
	// 내용
};
```

기명 함수 표현식이 일반 함수 표현식과 다른점은 다음과 같다.

- 이름을 사용해 함수 표현식 내부에서 자기 자신을 참조할 수 있다.

- 기명 함수 표현식 외부에선 그 이름을 사용할 수 없다.

```js
let sayHi = function func(who) {
	if (who) {
		alert(`Hello, ${who}`);
	} else {
		func('Guest'); // func를 사용해서 자신을 호출한다.
	}
};

sayHi(); // Hello, Guest

// 하지만 아래와 같이 func를 호출하는 건 불가능하다.
func(); // Error, func is not defined (기명 함수 표현식 밖에서는 그 이름에 접근할 수 없다.)
```

> 참고  
> [객체로서의 함수와 기명 함수 표현식](https://ko.javascript.info/function-object#ref-4)  
> [함수 표현식 VS 함수 선언식](https://velog.io/@bisu8018/%ED%95%A8%EC%88%98-%ED%91%9C%ED%98%84%EC%8B%9D-VS-%ED%95%A8%EC%88%98-%EC%84%A0%EC%96%B8%EC%8B%9D)  
> [함수 표현식 vs 함수 선언식](https://joshua1988.github.io/web-development/javascript/function-expressions-vs-declarations/)
