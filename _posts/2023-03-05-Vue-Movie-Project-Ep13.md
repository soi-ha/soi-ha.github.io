---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: Vuex Helper, mapState "
tags:
  - TIL
  - Vue.js
  - project
---

> [Vuex: State](https://vuex.vuejs.org/guide/state.html#the-mapstate-helper)

계산된 데이터(computed)를 이용해서 컴포넌트 내부에서 사용할 데이터들을 하나씩 만들때, 유사한 코드들을 반복적으로 작성하게 된다.

```jsx
computed: {
  image() {
    return this.$store.state.about.image
  },
  name() {
    return this.$store.state.about.name
  },
  email() {
    return this.$store.state.about.email
  },
  blog() {
    return this.$store.state.about.blog
  },
  phone() {
    return this.$store.state.about.phone
  }
},
```

이럴때, mapState를 사용하면 간략하게 코드를 작성할 수 있다.   
state를 map이라는 단어처럼 반복적으로 출력해서 computed 옵션에 등록을 시켜줄 수 있다.

**문서 예시**

```jsx
computed: mapState([
  // map this.count to store.state.count
  'count'
])
```

mapState함수를 실행해서 내부에 하나의 배열 데이터를 추가해주고, 배열 데이터안에서 내가 사용하고 싶은 스토어의 상태만 문자데이터로 명시해주면 된다.

mapState 뿐만 아니라 mapGetters, mapMutations, mapActions 등, vuex helper등이 있다.

### __mapState 적용하기__
---

```java
computed: {
  ...mapState('about', [
    'image',
    'name',
    'email',
    'blog',
    'phone'
  ])
},
```

mapState 앞에 **전개연산자 (…)을 붙여준다.** mapState 함수가 실행이 되고, 반환된 결과가 작성했던 계산된 데이터처럼 자동으로 등록될 수 있다.  
첫번째 인수로는 우리가 사용할 모듈을 작성해준다. 두번쨰 인수로, 배열안에 상태(state)를 작성해주면 된다.  
 해석하자면 about 모듈안에 있는 image 상태, name 상태를 가져와서 등록해준 것이다.

전개연산자를 붙여주는 이유는 우리가 computed 옵션에 mapState 작성하는 것이 아니기 때문이다. 우리가 만든 함수가 있을 수도 있기 때문이다. 그렇기에 mapState 함수를 문서 예시처럼 직접 할당하지 않는 것이 좋다.   
되도록이면 다른 계산된 데이터가 없더라도 앞에 전개연산자를 통해서 하나의 객체 데이터 내부에 작성하는 방식을 추천한다.

### __mapActions__
---

```jsx
created() {
  console.log(this.$route)
  this.$store.dispatch('movie/searchMovieWithId', {
    id: this.$route.params.id
  })
},
methods: {
  requestDiffSizeImage(url, size=700) {
    if (!url || url === 'N/A') {
      this.imageLoading = false
      return ''
    }
    const src = url.replace('SX300', `SX${size}`)
    this.$loadImage(src)
      .then(() => {
        this.imageLoading = false
      })
    return src
	}
}
```

mapState와 거의 동일하며 두번째 인수에는 메소드처럼 사용하고 싶은 action의 이름을 문자 아이템으로 등록하면 된다.  
그리고 기존 dispatch는 삭제하고 this에 등록한 action의 이름을 메소드로 바로 실행하면 된다.

```jsx
created() {
  console.log(this.$route)
	this.searchMovieWithId({
    id: this.$route.params.id
  })
},
methods: {
	...mapActions('movie', [
    'searchMovieWithId'
  ])
  requestDiffSizeImage(url, size=700) {
    if (!url || url === 'N/A') {
      this.imageLoading = false
      return ''
    }
    const src = url.replace('SX300', `SX${size}`)
    this.$loadImage(src)
      .then(() => {
        this.imageLoading = false
      })
    return src
	}
}
```
mapActions의 경우, store에서 코드만 봐서는 actions라는 것을 알수 없기 때문에 직접적으로 dispatch 메소드를 사용해서 해당하는 action을 사용해주는 것이 직접적이여서 추천한다.  
따라서, mapstate가 아닌 actions, mutations에서는 활용도가 높지는 않다.