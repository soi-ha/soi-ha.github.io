---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 문법 Ep.7 Form 입력 바인딩 "
tags:
  - TIL
  - Vue.js
  - JS
---
## __Form 입력 바인딩__
---

>[Vue.js: Form 입력 바인딩](https://v3-docs.vuejs-korea.org/guide/essentials/forms.html)

**양방향 데이터 바인딩(연결)**  
script의 data를 template에서 사용하고 template에 입력된 데이터를 script의 method에서 실행이 되면서 해당 내용이 다시 template에 출력되는..   
즉, 데이터의 연결이 양쪽에서 모두 일어나는 것을 말한다.

추가로, **단방향 데이터 바인딩**도 있다.   
script의 data를 template에서 사용하고 template의 데이터가 script로 주지 않을 때이다.   
즉, 어디서 데이터를 주는 것인지 상관없이 어느 한쪽에서만 데이터를 줄때를 단방향 데이터 바인딩이라고 한다.

**From 입력 바인딩**은 양방향 데이터 바인딩이 발생할 때, `v-model`디렉티브를 통해 위의 내용을 단순화할 수 있도록 한다.

```jsx
<h1>{{ msg }}</h1>
<input
	type="text"
	:value="msg"
	@input="msg = $event.target.value"
/>
```

input에 입력받은 값을 msg data에 넣어서 해당 내용을 h1에 출력한다.  
from 입력 바인딩을 사용하기 전에는 v-bind와 input 이벤트를 사용하여 해당 동작을 실행시켰다.

v-model을 사용하면 값으로 원하는 데이터만 넣으면 위와 같은 동작을 간단한 코드로 동작할 수 있게 만든다.

```jsx
<input
	type="text"
	:v-model="msg"
/>
```

**즉, v-model은 간단하게 데이터를 연결해주는 디렉티브이다.**

**v-model 사용시 주의할 점**은 한글을 값으로 가지는 데이터를 연결할 경우 출력되는 데이터가 한 박자 늦게 반응한다.   
한글의 경우에는 자음과 모음을 통해서 하나의 글자가 만들어져야만 결과가 반영되는 구조를 가지기 때문이다.  
따라서, **한글 데이터를 사용할 경우** v-model이 아닌 **단순화하기 전의 코드(수동)로 사용하는 것이 좋다.**

## __Form 입력 바인딩: v-model 수식어__
---

- `.lazy`
  
  ```jsx
  <h1>{{ msg }}</h1>
  <input
    type="text"
    :value="msg"
    @change="msg = $event.target.value"
  />
  ```
  
  input 이벤트와 달리 change 이벤트는 input 창에 입력되는 데이터를 즉시 msg에 출력하는 것이 아니라, 입력이 멈춰야(커서를 없앤) 데이터를 출력한다.  
  말 그대로 데이터를 change(변경)한다고 보면 된다.
  
  **lazy 수식어**는 **change 이벤트를 v-model로 단순화**시킨 것이다. 
  
  ```jsx
  <h1>{{ msg }}</h1>
  <input
    type="text"
    v-model.lazy="msg"
  />
  ```
    
- `.number`
  
  input 요소에 입력되는 데이터가 숫자 데이터로 존재해야 할 때 사용한다.
  
  해당 수식어를 사용하지 않고 숫자를 입력하면 type은 number가 아닌 string이 된다. number 수식어를 사용하면 type이 number가 된다.
  
  ```jsx
  <h1>{{ msg }}</h1>
  <input
    type="text"
    v-model.number="msg"
  />
  ```
    
- `.trim`
  
  input 요소에 입력되는 데이터의 앞, 뒤 공백을 없애준다.
  
  해당 수식어를 사용하고 앞에 공백을 만들어도 값이 변경되지 않으며, 커서를 없애게 되면 앞의 공백이 사라지게 된다.
  
  ```jsx
  <h1>{{ msg }}</h1>
  <input
    type="text"
    v-model.trim="msg"
  />
  ```