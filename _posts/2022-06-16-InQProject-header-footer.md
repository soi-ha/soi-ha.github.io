---
layout: post
categories:
  - Project
  - TIL
title: "InQ Project Manager: Header 와 Footer 제작"
tags:
  - InQ
  - project
  - InQ Project Manager
  - TIL
---
## __Header 제작__
---
내가 가장 먼저 만든 것은 역시 Header이다.  
헤더 좌측에는 동아리 로고를 넣었고, 우측에는 회원 이름 (비 로그인시, 로그인하기 버튼)을 나오게 하였다.  
동아리 로고를 클릭하면 메인 홈 화면으로 이동하도록 링크를 걸어 두었다. 우측 로그인 혹은 회원이름에 마우스를 hover하면 노란 박스 배경이 생긴다. 비 로그인시에 로그인 버튼을 클릭하면 로그인 화면으로 넘어간다. 회원 이름일때에 클릭하면 홈 화면으로 넘어간다. (홈 화면에 회원 정보가 있기 때문이다.)  

<img width="1512" alt="로그인" src="https://user-images.githubusercontent.com/77609591/174257951-b0806be7-19b2-46f9-b1dd-136b322f01a0.png"> 

### __Header 코드__  
헤더는 로그인과 비로그인 상태 두가지가 존재하기 때문에 파일을 두개로 만들었다.  
로그인 상태일때는 "OO님 반갑습니다", 비 로그인 상태일때는 "로그인"으로 상태에 따라 멘트가 다르다.  
해당 페이지에 적절한 로그인 상태가 다르기 때문에 따로 구별을 하였다.  
__로그인 상태__: 홈, 프로젝트, 프로젝트 상세, 프로젝트 등록  
__비로그인 상태__: 로그인, 회원가입

```html 
  <!-- 로그인 상태 -->
  <header>
    <div class="header-inner">

      <div onClick="location.href='../home.html'" class="logo">
        <img src="../images/inq_logo.png" alt="InQ" />
      </div>

      <div class="menu">
        <a href="javascript:void(0)" class="bton login_name">인큐님 반갑습니다</a>
      </div>

    </div>
  </header>

  <!-- 비 로그인 상태 -->
  <header>
    <div class="header-inner">

      <div onClick="location.href='../login/index.html'" class="logo">
        <img src="../images/inq_logo.png" alt="InQ" />
      </div>

      <div class="menu">
        <a href="javascript:void(0)" class="bton login_name">로그인</a>
      </div>

    </div>
  </header>
```
- onClick을 사용하여 클릭시, 원하는 페이지로 이동할 수 있게끔 했다.  
- menu div를 만들어 hover시 버튼 박스가 나오도록 css를 작성했다.  
```css
/* Header */
header {
  position: fixed;
  top: 0;
  width: 100%;
  background-color: var(--inq-blue);
  z-index: 9;
}
header > .header-inner {
  height: 200px;
}
header .logo {
  /* height: 150px; */
  position: absolute;
  top: 0;
  bottom: 0;
  left: 30px;
  margin: auto;
  cursor: pointer;
}
header .logo img {
  height: 200px;
}
header .menu .login_name {
  width: auto;
  position: absolute;
  right: 30px;
  bottom: 30px;
}
```
header 스타일의 배경색은 `root:`를 통해 색을 정해두었다.   
<br>

## __Footer 제작__
---
footer에 입력할 내용들은 모두 중앙에 위치하게 하였다.  
중앙 최상단에는 깃허브 로고를 넣었다. 해당 로고를 누르면 동아리 깃허브 링크로 이동한다.  
그리고 임시로 작성한 동아리 이메일과 학교 이름을 작성했다.  
최하단에는 copyright 기호와 함께 프로젝트 이름을 작성했다.
### __Footer 코드__
```html
<script src="../js/footer.js"></script>
<footer>
  <div class="inner">

    <div class="info">
      <a href="https://github.com/InQ-InQ-InQ-InQ-InQ" class="github-logo">
        <img src="../images/github.png" alt="inq_logo" />
      </a>
      <span>Email : inqiniqinqinqin@gamil.com</span>
      <span>Kyounggi Univ.</span>
    </div>

    <div class="copyright">
      &copy; <span class="this-year"></span> InQ Project Manager
    </div>

  </div>
</footer>
```
```css
/* Footer */
footer {
  background-color: var(--inq-blue);
  margin-top: 200px;
  color: var(--inq-yellow);
}
footer .inner {
  width: 680px;
  padding: 80px
}
footer .info .github-logo img {
  width: 80px;
  margin: 0 auto;
  margin-bottom: 20px;
  color: var(--inq-yellow);
}
```
<br>

