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

<br>

### __버튼 css__
모든 페이지에서 버튼은 굉장히 많이 사용한다. 버튼의 스타일이 대부분 동일하고 약간의 색상 변화를 주는 경우가 대다수였기에 common 파일에 버튼 스타일을 정의해두어 각 페이지에서 사용할때마다 해당 클래스명만을 입력하여 버튼 스타일을 사용하였다.
```css
/* Button */
.bton {
  width: 250px;
  padding: 10px;
  border-radius: 4px;
  color: var(--font-black);
  background-color: transparent;
  font-size: var(--btn-font);
  font-weight: 500;
  text-align: center; /*박스 안에서 글자 가운데 정렬*/
  cursor: pointer;
  /* box-sizing: border-box; 패딩, 보더가 들어간 만큼 요소가 커지지 않도록 */
  display: block; /* a나 span태그에 버튼 클래스를 부여했을 때도 버튼이 정상적으로 나오도록 */
  transition: .4s; /* hover시 자연스럽게 색상이 바뀌도록 */
}
.bton:hover {
  background-color: var(--inq-yellow);
}
.bton.btn--reverse {
  background-color: var(--inq-yellow);
  border: 0;
  color: white;
}
.bton.btn--reverse:hover {
  background-color: #ffa703;
}
.bton.btn--blue {
  background-color: transparent;
  border: 2px solid var(--inq-blue);
  border-radius: 4px;
  color: var(--inq-blue);
}
.bton.btn--blue:hover {
  background-color: var(--inq-blue);
  color: var(--inq-white);
}
.bton.btn--blue-reverse {
  background-color: var(--inq-blue);
  border: 2px solid var(--inq-blue);
  border-radius: 4px;
  color: var(--inq-white);
}
.bton.btn--blue-reverse:hover {
  background-color: transparent;
  color: var(--inq-blue);
}
```
- __bton__  
가장 기본적인 버튼 스타일 속성이다. 해당 클래스명을 작성하면 내가 작성한 기본적인 버튼 속성이 적용된다. 
- __CSS선택자: 일치 선택자__  
선택자 ABC와 XYZ를 동시에 만족하는 요소를 선택할 때 사용한다.  
  ```html
  <div class="abc xyz">hello</div>
  <span class="ght">world</span>
  ```
  ```css
  .abc.xyz {
    color: red;
  }
  span.ght {
    color: blue;
  }
  ```  
  위의 경우에는 hello는 빨간색, world는 파란색으로 결과가 나온다.  
  나는 일치 선택자를 기본적으로 적용한 버튼 스타일 속성에서 추가적으로 변경하고 싶은 속성이 있을 때 사용하였다.  
  예를 들어,.bton에서 글자 색상만을 바꾸고 싶을 때, 배경 색상을 변경하고 싶을 때 등등 
- __background: transparent__  
`background: transparend`를 사용하여 배경색상을 투명하게 만든다.
- __transition: 속성명 지속시간 타이밍함수 대기시간;__  
`transition: .6s`  
요소의 전환(시작과 끝)효과를 지정하는 단축 속성이다. 이때, 지속시간은 필수적으로 작성해야 하는 속성이다.  
지속시간을 입력하면 전환 효과의 시간을 지정한다. 기본은 **0s**로 전환 효과가 없다. 지속시간을 작성할때는 원하는 시간 뒤에 **S**를 붙혀 작성한다. 이때 **S**는 second를 의미한다.  
만약 0.5초 지속시간을 주고 싶다면 앞의 0은 생략이 가능하다. (0.5s -> .5s)  

<br>

### __카드 Card__  

<img width="1494" alt="홈2" src="https://user-images.githubusercontent.com/77609591/182856650-25758d2f-57ab-4b57-9e62-31196ad1cf1b.png">

홈 화면과, 프로젝트 홈 화면에서 사용하는 카드에 대한 스타일을 common 파일에 작성하였다. 아직은 두 페이지에서만 사용하지만 차후 업데이트를 하게 된다면 다른 곳에서 사용가능성이 높기 때문이다. 또, 두 페이지에서 사용하는 카드의 모양이 완전히 동일하기 때문에 각 페이지의 css파일에 작성하는 것 보다는 common 파일에 작성하는 것이 더 수정에 편리할 것이라 생각이 들어 common 파일에 작성하였다. 
```html
<a href="#" onclick="location.href='./project/project_info.html'" class="card swiper-slide">
            <img class="card-img-top" src="./images/inq_logo.png" alt="Card image cap" />
            <div class="card-body">
              <div class="card-text card-title">[인큐] 프로젝트 관리 매니저</div>
              <div class="card-text">
                <div class="card-info">
                  <div class="info-left">
                    <div class="info-date">
                      모집기간
                      <span class="recruit-date">22.05.10 - 22.05.20</span>
                    </div>
                  </div>
                  <div class="info-right">
                    <div class="info-member">
                      <span class="symbol material-icons">person</span>
                      <span class="member-personnel">
                        2
                        /
                        5
                      </span>
                    </div>
                    <div class="info-progress recruit">모집중</div>
                  </div>
                </div>
              </div>
            </div>
          </a>
``` 
```css
/* Card */
.card {
  width: 600px;
  padding: 50px;
  /* border: 3px solid var(--inq-blue); */
  box-shadow: rgba(17, 17, 26, 0.05) 0px 1px 0px, rgba(17, 17, 26, 0.1) 0px 0px 8px;
}
.card .card-body {
  margin-top: 50px;
}
.card .card-body .card-text.card-title {
  font-weight: 700;
  font-size: var(--large-font);
}
.card .card-body .card-text .card-info .info-left {
  margin:20px 0 20px 0;
}
.card .card-body .card-text .card-info .info-right {
  display: flex;
}
.card .card-body .card-text .card-info .info-right .info-member {
  display: flex;
}
.card .card-body .card-text .card-info .info-right .info-member .symbol {
  font-size: 50px;
  color: #5b5b5b;
}
.card .card-body .card-text .card-info .info-right .info-member .member-personnel {
  margin-top: 7px;
  margin-left: 15px;
}
.card .card-body .card-text .card-info .info-right .info-progress {
  width: auto;
  border-radius: 8px;
  padding: 10px;
  margin-left: 150px;
}
/* 모집중 */
.card .card-body .card-text .card-info .info-right .info-progress.recruit {
  background-color: var(--recruit-color);
}
/* 진행중 */
.card .card-body .card-text .card-info .info-right .info-progress.execution {
  background-color: var(--inq-yellow);
}
/* 완료 */
.card .card-body .card-text .card-info .info-right .info-progress.complete {
  background-color: var(--complete-color);
}
```
- __카드 구조__  
  카드의 구조는 크게 img, card-title, info-right, info-left로 나눌 수 있다.  
  img는 사용자가 등록하는 프로젝트의 이미지이다.  
  card-title은 등록한 프로젝트의 이름이다.  
  info는 두개로 나뉘어 info-right, info-left인데, info-right에는 모집기간, info-left에는 모집중인 인원과 참여 인원, 모집 상태가 나온다. 
