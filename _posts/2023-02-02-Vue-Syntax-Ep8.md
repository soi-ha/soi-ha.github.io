---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 문법 Ep.8 컴포넌트1 "
tags:
  - TIL
  - Vue.js
  - JS
---

## __컴포넌트: 기초__
---

>[Vue.js: 컴포넌트 기초](https://v3-docs.vuejs-korea.org/guide/essentials/component-basics.html)

- 컴포넌트 정의하기
  
  빌드 방식을 사용할 때 일반적으로 싱글파일컴포넌트(SFC)라고 하는 `.vue` 확장자를 사용하는 전용 파일에 각 Vue 컴포넌트를 정의한다.
  빌드 방식을 사용하지 않을 때, Vue 컴포넌트는 Vue 관련 옵션을 포함하는 일반 자바스크립트 객체로 정의할 수 있다.
    
- 컴포넌트 사용하기
  
  ```jsx
  <template>
    <MyBtn />
    <MyBtn />
    <MyBtn />
    <MyBtn />
  </template>
  
  <script>
  import MyBtn from '~/components/MyBtn'
  
  export default {
    components: {
      MyBtn
    }
  }
  </script>
  ```
  
  가져온 컴포넌트를 템플릿에 노출하려면 `components` 옵션을 사용하여 등록해야 한다. 그러면 컴포넌트는 등록된 키를 사용하여 태그로 사용할 수 있다.
  
  컴포넌트를 전역으로 등록하면, import 없이 지정된 앱의 모든 곳에서 컴포넌트를 사용할 수 있다.
  
  컴포넌트는 원하는 만큼 재사용할 수 있다.
  
  SFC에서는 HTML 요소와 구별하기 위해 자식 컴포넌트에 PascalCase 태그 이름을 사용하는 것이 좋다. 또한 `/>`를 사용하여 태그를 닫을 수 있다.
  
- Props 전달하기
  
  props은 컴포넌트에 등록할 수 있는 사용자 정의 속성이다.
  
  props 속성에 값이 전달되면, 해당 컴포넌트 인스턴스의 속성이 된다.  
  컴포넌트는 원하는 만큼 props를 가질 수 있으며, 기본적으로 모든 값을 모두 props에 전달할 수 있다.
  
  `props`가 등록되면, 다음과 같이 데이터를 사용자 정의 속성으로 전달할 수 있다.
  
  ```jsx
  <template>
    <MyBtn />
    <MyBtn color="royalblue" />
    <MyBtn />
    <MyBtn />
  </template>
  
  <script>
  import MyBtn from '~/components/MyBtn'
  
  export default {
    components: {
      MyBtn
    }
  }
  </script>
  ```
  
  ```jsx
  <template>
    <div 
      :style="{ backgroundColor: color }"
      class="btn">
      Apple
    </div>
  </template>
  
  <script>
  export default {
    props: {
      color: {
        type: String,
        default: 'gray'
      }
    }
  }
  </script>
  
  <style scoped>
    .btn {
      display: inline-block;
      margin: 4px;
      padding: 6px 12px;
      border-radius: 4px;
      background-color: gray;
      color: white;
      cursor: pointer;
    }
  </style>
  ```
  
  props로 color를 만들어준다.  
  class btn에 데이터 바인딩으로 style 속성을 출력한다. 내부 객체 데이터에 backgroundColor는 카멜케이스로 작성하여 props에 해당하는 color 데이터를 연결해준다.
  
  **props**는 부모 컴포넌트에서 자식 컴포넌트로 특정한 데이터를 전달할때 사용하는 용도라고 해서, 부모 컴포넌트와 자식 컴포넌트간의 **데이터 통신 방법**이라고도 한다.  
  부모-자식간의 데이터 통신을 props 기능으로 만들어 낸 것이다.
  
- slot 태그
  
  App.vue 파일
  
  ```jsx
  <template>
    <MyBtn>Banana</MyBtn>
    <MyBtn text="Banana" />
  </template>
  ```
  
  MyBtn.vue 파일
  
  ```jsx
  <template>
    <div 
      :class="{ large }"
      :style="{ backgroundColor: color }"
      class="btn">
      <slot></slot>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      text: {
        type: String,
        default: ''
      }
    }
  }
  </script>
  ```
  
  컴포넌트도 일반적인 요소처럼 열리고 닫히는 태그의 사이에 내용을 적어줄 수 있다.   
  적힌 내용을 어디에 출력할 것인지 `slot`태그를 통해 지정해줄 수 있다.  
  slot 태그를 사용하게 되면 text라는 props를 만들어 줄 필요가 사라지게 된다.
  
  ```jsx
  <template>
    <MyBtn>Banana</MyBtn>
    <MyBtn>
      <span style="color: red;">Orange</span>
    </MyBtn>
  </template>
  ```
  
  slot 태그는 단순하게 텍스트만 받아서 태그의 위치에 출력만 해주는 것이 아니다. MyBtn 이라는 컴포넌트가 실행되는 부분에서 열리고 닫히는 태그 사이에 있는 **모든 내용을 slot 태그 부분에 대체**해서 넣어준다.

## __컴포넌트: 속성 상속__
---

```jsx
<template>
  <MyBtn class="soha">
    Banana
  </MyBtn>
</template>
```

```jsx
<template>
  <div class="btn">
    <slot></slot>
  </div>
</template>
```

개발자도구를 통해서 MyBtn의 class를 확인하면 btn과 soha가 들어가있는 것을 확인할 수 있다.

```jsx
<template>
  <div class="btn">
    <slot></slot>
  </div>
	<div></div>
</template>
```

MyBtn 파일에 div 박스를 추가하면 MyBtn의 클래스에 soha가 사라지고 btn만 남아있게 된다. 왜 이렇게 되는 걸까?

하나의 컴포넌트의 경우에는 template 태그 사이에 기본적인 html 구조를 작성하게 된다. 이때, template 태그의 바로 아래 자식요소를 해당하는 컴포넌트의 **최상위요소(루트요소)**라고 부른다.  
Mybtn 컴포넌트의 경우 최상위요소가 두개 (`<div class=”btn” …>`, `<div></div>`)존재하게 된다.

이렇게 하나의 컴포넌트에 최상위요소가 두개인 경우에는 컴포넌트가 사용되는 곳에적용이 된 여러 속성들의 내용이 첫번째 최상위요소에 들어갈 것인지, 두번째 최상위요소에 들어갈 것인지 정확하게 지정해줄 수 없다.   그렇기에 어떠한 최상위요소에도 들어가지 않게 된다. (어디에도 들어가지 않게 된, class soha)

우리가 최상위 요소를 단 하나만 남겨두게 된다면 해당 내용이 하나만 존재하는 곳에 들어가게 되는 것이다.

그래서, 컴포넌트가 사용되는 곳의 여러 속성들이 컴포넌트의 하나의 요소에 연결되는 것을 **속성의 상속**이라고 부른다.

- 속성의 상속 지정 옵션
  
  ```jsx
  <script>
  export default {
    inheritAttrs: false
  }
  </script>
  ```
  
  옵션으로 **inheritAttrs**를 추가하고 값을 false로 하면, 최상위요소가 하나만 존재해도 상속이 일어나지 않는다.
  
  해당 옵션은 상속의 지정을 설정하는 옵션이다. true면 상속이 이뤄지고 false면 어떤 경우에도 상속이 이뤄지지 않는다.
    
- 들어오는 옵션을 원하는 곳에 배치하기
  
  created()의 경우 렌더링된 직후의 데이터들을 보여준다.   
  콘솔창에서 데이터를 확인하면 proxy의 target에 class는 soha, style은 red가 들어온 것을 볼 수 있다.
  
  ```jsx
  <template>
    <div class="btn">
      <slot></slot>
    </div>
    <div
      :class="$attrs.class"
      :style="$attrs.style"></div>
  </template>
  
  <script>
  export default {
    inheritAttrs: false,
    created() {
      console.log(this.$attrs)
    }
  }
  </script>
  ```
  
  해당 데이터의 값이 $attrs에 들어가 있으므로, 해당 데이터를 넣어주고 싶은 곳에 위와 같이 데이터를 연결해주면 된다.
  
  두번째 최상위요소의 클래스와 스타일을 확인하면 soha와 color:red가 들어가있는 것을 확인할 수 있다.
    
- 하나의 요소에 내용 모두 적용
  
  ```jsx
  <template>
    <div class="btn">
      <slot></slot>
    </div>
    <div
      v-bind="$attrs"></div>
  </template>
  ```
  
  v-bind를 통해서 해당 값으로 $attrs를 넣어주면 해당하는 내용들이 모두 들어가게 된다.
  
  해당하는 컴포넌트(App.vue)부분에 여러가지 속성들을 추가할 때마다, MyBtn 컴포넌트의 div 부분에 따로 명시해줄 필요가 사라진다.

## __컴포넌트: Emit__
---

속성 상속에서 $attrs 라는 객체를 특정한 요소에 직접적으로 연결을 해서 사용한 것처럼, 이벤트(@click…)들도 직접적으로 연결을 해줄 수 있다.

```jsx
<template>
  <MyBtn @click="log">
    Banana
  </MyBtn>
</template>
```

```jsx
<template>
  <div class="btn">
    <slot></slot>
  </div>
  <h1 @click="$emit('click')">
    abc
  </h1>
</template>

<script>
export default {
  emits: [
    'click'
  ]
}
</script>
```

MyBtn 컴포넌트의 스크립트에 emits 옵션을 추가해서 배열 데이터로 ‘click’ 이벤트를 추가해준다. 컴포넌트 내부에서 사용할 것이라는 걸 정의해준 것이다.

click 이벤트를 연결할 부분에 click 이벤트를 추가하고 해당 태그(h1)를 클릭하게 되면 어떤 내용을 실행시켜줄 것인지를 $emit() 메소드를 통해 지정해준다. 

emits 옵션을 통해서 click 이벤트를 부모요소(App.vue)에서 적용된 내용을 가지고와서 명시를 했다. 명시된 내용을 $emit 부분에 작성해주면 된다.   emit 메소드의 괄호에 ‘click’을 적어주면 된다.

abc를 클릭하게 되면 콘솔창에 Click!이 출력되는 것을 확인할 수 있다.

- App.vue 이벤트 이름 변경하기
  
  App.vue 파일에서 해당 컴포넌트(MyBtn)를 클릭(@click)을 했을 때 log 메소드가 실행되는 것일까?
  
  정확하게 따지면 그렇지 않다. App파일에서 말하는 click이라는 이벤트는 MyBtn 컴포넌트의 emits 옵션으로 넘어가 내부에서 사용되기 때문에 굳이 click이라는 이름으로 이벤트를 실행하지 않아도 된다.
  
  ```jsx
  <template>
    <MyBtn @soha="log">
      Banana
    </MyBtn>
  </template>
  ```
  
  ```jsx
  <template>
    <div class="btn">
      <slot></slot>
    </div>
    <h1 @click="$emit('soha')">
      abc
    </h1>
  </template>
  
  <script>
  export default {
    emits: [
      'soha'
    ]
  }
  </script>
  ```
  
  위와 같이 click 이벤트의 이름을 soha로 변경해도 h1 태그를 클릭했을 때, 콘솔창에 Click!이 출력되는 것을 볼 수 있다.
  
  결국 컴포넌트에 연결하는 이벤트는 우리가 실제로 쓸 수 있는 이벤트의 이름이 아니어도 상관이 없게된다.
  
  단, 컴포넌트(MyBtn)의 이벤트는 실제 동작할 수 있는 이벤트의 이름으로 해야한다. 
  
- 이벤트가 동작했을 때, 객체 활용하기
    
  $emit의 첫번째 인수로는 이벤트의 이름을 문자데이터로 작성하고 두번째 인수로는 해당 이벤트가 실행될 때 같이 넘겨줄 데이터를 작성할 수 있다.
  
  ```jsx
  <template>
    <MyBtn @soha="log">
      Banana
    </MyBtn>
  </template>
  
  <script>
  import MyBtn from '~/components/MyBtn'
  
  export default {
    components: {
      MyBtn
    },
    methods: {
      log(event) {
        console.log('Click!!')
        console.log(event)
      }
    }
  }
  </script>
  ```
  
  log 메소드에 event를 추가해서 콘솔창에 event를 출력하게 해준다.   
  MyBtn 컴포넌트에서 $emit에 두번째 인수를 추가하지 않으면 event 값은 undefined로 출력된다.
  
  ```jsx
  <template>
    <div class="btn">
      <slot></slot>
    </div>
    <h1 @click="$emit('soha',123)">
      abc
    </h1>
  </template>
  ```
  
  $emit의 두번째 인수로 123 숫자 데이터를 넣어주게 되어 click을 했을 때 event의 값으로 123이 콘솔창에 출력된다.
  
- 양방향 데이터 바인딩으로 활용하기
  
  msg 데이터를 실시간으로 반영하는 양방향 데이터 바인딩을 만든다.  
  msg 데이터가 변경될 때마다, App.vue에서 확인할 수 있게 만든다.
  
  ```jsx
  <template>
    <MyBtn
      @soha="log"
      @change-msg="logMsg">
      Banana
    </MyBtn>
  </template>
  
  <script>
  import MyBtn from '~/components/MyBtn'
  
  export default {
    components: {
      MyBtn
    },
    methods: {
      log(event) {
        console.log('Click!!')
        console.log(event)
      },
      logMsg(msg) {
        console.log(msg)
      }
    }
  }
  </script>
  ```
  
  화면에서 input요소에 데이터를 입력할 때마다, watch 옵션을 통해서 msg 메소드가 실행되도록 한다. 이때 this.$emit을 통해서 changeMsg 이벤트를 실행해준다.
  
  주의! html의 속성을 작성할때는 카멜케이스를 사용할 수 없으므로 대쉬케이스로 수정해준다.  
  changeMsg ⇒ change-msg
  
  ```jsx
  <div class="btn">
      <slot></slot>
    </div>
    <h1 @click="$emit('soha',123)">
      abc
    </h1>
    <input
      type="text"
      v-model="msg" />
  </template>
  
  <script>
  export default {
    emits: [
      'soha',
      'changeMsg'
    ],
    data() {
      return {
        msg: ''
      }
    },
    watch: {
      msg() {
        this.$emit('changeMsg', this.msg)
      }
    }
  }
  </script>
  ```
  
  MyBtn 컴포넌트의 input 요소에 데이터를 입력할 때마다, msg 데이터가 양방향 데이터 바인딩으로 갱신이 되는 구조이다.  
  watch 옵션을 통해서 갱신되는 msg 데이터를 감시하여 데이터가 변경될 때마다 해당 내용(`$emit('changeMsg’…,)`)을 실행하게 된다.
  
  실행되는 내용은 $emit 메소드를 실행하여 내부의 changeMsg라는 이름의 이벤트를 발생시킨다. 해당 이벤트가 발생되면 changeMsg가 연결되어져 있는 App.vue의 change-msg가 실행되면서 연결된 logMsg 메소드가 동작하게 된다.
  
  logMsg 메소드가 동작할 때, 매개변수는 어떤 내용을 받아서 콘솔창에 출력한다. 이때 받는 내용(매개변수 msg)은 MyBtn의 watch의 $emit 메소드에 작성되어있는 두번째 인수 this.msg 즉, 실제 입력한 데이터이다.
  
  결과적으로 콘솔창을 확인하면 input 요소에 123을 입력하면 다음과 같이 출력된다.
  
  ```
  1
  -----
  12
  -----
  123
  ```