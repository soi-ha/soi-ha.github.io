---
layout: post
categories:
  - TIL
title: 'UMC: 마켓컬리 클론코딩, object-fit'
tags:
  - TIL
  - UMC
---

## **마켓컬리 클론코딩**

---

UMC 1주차 미션은 클론코딩이었다.  
클론코딩은 유튜브 강의로 유튜브 클론코딩만 따라서 해본 적 밖에 없어서 어떻게 해야 할지 약간 막막했지만... 이틀동안 열심히 만들어봤다..!  
react나 vue는 사용하지 않고 그냥 만드는 거라... 컴포넌트의 존재가 새삼 최고였다는 것을... 깨닫게 되었다...  
한 파일에 만드니까 뭔가 헷갈린다고 해야 하나..? 정리가 안된 느낌..

그리고 css는... 차마 하나하나 따로 치기에는 너무 헷갈리고 힘든 미래가 보여서 scss를 사용했다. 간단히 scss 사용하는 방법은 다른 글에 작성하기로...

이틀동안 만드느라 시간이 부족해서 자바스크립트는 사용하지 않았다. 페이지 자동으로 변환되거나.. 시간 움직이기 등을 했어야 했는데.. 시간이 없는 관계로 선택과 집중을 했다.

그리고 버튼이나 링크 하나 하나 신경써서 하기에는 시간도 없고.. 복잡(?)하므로 쿨하게 패스하고 다 모양만 만들어줬다.

<img width="1515" alt="마켓컬리 클론코딩" src="https://user-images.githubusercontent.com/77609591/228606387-41beba9b-b7dd-433c-99de-70dad6e587ba.png">

<img width="1519" alt="마켓컬리 클론코딩" src="https://user-images.githubusercontent.com/77609591/228606428-8257db48-4d3a-413b-8fd2-dbbac22ce0d7.png">

_내가 만든 마켓컬리 클론코딩 페이지!_

**어려웠던 점**  
클론코딩을 하면서 어려운 점은.. 기존에 코드를 참고해서 하는데, 해당 코드를 바탕으로 새롭게 나만의 코드 만드는게 처음이라 애를 좀 먹었다. 어떻게 해야 할 지 감을 잡는데 조금 걸렸달까? 그래도 여러번 반복되니 이제 대략 코드들도 이해가 되고 버려도 되는 코드를 금방 파악할 수 있어서 후반부에 갈 수록 코드 작성하는 시간은 적게 걸렸다.

그런 점 말고는... 딱히...?  
그저.. vue나 react를 사용하지 않으니까 정리가 안된 느낌이라 답답했다는 것?! 정도?  
후반부에는 react를 사용하게 될 거니까 괜찮을 것이다.

**종합 소감**  
클론코딩... 결코 만만하지 않은 것이었다..

## **TIL!!**

---

- **object-fit**

  `<img>`나 `<video>` 요소의 크기를 어떤 방식에 맞출 것인지 지정한다.

  - contain

    대체 콘텐츠의 가로 세로비를 유지하면서, 요소의 콘텐츠 박스 내부에 들어가도록 크기를 맞춤 조절한다.

  - cover

    대체 콘텐츠의 가로 세로비를 유지하면서, 요소 콘텐츠 박스를 가득 채운다. 가로 세러비가 일치하지 않으면 객체 일부가 잘려나간다.

  - fill

    콘텐츠 박스 크기에 맞춰 대체 콘텐츠의 크기를 조절한다. 콘텐츠가 박스를 가득 채운다. 가로세로비가 일치하지 않으면 콘텐츠가 늘어난다.

  > [object-fit - CSS: Cascading Style Sheets : MDN](https://developer.mozilla.org/ko/docs/Web/CSS/object-fit)

- **z-index**

  요소를 가장 앞에 배치하고 싶다면 큰 수를 작성한다.  
  단! z-index를 작성할 때, 항상 position 값은 relative로 설정해줘야 z-index가 작동한다. **static일 때는 적용되지 않는다.**

  ```css
  /* 최상위 요소 */
  .first {
  	position: relative;
  	z-index: 9999;
  }

  /* 밑에 깔리는 요소*/
  .second {
  	position: relative;
  	z-index: 10;
  }
  ```

  > [z-index - CSS: Cascading Style Sheets : MDN](https://developer.mozilla.org/ko/docs/Web/CSS/z-index)

- **letter-spacing**

  글자 사이의 간격을 조절한다.  
  값이 커지면 간격이 커진다. 값에 음수를 넣을 수 있으나 글자가 겹칠 수 있다.  
  추가로 단어 사이의 간격은 word-spacing이다.

  ```css
  p {
  	letter-spacing: 10px;
  }
  ```

  > [letter-spacing - CSS: Cascading Style Sheets : MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/letter-spacing)
