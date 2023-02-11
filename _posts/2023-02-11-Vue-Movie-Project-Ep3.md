---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: v-for와 v-model 동시사용, v-if 활용"
tags:
  - TIL
  - Vue.js
  - project
---
## __v-for와 v-model 동시사용__
---

### __상황__

v-model 디렉티브를 통해서 script에서 만들어둔 데이터인 type이나 name 등을 양방향 데이터 바인딩으로 연결해줘야 하는데 filters 배열데이터를 통해서 select를 반복적으로(v-for) 출력하고 있기 때문에 script에서 만든 데이터들을 직접적으로 명시할 수 없다.

### __해결__

따라서, vue.js에서 제공하는 ‘$’로 시작하는 $data속성을 사용한다.  
여기서 $data는 script의 data()로 보면 된다.  
$data를 통해서 script의 data 안의 각 데이터들(title, type, number 등)에 직접 접근할 수 있게 된다.   
(`$data.type` 라고 하면 data의 type 데이터에 접근할 수 있게 된다.)

근데, 이 부분이 동적으로 반응을 해야 하기 때문에 $data 뒤에 대괄호를 열어서 filter의 속성(name)으로 작성하여 동적으로 사용할 수 있도록 명시한다. (`$data[filter.type]` 등)

```jsx
<select
	v-for="filter in filters"
	v-model="$data[filter.name]"
	:key="filter.name"
	class="form-select">
	<option></option>
</select>
```

## __v-if를 통해 원하는 부분에만 내용 추가__
---

### __상황__

년도가 출력되는 select 부분에 가장 첫번째 기본 옵션의 값으로 All Years를 추가하고자 한다.

데이터를 출력하는 option 상단에 다음과 같은 option을 추가한다.

```jsx
<option
	value="">
	All Years
</option>
```

해당 옵션을 추가하면 년도 출력 부분에 처음 값으로 All Years가 표시되지만 다른 type과 number 데이터 부분에도 All Years 값이 추가되게 된다. 

year 데이터를 가지는 select 부분에만 해당 값을 가지게 하려면 어떻게 할까?

### __해결__

답은 v-if를 사용하면 된다.  
v-if를 사용하여 filter의 name 값이 year인 것만 해당 option을 출력하도록 한다.

```jsx
<option
	v-if="filter.name == 'year'"
	value="">
	All Years
</option>
```

해당 코드를 통해 name 값이 year인 부분만 All Years 값을 가지는 것을 확인할 수 있다.