- __모집상태 표시__  
  카드에는 해당 프로젝트의 모집상태가 나오게 되는데 모집중, 진행중, 완료 3가지이다. 모집상태에 따라 색상이 다르게 나온다.  
  모집중은 빨강, 진행중은 노랑, 완료는 초록이다. 상태표시 색상은 **신호등**을 착안하여 정했다.
- __box-shadow__  
  카드 테두리에 그림자를 주었다. 내가 직접 설정해서 하기에는 이쁘게 만들기가 힘들었다. 그래서 서칭을 통해 다양한 box-shadow를 정리해둔 
  [box-shadow 사이트](https://getcssscan.com/css-box-shadow-examples)를 찾았다.

<br>

### __언더라인 css__  
홈 페이지와 프로젝트 홈 페이지에 적용되는 언더라인이 있다.  
홈 페이지에서는 프로젝트 홈 페이지로 이동하는 버튼에서 **프로젝트**클릭시 언더라인이 나타나면서 이동한다.  
프로젝트 홈 페이지에서는 검색을 할 때 모집중, 진행중, 완료를 선택한 후 검색을 할 수 있다. 이때, 프로젝트의 상태를 어떤 것을 선택했는지 나타내줄 때 언더라인을 사용한다.  
```html
<div class="text-title">
  <a href="#" onClick="location.href='../project/project_home.html'" class="title title-cursor underline line" title="프로젝트 페이지로 이동">프로젝트</a>
</div>
```
```css
/* Underline */
.underline {
  background-repeat: no-repeat;
  background-size: 0% 100%;
  background-image: linear-gradient(transparent 60%, var(--inq-yellow) 40%);
}
.underline:hover {
  background-size: 100% 100%;
}
.underline.click-line {
  background-size: 100% 100%; 
}
```
```js
$('.underline').each(function(index){
  $(this).attr('underline-index', index);
}).click(function(){
  /*클릭된 <div>의 menu-index 값을 index 변수에 할당한다.*/
  var index = $(this).attr('underline-index');
  /*클릭한 <div>에  click-line 클래스 추가*/
  $('.underline[underline-index=' + index + ']').addClass('click-line'); 
    /*그 외 <div>는  click-line 클래스 삭제*/
  $('.underline[underline-index!=' + index + ']').removeClass('click-line');
});
```
- __HTML__  
  사용하고자 하는 부분의 클래스에 **underline**을 작성한다.
- __CSS__  
  - __background: linear-gradient();__  
    linear-gradient(방향 또는 각도, 색상과 정지 지점 ... )  
    두개의 색상이 직선을 따라 점진적으로 색상이 변경되게 해준다.  
    내가 작성한 `background-image: linear-gradient(transparent 60%, var(--inq-yellow) 40%);`은, 투명 배경을 화면의 60%, 노란색 (--inq-yellow)을 40%로 설정했다.  
    이렇게 %로 원하는 화면의 비율을 설정할 수도 있다.  
    방향(각도)를 줌으로 자연스럽게 색상이 섞이게 하고 색상이 변경되는 방향도 설정할 수 있다.  
    `background-image: linear-gradient(90deg, red 20% ,yellow 80%)`    
    해당 코드는 색상이 변경되는 각도는 90도(오른쪽) 빨강은 20%, 노랑은 80% 차지한다.
  - __background-size__  
    요소의 배경 이미지의 크기를 설정한다. 그대로 두거나, 늘리거나 줄이는 등을 할 수 있다. 배경 이미지로 덮히지 않는 부분은 `background-color` 속성으로 채워지며, 배경 이미지가 투명, 반투명해도 background-color 색상이 보인다.  
    background-size는 키워드값, 단일값, 두개값, 다중배경, 전역값이 있다. 이중에서 내가 사용한 것은 **두개값**이다.  
    `background-size: 0% 100%;`의 첫번째 값은 너비, 두번째 값은 높이를 설정한다.  
    hover와 click을 하지 않았을 때는 아무것도 없다. (너비 0% 높이 100%) 해당 언더라인 부분에 마우스를 올리거나 클릭했을때는 너비가 100%이 되면서 linear-gradient 속성이 보이게 된다. 
- __JS__