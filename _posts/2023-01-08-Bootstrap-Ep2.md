---
layout: post
categories:
  - TIL
title: "BootStrap: CSS 프레임워크 Ep.2"
tags:
  - TIL
  - Bootstrap
  - CSS
  - Framework
---
## __NPM 프로젝트 생성__
---
BootStrap을 NPM 프로젝트에서 관리하기

기존에는 link 태그와 script 태그로 손쉽게 bootstrap을 가져왔다. 하지만 해당 방법은 약간의 기능 제한이 존재한다.  
외부에서 완성되어져 있는 파일만 연결하는 것이기 때문에 필요에 따라서 세부적으로 정리를 하는 것이 불가능하다.

그렇기에 우리는 NPM으로 관리를 할 것이다.

```bash
$ npm init -y
$ npm i -D parcel-bundler
```

~ **package.json 파일** ~

```json
"scripts": {
    "dev": "parcel index.html",
    "build": "parcel build index.html"
  },
```

```bash
npm install bootstrap@5.3.0-alpha1
```

해당 부트스트랩은 개발할때만 사용하는 것이 아니고 실제 브라우저에도 동작해야 하기 때문에 일반 의존성 모드로 설치해야 한다.

~ **main.js** ~

```jsx
*import* bootstrap *from* 'bootstrap/dist/js/bootstrap.bundle'
```

bootstrap의 dist 폴더에서 js 폴더 선택 후 bootstrap.bundle 파일을 선택하면 popper JS 패키지까지 합쳐서 번들로 프로젝트에 적용이 된다.

~ **main.scss** ~

```scss
*@import* "../node_modules/bootstrap/scss/bootstrap.scss";
@import "../node_modules/bootstrap/scss/bootstrap";
```

두 코드중에 하나를 선택해서 사용하면 된다. (scss 확장자 입력 유무차이 / scss는 확장자를 적지 않아도 동작한다.)

### **js파일과 scss(css)파일의 차이점**

main.js 에서는 node_modules에 설치되어져 있는 bootstrap 폴더로 바로 접근이 가능하다.  
그러나, scss(css)는 바로 node_modules 폴더로 접근이 불가능하기 때문에 상대경로로 정확하게 node_modules 먼저 찾아주고 안에 있는 bootstrap으로 접근해야 한다. 

### __NPM 관리의 장점__

NPM으로 bootstrap을 가져오는 방법이 더 복잡하고 귀찮다. 그런데도 우리가 NPM을 통해 관리해야 하는 이유는 무엇일까?

해당 방법을 통해 bootstrap을 가져오면 우리가 필요로 하는 기능만을 가져와서 사용할 수 있다.  
그리고 bootstrap에서 제공하는 기본적인 테마들을 우리가 원하는 대로 커스터마이징을 할 수 있다.

## __테마 색상 커스터마이징__
---

[Color Link](https://getbootstrap.com/docs/5.3/customize/color/)  
색상 커스터마이징을 하기 위해서 필수적으로 불러와야 하는 코드이다.  

```scss
// required
@import "../node_modules/bootstrap/scss/functions";
@import "../node_modules/bootstrap/scss/variables";
@import "../node_modules/bootstrap/scss/mixins";

$theme-colors: (
  "primary":    $primary,
  "secondary":  yellowgreen,
  "success":    $success,
  "info":       $info,
  "warning":    $warning,
  "danger":     $danger,
  "light":      $light,
  "dark":       $dark
);
```
theme-colors는 bootstrap에서 지정해 놓은 색상들이다. 해당 테마의 색상을 원하는 형태로 변경이 가능하다

## __성능 최적화(트리 쉐이킹)__
---

### __main.scss__

특정한 스타일만 가져와서 적용할 수 있는데 이게 성능적으로는 가장 최적화된 방법이긴 하지만 아직은 해당 기능이 성숙도가 떨어져 스타일 에러가 발생할 수 있다.  
그렇기 때문에 그냥 bootstrap.scss 파일을 불러오는 것이 더 안전하다.  
해당 파일은 스타일 내용만 가지고 있기 때문에 모든 스타일을 불러오는 것이 성능적으로 크게 바뀌는 것이 없기 때문이다.

```scss
@import "../node_modules/bootstrap/scss/bootstrap";
@import "../node_modules/bootstrap/scss/dropdown";
```

### __main.js__

자바스크립트에서는 개별적으로 가지고 오는 것이 큰 성능적인 차이를 가질 수 있다.  
bootstrap.bundle은 모든 자바스크립트 기능을 불러오는 파일을 사용하는 것이기 때문이다.  

기능 하나만 불러오는 것은 해당 기능만을 가져오기 때문에 bootstrap이라는 단어로 받아줄 필요가 없다. 따라서 해당 기능의 이름으로 받아준다.  
추가로, 개별적인 기능에서 가지고 오는 내용은 자바스크립트의 기본 내보내기로 나온다.

해당 기능을 불러왔으면 초기화를 해줘야 한다. 가져온 기능의 bootstrap 홈페이지로 들어가서 Via JavaScript 부분을 찾는다. 해당 부분에서 초기화되는 코드를 복사해서 붙히면 된다.

```jsx
const dropdownElementList = document.querySelectorAll('.dropdown-toggle')
const dropdownList = [...dropdownElementList].map(dropdownToggleEl => new bootstrap.Dropdown(dropdownToggleEl))
```

Via JavaScript에서 복사해온 코드는 마지막 new 키워드 뒤에 bootstrap이 존재하는데 해당 코드는 삭제한다. 개별적인 기능만 불러와서 사용하기 때문에 해당 객체 데이터는 변수로 사용이 불가능하다.

```jsx
import Dropdown from 'bootstrap/js/dist/dropdown'

const dropdownElementList = document.querySelectorAll('.dropdown-toggle')
const dropdownList = [...dropdownElementList].map(dropdownToggleEl => new Dropdown(dropdownToggleEl)
```

해당 코드만 작성해서는 동작하지 않을 것이다.  
bootstrap.bundle에 popperjs 패키지가 같이 존재했기 때문이다. 해당 기능은 popperjs가 필요하기에 따로 해당 패키지를 설치해줘야 한다.

```bash
npm i @popperjs/core
```

패키지 설치 후 package.json 파일의 dependecies에 popperjs가 잘 설치가 되었다면 다시 서버를 열어 확인해보면 잘 작동하는 것을 볼 수 있다.  

__+ Modal__  
modal의 초기화 코드의 경우 options가 붙는 것을 볼 수 있는데, options는 모달을 생성할 때 기본 옵션을 추가적으로 붙여줄 수 있다.  
예시로, backdrop의 경우 default가 ture인데 이걸 static으로 변경하면 배경을 선택해도 꺼지지 않도록 만든다.

```jsx
new Modal('#exampleModal', {
  backdrop:'static'
})
```

### __초기화 컴포넌트__

초기화를 해야하는 컴포넌트와 안해도 되는 컴포넌트를 어떻게 구분할 수 있을까?

bootstrap 홈페이지에서 해당 컴포넌트 페이지에 Via JavaScript가 존재하지 않는다면 초기화를 하지 않아도 되는 컴포넌트라고 보면 된다.   
단, method를 들어가면 초기화 코드가 보일 수 있는데 해당 초기화의 경우에는 어떤 기능을 사용하기 위해서 필요한 초기화이기 때문에 무조건 초기화를 진행하지 않아도 된다. (해당 기능을 사용하고 싶을 때만 초기화를 하면 된다.)
