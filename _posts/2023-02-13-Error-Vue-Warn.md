---
layout: post
categories:
  - TIL
title: "[Vue warn]: Property '~~' was accesed during ... "
tags:
  - TIL
  - Vue.js
  - Error
---

Vue로 페이지를 제작한 후, 기능이 동작하지 않는 현상이 발생했다..  
일단 문제가 생겼으니 console 탭을 확인하니 다음과 같은 warn 표시가 발생했다.

<img width="472" alt="Vue 오타 에러" src="https://user-images.githubusercontent.com/77609591/218483485-d6007f7d-f9cf-48da-93fa-ba010111d9ea.png">

해석해보면 moives라는 property는 정의되지 않았다. 뭐 이런 뜻인데..  
어디선가 잘못 작성한게 확실했다.   
하단에 후보군도 나와 있으니 모두 뒤져봤으니 나오지 않았다..!!!  
그렇게 헤매다가 다시 콘솔탭을 확인했더니, 웬걸..? movies가 아닌 moives 였던것...!!!  
moives로 단어 검색을 해보니 오타가 발생해 있었다..  

### 그렇다. 해당 에러는 대부분 오타에서 발생한다.. 작성할 때 주의하도록 하자..ㅜ