- footer.js 연결하기
```html
<script src="../js/footer.js"></script>
```
- footer.js 코드
```js
const thisYear = document.querySelector('.this-year');
thisYear.textContent = new Date().getFullYear(); // 올해를 계산해줌
```
html 파일에서 클래스명 this-year을 찾아 __thisYear__ 이라는 변수를 생성한다.  
thisYear은 copyright부분의 년도 변경을 알아서 해준다.  
- __textContent__  
__Node__ 속성으로, 사용자에게 보여지는 text값, script, style 등 해당 노드가 가지고 있는 텍스트 값을 모두 가져온다.  
즉, `<p style="color:red;">안녕</p>` 에서 __안녕__ 이라는 텍스트만 가져오는 것이 아닌 <span style='color:red;'>안녕</span> 이라는 텍스트를 가져온다.
- __Date()__  
Date 객체를 통해 시간 생성, 수정, 저장, 측정을 할 수 있다. 현재 날짜를 출력하는 용도 등으로 활용이 가능하다.  
  - __new Date()__  
  새로운 Date 객체를 만든다.  
  new Date()를 인수 없이 호출하면 현재 날짜와 시간이 저장된 Date 객체가 반환된다.
  - __getFullYear()__  
  Date 객체의 메소드이며, 연도(네자리수)를 반환한다.

<br> 

## __common 파일 연결시키기__
#### __html에서 html include 하기__
---
header와 footer는 모든 페이지마다 사용한다. 해당 코드를 매번 추가하는 것은 비효율적이다. 여러 페이지가 존재하기 때문에 유지보수를 하기에 좋지 않기 때문이다.  
그래서 header html코드는 header.html에 footer html코드는 footer.html에 따로 입력해두었다. header와 footer의 css는 commom.css에 입력했다.  
header와 footer html파일을 각 페이지 파일에 include를 하는 방식을 통해 유지보수성을 높였다. 
### __head 태그에 삽입__
```html
<head>
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
  <script type="text/javascript">
      $(document).ready( function() {
        $("#headers").load("../common/header.html");  // 원하는 파일 경로를 삽입
      });
      $(document).ready( function() {
        $("#footers").load("../common/footer.html");  // 원하는 파일 경로를 삽입
      });
  </script>
</head>
```
- __jquery 부르기__
- __script 태그 사용__   
  - __ready 메소드안에 원하는 파일의 경로를 삽입한다.__  
  ready(): js의 DOM 트리가 준비되었을 때 시점을 컨트롤 하는 메소드이다. 해당 메소드는 외부 라이브러리 및 이미지와는 상관없이 DOM 데이터 로드시 바로 사용이 가능하다. (window.onload()보다 더 빨리 실행된다.) 여러번 사용시 선언 순서에 따라 순차적으로 실행된다.
  - __load()를 사용하여 파일 가져오기__  
  load(): 입력한 데이터를 불러온다.
  - __`$(#headers), $(#footers)`__  
  body에서 불러올 아이디 이름을 입력한다.  
    - `$(선택자).동작함수()`  
    $ 기호는 jquery를 의미하며 jquery에 접근할 수 있는 식별자이다. 선택자를 통해 원하는 html요소를 선택하고 동작함수를 정의하여 선택된 요소에 원하는 동작함수를 설정한다.
    - `$()`: 선택된 HTML 요소를 jquery에서 이용할 수 있는 형태로 생성해준다.
    인수로는 HTML 태그, CSS 선택자를 전달하여 특정 HTML 요소를 선택할 수 있다.
    `$()`함수를 통해 생성된 요소를 jquery 객체(jQuery Object)라고 한다.

### __body 태그에 삽입__
```html
<body>
  <div id="headers"></div>

  <div id="footers"></div>
</body>
```
- __id를 통해 값 불러오기__  
head에서 정의했던 id를 불러온다. 해당 id가 가지고 있던 값이 출력된다.

<br>

## __마무리__
---
간단해 보였던 header와 footer였지만 생각보다 고려해야 할 점들이 많았다. 위치라던가, 색상 조합, 기능들 등등...  
그리고 header와 footer은 모든 페이지에 공통적으로 들어가는 내용이기 때문에 어떻게 하면 매번 수정할 때마다 코드를 복붙하지 않고 편하게 단 한번으로 변경할 수 있을 까 생각을 했었다.  
그때, 3학년 전공수업때 php를 다루며 파일을 include 하던것이 생각났다. php include는 알겠는데 html파일을 html에 include하는 법을 몰라 한참을 서칭해서 겨우 찾아냈다.  
하지만 바로 이해가 안되서 코드를 붙혀 넣었지만 안돌아가고... 여러가지 건들여보면서 작동시키게 하는 등 파일 하나 연결하는데 엄청 시간을 사용했던 것 같다. 그래도 이 시간이 언젠가는 피가되고 살이 되겠지..?! 