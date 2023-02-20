---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: CSS, 텍스트 말줄임 표시와 배경 흐림 처리"
tags:
  - TIL
  - Vue.js
  - project
  - CSS
---
## __텍스트 말줄임 표시와 배경 흐림 처리__
---


### __텍스트 말줄임 표시__
  
  ```css
  div {
    whith-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  ```
  
  - `white-space: nowrap;`
    
    공백문자를 처리해주는 속성이다.
    nowrap, 감싸지 않겠다는 의미이다. 줄바꿈 처리가 되지 않고 한줄로만 표시될 수 있도록 한다. 
    
    > [MDN: white-space - CSS](https://developer.mozilla.org/ko/docs/Web/CSS/white-space)
    
    추가) 공백 문자는 띄어쓰기(space)나 들여쓰기(tab) 등을 의미한다. 
      
  - `overflow: hidden;`
    
    텍스트가 박스 바깥으로 튀어나오는 부분을 잘라낸다.
      
  - `text-overflow: ellipsis;`
    
    텍스트가 박스 바깥으로 넘쳐서 잘리는 부분이 말줄임 표시가 된다.
    ellipsis는 생략이라는 의미이다.
    
    text-overflow는 overflow와 white-space와 대부분 같이 사용하며, 해당 방식의 사용이외에는 잘 사용되지 않는다.
    
    > [MDN: text-overflow - CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/text-overflow)
      
### __배경 흐림 처리__
  
  ```css
  div { 
    backdrop-filter: blur(10px);
  }
  ```
  
  - `backdrop-filter: blur();`
      
    backdrop-filter은 해당하는 요소 범위에서 보이는 배경에 css의 여러 필터 효과를 적용해주는 속성이다.
    
    단, 브라우저 호환성을 확인했을 때 Firefox에서는 호환이 불가능하다는 단점이 있다.
    
    - blur(px)
        
      배경을 흐림처리한다.
      
    - grayscale(%)
      
      배경을 흑백처리한다.
        
    
    > [MDN: backdrop-filter - CSS](https://developer.mozilla.org/ko/docs/Web/CSS/backdrop-filter)