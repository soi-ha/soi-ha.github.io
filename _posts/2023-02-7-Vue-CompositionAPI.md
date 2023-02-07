---
layout: post
categories:
  - TIL
title: "Vue.js: 컴포지션 API "
tags:
  - TIL
  - Vue.js
  - JS
---
## __개요__
---

>[Vue.js: 컴포지션 API FAQ](https://v3-docs.vuejs-korea.org/guide/extras/composition-api-faq.html)  
>[Vue.js: 컴포지션 API: setup()](https://v3-docs.vuejs-korea.org/api/composition-api-setup.html)

### __컴포지션 API__

컴포지션(Composition) API는 옵션을 선언하는 대신 import한 함수를 사용하여 Vue 컴포넌트를 작성할 수 있는 API 세트이다. 

### __컴포지션 API 사용이유__

- 더 나은 로직 재사용성
  
  컴포지션 API의 가장 큰 장점은 컴포저블 함수의 형태로 깔끔하고 효율적인 로직 재사용이 가능하다는 것이다. 옵션 API의 기본 로직 재사용 메커니즘인 mixins의 모든 단점을 해결한다.
    
- **보다 유연한 코드 구성**
  
  동일한 논리적 문제를 처리하는 코드가 파일의 여러 부분에 분할되어있다.   
  추후 해당 로직을 이해하는데 위아래 스크롤을 통해 확인을 해야 한다. 이러한 점들은 코드의 길이가 수백 줄이 넘어가면 어려움이 발생하게 된다.   
  또한 논리적 문제를 재사용 가능한 유틸리티로 추출하려는 경우, 파일의 다른 부분에서 올바른 코드 조각을 찾아 추출하는 것이 어려워진다.
  
  ![Composition API](https://v3-docs.vuejs-korea.org/assets/composition-api-after.e3f2c350.png)
  
  하지만, 컴포지션 API를 사용하게 되면 이런 어려웠던 점들이 훨씬 간편해진다.  
  더 이상 다른 옵션 블록 사이를 이동할 필요가 없어지며, 추출을 위해 코드를 섞을 필요가 없기 때문에 보다 간편하게 코드 그룹을 외부 파일로 이동시킬 수 있다.  
  따라서, 컴포지션 API를 사용하는 것 만으로도 리팩토링을 위한 소모 시간이 감소하게 되어 대규모 코드베이스에서 장기적인 유지 관리에 큰 도움을 준다.
    
- 더 나은 타입 추론
  
  컴포지션 API는 기본적으로 유형 친화적인 일반 변수와 함수를 주로 사용한다. 컴포지션 API로 작성된 코드는 수동 유형 힌트가 거의 필요 없어 전체 유형 추론에 큰 도움을 준다.
    
- 더 작은 프로덕션 번들 및 더 적은 오버헤드
  
  컴포지션 API와 script setup으로 작성된 코드는 옵션 API에 비해 더 효율적이고 축소하기 쉽다. 모든 변수 이름을 안전하게 단축할 수 있기 때문에 더 간편하게 코드 작성이 가능하다.

## __반응형 데이터(반응성)__
---

>[Vue.js: 반응형 API: 핵심](https://v3-docs.vuejs-korea.org/api/reactivity-core.html)

`ref()`및 `reactive()`를 사용하여 반응형 상태, 계산된 상태, 감시자를 생성한다. 해당 함수를 사용해야지만 컴포지션 API로 정리한 후, 반응성을 띈다.

- 컴포지션 API 사용 X
  
  ```jsx
  <template>
    <div @click="increase">
      {{ count }}
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        count: 0
      }
    },
    methods: {
      increase() {
        this.count += 1
      }
    }
  }
  </script>
  ```
    
- 컴포지션 API 사용 O
  
  ```jsx
  <template>
    <div @click="increase">
      {{ count }}
    </div>
  </template>
  
  <script>
  export default {
    setup() {
      let count = 0
      function increase() {
        count += 1
      }
  
      return {
        count,
        increase
      }
    }
  }
  </script>
  ```
  
  반응성을 가지지 않음. count 값이 갱신되지 않는다.
  
- 컴포지션 API 사용 및 반응성
  
  ref 기능을 사용하여 반응성을 가지도록 만든다. 
  
  ```jsx
  <template>
    <div @click="increase">
      {{ count }}
    </div>
  </template>
  
  <script>
  import { ref } from 'vue'
  export default {
    setup() {
      let count = ref(0)
      function increase() {
        count.value += 1
      }
  
      return {
        count,
        increase
      }
    }
  }
  </script>
  ```
  
  count에 바로 숫자를 할당하는 것이 아니라 ref 함수를 호출하여 할당할 값인 0을 적어준다. ref가 실행이 되면서 반응성을 가지는 객체 데이터가 반환이 된다. 따라서 count는 반응성을 가질 수 있게 된다.  
  하지만, 하나의 ‘객체’ 데이터가 반환이 되는 것이다 보니, count를 데이터로 직접 사용할 수 없다. 데이터로 활용하기 위해서 count의 객체 데이터의 value 속성을 통해 우리가 원하는 데이터에 접근 할 수 있다.

## __기본 옵션과 라이프사이클__
---

>[Vue.js: 컴포지션 API: 수명주기 훅](https://v3-docs.vuejs-korea.org/api/composition-api-lifecycle.html)

라이프사이클 훅에 접두사 ‘on’을 추가함으로 컴포넌트의 라이프사이클 훅에 접근할 수 있다.

- computed
  
  **컴포지션 API 미사용**
  
  ```jsx
  computed: {
    doubleCount() {
      return this.count * 2
    }
  }
  ```
  
  **컴포지션 API 사용**
  
  ```jsx
  import { computed } from 'vue'
  ...
  setup() {
    const doubleCount = computed(() => {
      return count.value * 2
    })
  }
  ```
    
- watch
    
  **컴포지션 API 미사용**
  
  ```jsx
  watch: {
    message(newvalue) {
        console.log(newValue)
    }
  }
  ```
  
  **컴포지션 API 사용**
  
  ```jsx
  import { watch } from 'vue'
  ...
  setup() {
    watch(message, newValue => {
      console.log(newValue)
    })
  }
  ```
    
- created
    
  created는 컴포넌트가 생성된 직후에서 실행이 되는 라이프사이클이다.
  
  **컴포지션 API 미사용**
  
  ```jsx
  created() {
    console.log(this.message)
  }
  ```
  
  **컴포지션 API 사용**
  
  ```jsx
  setup() {
    console.log(message.value)
  }
  ```
  
  setup이라는 메소드는 컴포넌트가 생성된 직후에 실행이 되기 때문에 setup 메소드 내부에서 실행되는 기본적인 로직은 created에 작성하는 것과 동일한 효과를 갖는다.  
  따라서, created는 따로 import를 해야 할 것도, 메소드는 사용할 것도 없이 그냥 setup 내부에 작성하면 된다. (setup 내부 어디에서든지 사용해도 된다.)
  
- mounted (onMounted)
  
  mounted는 html과 컴포넌트가 연결이 된 직후에 실행이 되는 라이프사이클이다.
  
  **컴포지션 API 미사용**
  
  ```jsx
  mounted() {
    console.log(this.count)
  }
  ```
  
  **컴포지션 API 사용**
  
  ```jsx
  import { onMounted } from 'vue'
  ...
  setup() {
    onMounted(() => {
      console.log(count.value)
    })
  }
  ```
  
  mounted를 컴포지션 API에서 사용하려면 onMounted로 작성해야 한다.

## __props, context__
---

기존에는 this를 통해서 color (props 옵션에 정의된)에 바로 접근을 할 수 있었다.  
this를 통해 $attrs(상속받은 모든 속성 개체)를 가져올 수 있었다.

컴포지션API에서는 this를 사용할 수 없기 때문에 `this.color`를 `props.color`로, `this.$attrs`를 `context.attrs`로 바꿔서 사용할 수 있다.

**컴포지션 API 미사용**

```jsx
props: {
	color: {
		type: String,
		default: 'gray'
	}
},
emits: ['hello'],
mounted() {
	console.log(this.color)
	console.log(this.$attrs)
},
methods: {
	hello() {
		this.$emit('hello')
	}
}
```

**컴포지션 API 사용**

```jsx
import { onMounted } from 'vue'
...
props: {
	color: {
		type: String,
		default: 'gray'
	}
},
emits: ['hello'],
setup(props, context) {
	function hello() {
		context.emit('hello')
	}
	onMounted(() => {
		console.log(props.color)
		console.log(context.attrs)
	})

	return {
		hello
	}
}
```

- props
  
  setup함수의 첫 번째 인자는 props이다.   
  setup함수 내부의 props는 반응형이며, 새 props가 전달되면 업데이트된다.  
  props 객체를 분해할 경우, 분해 된 변수는 반응성을 잃게 되기에 항상 props.xxx 형태처럼 접근해야 한다.
  
- context
    
  setup함수에 전달되는 두 번째 인자는 context객체이다.   
  context 객체는 setup 내부에서 유용할 수 있는 다른 값을 노출한다.   context 객체는 반응형이 아니기에 안전하게 분해 할당될 수 있다.