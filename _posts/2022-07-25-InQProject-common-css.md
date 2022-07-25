---
layout: post
categories:
  - Project
  - TIL
title: "InQ Project Manager: 공통 CSS 파일 만들기"
tags:
  - InQ
  - project
  - InQ Project Manager
---

## __공통 CSS 파일 필요성__
---

내가 이번에 진행하는 프로젝트는 하나의 페이지만 존재하는 것이 아닌, 페이지 수가 최소 3장 이상인 프로젝트이다.  
이 프로젝트 페이지를 하나하나 만들때마다 공통으로 사용되는 것들이 존재한다. 버튼이라던가, 카드, header, footer, 글자 사이즈 및 색상 등...  
이 스타일들을 매번 각 페이지에 작성하기에는 비효율적이고 관리하는데도 복잡한 점이 많다. 색상이나 글자 사이즈를 하나 변경시에 관련된 다른 페이지의 모든 것들을 하나하나 수정해야하기 때문이다.  
그래서 모든 페이지에서 공통적으로 사용할 스타일들을 **common.css**파일에 작성해두었다. 그리고 모든 html파일에 해당 파일을 연결시켜줬다. 이로인해 유지보수를 하는데 효율성이 매우 상승하였다!  

<br>

## __common.css__
---
내가 작성했던 스타일 코드들을 기능별로 나누어 설명하고자 한다.  
나는 해당 파일에 내가 설정한 css 속성, 기본 설정, button, card, header, footer, 그 외의 내용을 작성했다.  
common.css에 작성했던 것 중에 header와 footer의 스타일은 [저번 글](./2022-06-16-InQProject-header-footer.md)에 작성하였으므로 제외하고 작성하겠다.  

<br>

### __사용자 지정 css 속성__
  사용자 지정 속성(css 변수, 종속 변수)은 내가 직접 정의한 개체를 의미한다. 현재 진행중인 프로젝트에서 전반적으로 재사용할 값들을 담을 때 주로 사용한다. 표기법과 사용법은 다음과 같다. 
```css
:root {
  --inq-blue: #0088BD;
  --inq-yellow: #FFD303;
  --inq-white: #f4fbff;
  --font-black: #1f1e1e;
  --recruit-color: #FC8B79;
  --execution-color:#FFD303;
  --complete-color: #A1ED85;
  --title-font: 80px;
  --sub-title-font: 50px;
  --large-font: 40px;
  --mideum-font: 32px;
  --small-font: 20px;
  --btn-font: 30px
}
```
  - 표기법 `:root`
    ```css
    /* 표기법 */
    :root {
      --main-color: #FC8B79;
      --sub-color: #0088BD;
      --main-font: 60px;
    }
    ```
    `:root{--변수명: 값;}` 형태로 작성한다. 이렇게 작성하면 나중에 색상이나 전반적인 글자 크기를 수정할 때 해당 파일에서 값 하나만 바꿔주면 모든 파일에서 해당 값들이 변경된다. 이로인해 수정할 때 오류없이 간단하게 바꿀 수 있다.  
    그리고 이렇게 속성을 지정하면 해당 값들이 의미하는 것이 무엇인지 직관적으로 알 수 있어서 이해하기 좋다. 그저 색상을 #0088BD로 적는 것 보다 --sub-color로 작성했을 때가 해당 속성이 의미하는 것이 무엇인지 한번에 이해가 가능하다는 장점이 있다.

  - 사용법 `var()`
    ```css
    /* 사용법 */
    p {
      color: var(--main-color);
      font-size: var(--main-font);
    }
    ```
    지정한 속성을 사용할때는 꼭 `var()`안에 넣어 사용해야 한다.  
    **var()** 함수안에 넣어 사용해야 사용자가 지정한 속성이라는 것을 인식해서 제대로 코드가 작동할 수 있게 된다. 

<br>

### __기본 설정__
공통적으로 자주 사용하는 태그들의 기본 속성들을 내가 원하는 스타일로 설정했다. 해당 태그를 사용할때마다 속성을 변경하기에는 효율성이 떨어지니 common 스타일 파일에 작성하였다. 덕분에 매번 속성을 변경해야하는 번거로움을 줄일 수 있었다.
```css
/* COMMON */
body {
  color: var(--font-black);
  font-size: var(--mideum-font);
  font-weight: 400;
  line-height: 1.4; /*글자의 행간을 줌 (줄높이)*/
  font-family: 'Noto Sans KR', sans-serif;
}
img {
  display: block;
  /* 이제 이미지는 더이상 인라인 요소가 아닌 블럭요소가 된다 */
}
a {
  /* 모든 a태그의 밑줄을 없애기 */
  text-decoration: none;
  color: var(--font-black);
}
a:hover {
  text-decoration: none;
  color: var(--font-black);
}
input,
select {
  font-size: var(--mideum-font);
}
.inner {
  width: 720px;
  margin: 0 auto;
  position: relative; /* 자식요소 때문에 position값을 넣는 것이기에 문제가 되지 않기 위해 relative로 함 */
}
```
- __body__  
  따로 속성을 부여하지 않은 것에는 해당 속성들이 적용된다. 
- __img__  
  img태그는 인라인 요소이다. 나는 img 태그를 사용할때 블럭처럼 사용하고 싶었기 때문에  `display:block;`을 사용하여 블럭요소로 변경하였다.
- __a__  
  a태그 사용시 파란 색상과 밑줄이 발생한다. 해당 요소를 없애고 싶어 위와같이 색상을 검정색으로 변경했다.  
  밑줄을 없애기 위해서 `text-decoration: none;`을 사용하였다. 밑줄을 없앤다(none)는 의미이다.
- __a:hover__  
  a태그에 hover를 하고 나면 색상과 밑줄이 발생하였기 때문에   
  ~~마우스를 올린 것만이 아닌 a태그를 클릭하고 나면 방문했다는 것을 나타내기 위해 색상이 변경되고 밑줄이 발생하는 것~~  
  a태그와 동일하게 속성을 작성했다.
- __input,select__  
input과 select요소를 사용할때마다 해당 파일에 글자 크기를 변경하는 번거로움을 줄이기 위해서 common 스타일 파일에 글자 크기를 정의하였다.
- __.inner__  
inner 클래스는 내가 만들 클래스이다.  
페이지를 만들 때 내용들을 꽉차게 넣는 것이 아닌 양쪽에 수직으로 여백을 넣어주는 것을 흔히 볼 수 있다. 그렇게 하기 위해 내용을 넣을 부분의 너비 사이즈를 정해준 것이다. `width: 720px;`  
그리고 가운데 정렬을 위해 `margin: 0 auto;`를 작성하였다.  
`position: relative;`는 다른 속성들이 부모요소를 기준으로 위치를 조정할 때가 빈번히 발생하게 되는데 그때 대부분 부모요소가 해당 inner이 된다. 그대마다 매번 작성하기 번거로우니 common 파일에 정의해두었다. 