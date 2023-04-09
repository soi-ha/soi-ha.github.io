---
layout: post
categories:
  - TIL
title: 'React: Basic Hook (useState, useEffect)'
tags:
  - TIL
  - React
---

## **Hook**

---

19년도 이전에는 함수형 컴포넌트는 state와 라이프사이클을 지원하지 않았다. 그렇기에 클래스형 컴포넌트를 많이 사용했지만 React v16부터 **Hook**을 통해 state와 라이프사이클 관리가 가능해지면서 공식적으로 함수형 컴포넌트 사용을 권장하기 시작했다.

Hook은 useState를 사용해서 state를 관리할 수 있고, useEffect를 사용해서 라이프사이클을 관리할 수 있다.

Hook은 React 내부에 있는 API이기에 별도로 라이브러리를 설치하지 않아도 된다.

## **useState**

---

class component의 state를 대체 할 수 있다.

**Class Component**

```jsx
export default class Example1 extends React.Component {
	state = { count: 0 };

	render() {
		const { count } = this.state;

		retrun(
			<div>
				<p>You clicked {count} times</p>
				<button onClick={click}>Click me</button>
			</div>
		);
	}

	click = () => {
		this.setState({ count: this.state.count + 1 });
	};
}
```

**Function Component**

useState를 사용해서 function component 안에 특정 값을 state처럼 사용하기

```jsx
export default function Example2() {
	const [count, setCount] = React.useState(0);

	retrun(
		<div>
			<p>You clicked {count} times</p>
			<button onClick={click}>Click me</button>
		</div>
	);

	function click() {
		setCount(count + 1);
	}
}
```

useState()안의 인자가 class 컴포넌트의 count: 0 과 같은 초기값이다. 따라서 인자로 0을 넣어준다.

React.useState(0)를 한 결과물은 배열이다. 배열의 앞 인덱스는 useState의 0, state(count)가 업데이트 됐을 때, 변경된 값을 의미한다. 즉, 앞 인덱스는 count를 작성하면 된다.  
두번째 인덱스는 count를 바꾸는 함수를 제공한다. 따라서 우리는 함수 이름을 setCount라고 명한다.

클릭을 했을 때, setCount가 호출되도록 한다. setCount는 클릭시, count 값이 증가하는 로직을 작성한다.

setCount가 하는 역할은 count의 값을 변경하는 것 + Example2 함수를 다시 실행하는 것이다.  
변경한 값을 적용하고 해당 값에 다시 값을 추가해야 하므로, 해당 함수를 다시 실행하기 때문이다.

- **useState를 객체를 사용하는 것으로 변경하기**

  현재는 `useState ⇒ count` 로 작성했지만 `useState ⇒ { count: 0 };` 로 변경해본다.

  class component의 state객체 처럼 사용하는 것과 유사하다.

  useState의 초기값으로 0이 아닌 state 객체를 넣어준다. `{ count: 0 }`

  인덱스 첫번째 부분은 count가 아니라 `state`라고 부른다.  
  setCount 또한 이름이 정해진 것이 아니고, 우리가 의도를 담아서 이름을 지을 수 있기 때문에 `setState`라고 변경한다.

  이제는 count가 아닌 `state.count`로 변경한다.

  ```jsx
  export default function Example2() {
  	const [state, setState] = React.useState({ count: 0 });

  	retrun(
  		<div>
  			<p>You clicked {state.count} times</p>
  			<button onClick={click}>Click me</button>
  		</div>
  	);

  	function click() {
  		setState({ count: state.count + 1 });
  	}
  }
  ```

  setState에 들어오는 인자가 this.setState와 같이 객체로도 들어올 수 있다. 기존 state 값에 의존적으로 변경하고 싶다면 객체가 아니라, 함수를 하용할 수 있다.

  state를 받아서 새로운 state를 return하는 형식으로 작성한다.

  ```jsx
  export default function Example2() {
    ...
    function click() {
      setState(state => {
        return {
          count: state.count + 1,
        }
      })
    }
  }
  ```

  해당 방식으로 사용하는 것은 중요하다. 추후에 setState만 사용하는 것이 아니라, setState가 어떤 것을 의존해서 사용하고 있는지 중요해진다. 간단하게 말하자면 의존적으로 state를 처리하지 않게 되기 때문에.. (나중에 이게 굉장히 중요해지는 듯) 해당 방식을 필히 이해해야 한다.

