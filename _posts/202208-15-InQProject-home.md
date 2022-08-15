---
layout: post
categories:
  - Project
  - TIL
title: "InQ Project Manager: 메인 페이지, HOME 만들기"
tags:
  - InQ
  - project
  - InQ Project Manager
  - TIL
  - CSS
  - HTML
  - JS
---

## __메인 페이지__
---

메인 페이지는 크게 2개로 나눌 수 있다.  
회원정보 파트와 프로젝트 파트로 나누며 회원정보 파트에는 회원가입하면서 기재한 내용들을 확인 할 수 있다. 프로젝트 파트에는 프로젝트 페이지로 이동과 모집중인 카드를 볼 수 있다. 

## __회원정보 파트__
---

<img width="1499" alt="홈1" src="https://user-images.githubusercontent.com/77609591/184624096-f333e423-a43c-4c0f-ba1d-1600e2a3c977.png">

### __구성__
- 최상단에 본인의 포지션 표시
- 본인의 깃허브로 이동하는 버튼
- 회원가입시, 작성한 한 줄 소개
- 보유 스킬
- 참여한 프로젝트   
  더보기 버튼 클릭을 통해 프로젝트 페이지로 이동이 가능하다.  
  본인이 참여한 프로젝트의 제목을 클릭하면 해당 프로젝트의 상세 설명 페이지로 이동한다.

### __디자인__
상단에 위치한 물결을 이미지가 아닌, html과 css 코드를 이용하여 만들었다.  
직접 하나하나 만들기에는 어렵기에 [wave 사이트](https://www.shapedivider.app/)의 힘을 빌려 제작하였다. 해당 페이지는 원하는 설정을 하면 이에 해당하는 코드를 만들어준다. 만들어준 코드를 복사하여 붙히기만 하면 끝이다. 
- **wave 디자인**
  사이트에서 만든 디자인을 복붙하면 다음과 같이 나온다.   
  여기에 나는 추가로 배경을 넣어주었다.  
  배경 이미지를 삽입하고 원하는 것으로 설정해주면 끝이다!
  ```html
  <div class="custom-shape-divider-bottom-1652199240">
    <svg data-name="Layer 1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1200 120" preserveAspectRatio="none">
      <path d="M985.66,92.83C906.67,72,823.78,31,743.84,14.19c-82.26-17.34-168.06-16.33-250.45.39-57.84,11.73-114,31.07-172,41.86A600.21,600.21,0,0,1,0,27.35V120H1200V95.8C1132.19,118.92,1055.71,111.31,985.66,92.83Z" class="shape-fill"></path>
    </svg>
  </div>
  ```
  ```css
  .custom-shape-divider-bottom-1652199240 {
  position: absolute;
  bottom: 200px;
  left: 0;
  width: 100%;
  overflow: hidden;
  line-height: 0;
  }

  .custom-shape-divider-bottom-1652199240 svg {
    position: relative;
    display: block;
    width: calc(100% + 1.3px);
    height: 400px;
    transform: rotateY(180deg);
  }

  .custom-shape-divider-bottom-1652199240 .shape-fill {
    fill: #FFFFFF;
  }
  /* wave 배경 이미지 설정 */
  .member-info {
    margin-top: 200px;
    background-image: url("../images/home_back.svg");
    background-repeat: no-repeat;
    background-size: cover;
  }
  ```

### __투명도 설정__
내가 만든 박스에 투명도를 넣어주고 싶다면 가장 간단한 방법은 opacity를 통해 투명도를 넣어주는 것이다.  
그래서 나는 회원정보의 박스에만 투명도를 약간 주기 위해서 opacity를 사용하였다. 그런데, 배경에도 같이 투명도가 들어갔다. 

- **문제점**  
  opacity를 이용하여 박스에 투명도를 주려고 했으나 그의 부모 요소인 배경또한 같이 투명해짐.
  ```css
  .member-info {
  margin-top: 200px;
  background-image: url("../images/home_back.svg");
  background-repeat: no-repeat;
  background-size: cover;
  } /* 얘도 같이 투명해짐 (부모요소) */
  .member-info .text-body .row {
  opacity: 0.9; /* 배경 + 자식요소까지 투명해짐*/
  }
  ```
- **해결방법**
  opacity를 사용하는 것이 아닌, background-color rgba()를 사용하여 투명도를 준다. 이렇게 하면 원하는 배경만을 투명하게 할 수 있다. 
  ```css
  .member-info .text-body .row {
    background-color: rgba(255,255,255,0.9);
  }
  ```