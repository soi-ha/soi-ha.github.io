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

### __사용자 지정 css 속성__
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
- __사용자 지정 속성__  
  사용자 지정 속성(css 변수, 종속 변수)은 내가 직접 정의한 개체를 의미한다. 현재 진행중인 프로젝트에서 전반적으로 재사용할 값들을 담을 때 주로 사용한다. 표기법과 사용법은 다음과 같다. 
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
  