## **useEffect**

---

라이프 사이클 훅을 대체할 수 있다. 다만, 대체 할 수 있다는 것이지 동등한 역할을 한다는 것은 아니다.  
useEffect는 여러가지 기능이 있는데 componentDidMount, componentDicUpdate, componentWillUnmount 라이프 사이클 훅을 대체할 수 있다.

**Class Component**

최초의 render가 발생한 직후에 componentDidMount를 구현

```jsx
export default class Example1 extends React.Component {
	state = { count: 0 };

	render() {
		const { count } = this.state;

		retrun(
			<div>
				<p>You clicked {count} times</p>
				<button onClick={click}>Click me</button>
			</div>
		);
	}
	// 최초의 render가 된 직후
	componentDidMount() {
		console.log('componentDidMount', this.state.count);
	}

	componentDidUpdate() {
		console.log('componentDidUpdate', this.state.count);
	}

	click = () => {
		this.setState({ count: this.state.count + 1 });
	};
}
```

최초 접속시 componentDidMount 0 출력  
버튼 클릭 시 componentDidUpdate 1 숫자 증가

**Function Component**

```jsx
export default function Example2() {
	const [count, setCount] = React.useState(0);

	React.useEffect(() => {
		conosle.log('componentDidMount & componentDidUpdate', count);
	});

	retrun(
		<div>
			<p>You clicked {count} times</p>
			<button onClick={click}>Click me</button>
		</div>
	);

	function click() {
		setCount(count + 1);
	}
}
```

최초 접속 시, componentDidMount & componentDidUpdate 0 출력  
버튼 클릭 할 때마다 componentDidMount & componentDidUpdate 1 숫자 증가

React.useEffect는 componentDidMount일 때에도 실행되고 componentDidUpdate일 때에도 실행된다.

- **React.DenpendencyList 추가**

  두번째 인자는 React.DenpendencyList로, 배열이다.

  ```jsx
  React.useEffect(() => {
  	conosle.log('componentDidMount & componentDidUpdate', count);
  }, []);
  ```

  두번째 인자를 추가하면 최초 접속시 0은 잘 출력되지만, 클릭을 해도 콘솔은 출력되지 않는다.

  두번째 인자가 없을때는, 항상 render가 된 직후에는 무조건 저 화살표함수(console.log…)를 실행하라는 의미이다.

  두번째 인자로 빈 배열을 넣으면, 최초에만 실행이 된다고 선언된다.  
  배열 안의 값으로 인해 return(render) 될 때, 직후에 useEffect를 실행하라는 역할로 사용된다. 현재 배열 안에는 아무 값도 없기 때문에 어떤 것에 의해서 render가 되더라도 해당 함수는 최초 말고는 다시 실행되지 않는 것이다.

  ```jsx
  React.useEffect(() => {
  	conosle.log('componentDidMount', count);
  }, []);
  ```

  따라서, 엄밀히 말하자면 해당 코드는 componentDidMount & componentDidUpdate가 아닌 componentDidMount인 것이다.

- **useEffect 여러개 사용**

  useEffect는 여러개 사용이 가능한데, 두개 이상이 존재한다면 순차적으로 실행된다. 따라서 dependecy에 맞는 것일 때만, 안의 함수를 실행한다.

  ```jsx
  // 최초 한 번만 실행
  React.useEffect(() => {
    conosle.log('componentDidMount')
  }.[])

  // 최초 한 번 + count가 업데이트 될 때만 실행
  React.useEffect(() => {
    conosle.log('componentDidMount & componentDidUpdate', count)
  },[count])
  ```

  count에 dependency가 있다는 건 count가 변했을 때만 render가 업데이트 된 것에 의해서 useEffect가 실행된다는 것이다.

- **componentWillUnmount 역할**

  useEffect 안의 함수가 새로운 함수를 return하도록 한다. 최초 render가 된 직후, 다음 dependency에 의해서 함수가 실행되기 직전에 return을 실행하고 console이 실행된다.

  따라서 return 부분이 componentWillUnmount의 역할이다.

  ```jsx
  React.useEffect(() => {
  	conosle.log('componentDidMount', count);

  	return () => {
  		// cleanup
  		// componentWillUmount
  	};
  });
  ```

- useContext
  추후에 따로 정리하도록 한다.
