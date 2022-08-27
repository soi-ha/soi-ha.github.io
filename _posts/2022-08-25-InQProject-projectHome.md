---
layout: post
categories:
  - Project
  - TIL
title: "InQ Project Manager: 프로젝트 페이지, 메인·등록·상세페이지 제작"
tags:
  - InQ
  - project
  - InQ Project Manager
  - TIL
  - CSS
  - HTML
  - JS
---

## __프로젝트 페이지 구성__
---
이번 프로젝트의 가장 핵심 기능 페이지인 프로젝트 페이지이다.  
프로젝트 페이지는 메인 프로젝트 페이지, 프로젝트 등록 페이지, 프로젝트 상세 페이지로 총 3가지의 페이지로 구성되어 있다.

## __메인 프로젝트 페이지__
---

<img width="1496" alt="프로젝트" src="https://user-images.githubusercontent.com/77609591/186700934-276474b3-a27d-4ace-bbd7-a8ff392830a5.png">

등록된 프로젝트들을 모아둔 페이지이다.  
상단의 모집중, 진행중, 완료 중에 하나를 선택하여 원하는 상태의 프로젝트 만을 모아서 볼 수 있다. 또, 상태 선택후 원하는 키워드의 프로젝트를 검색할 수 있다.  


### __HTML__
```html
<div class="search-radio">
  <div class="inner">

    <div class="radio-box">
      <input type="radio" id="recruitment" name="state" value="recruitment">
      <label for="recruitment" class="bton underline">모집중</label>
    </div>

    <div class="radio-box">
      <input type="radio" id="progress" name="state" value="progress">
      <label for="progress" class="bton underline">진행중</label>
    </div>

    <div class="radio-box">
      <input type="radio" id="complete" name="state" value="complete">
      <label for="complete" class="bton underline">완료</label>
    </div>

    <div class="search">
      <span class="icon material-symbols-outlined">search</span>
      <input placeholder="프로젝트명으로 검색하기" type="text" name="search-text" id="search-text">
      <input type="submit" class="bton btn--reverse" value="검색">
    </div>
    
  </div>
</div>
```
- 프로젝트의 상태를 선택하는 것은 input radio를 통해 하나의 상태만을 선택할 수 있도록 했다. 
- 상태 선택의 우측에는 키워드를 작성할 수 있는 검색란을 만들어 주었다.

### __CSS__
```css
/* Search - Radio */
.search-radio {
  margin-top: 200px;

}
.search-radio .inner {
  width: 2500px;
  display: flex;
}
.search-radio .radio-box input[type=radio] {
  width: 0;
  height: 0;
  left: -9999px;
}
.search-radio .radio-box input[type=radio] + label {
  font-size: var(--sub-title-font);
  margin-right: 20px;
  font-weight: 800;
  color: var(--inq-blue);
}
.search-radio .radio-box input[type=radio] + label:hover{
  background-color: transparent;
} 
/* Search - text*/
.search-radio .search {
  position: absolute;
  top: 55px;
  left: 800px;
  margin: auto;
}
.search-radio .search .icon {
  position: absolute;
  font-size: 70px;
  color: var(--inq-yellow);
}
.search-radio .search #search-text {
  width: 600px;
  margin-left: 80px ;
  margin-top: 10px;
  background-color: transparent;
  border: none;
}
.search-radio .search #search-text:focus {
  /* input 클릭시, 테두리 없애기 */
  outline: none;
  color: var(--inq-blue)
}
.search-radio .search #search-text::placeholder {
  color: var(--inq-blue);
  opacity: .6;
}
.search-radio .search input.bton {
  position: absolute;
  width: 100px;
  margin-left: 430px;
  top: 0;
}
```
__radio 기본 스타일 없애기__
- input의 radio에는 기본적으로 적용되는 스타일이 있다. 글자 옆에 동그라미 선택 표시가 뜨는데, 해당 스타일을 원하지 않아 없앴다.
- `[type=radio]`에 높이와 너비를 0으로 하고 화면에 보이지 않도록 left 속성을 -9999px로 함으로 위치를 않보이는 곳으로 옮겼다.  

__input 테두리 없애기__ 
- input 입력칸을 선택하면 파란색 테두리가 생기는데 해당 테두리를 없애기 위해 `outline:none;`을 사용하여 없앴다.

__placeholder 스타일__
- placeholder의 색상은 기본적으로 검정색 글씨로 나온다. 해당 글씨색을 통일 성을 위해 변경해주고 싶었다.
- `::placehlder`을 통해 placeholder의 스타일을 변경할 수 있도록 하였다. color를 통해 색을 변경했고, 글자의 투명도를 주기 위해 opacity를 사용하였다.

## __프로젝트 등록 페이지__
---

<img width="1492" alt="프로젝트 등록" src="https://user-images.githubusercontent.com/77609591/186700964-89fec169-e56b-46e5-a71a-b828a80cf478.png">

프로젝트 등록 페이지는 본인이 프로젝트의 인원을 모집하고 싶을 때, 프로젝트에 대한 사항들을 작성 후 올릴 수 있도록 하는 페이지이다. 필수 입력을 한 후, 자유롭게 프로젝트 소개글을 작성하여 등록하면 프로젝트가 등록된다.  
해당 페이지의 html과 css는 회원가입과 동일하여 따로 작성하지는 않겠다. 
