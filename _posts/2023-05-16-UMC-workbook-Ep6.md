---
layout: post
categories:
  - TIL
title: 'UMC: 마켓컬리 클론코딩, styled-component & 반응형 웹'
tags:
  - TIL
  - UMC
---

## **Styled Component 적용하기**

---

마켓컬리 접속 시, 뜨는 팝업창에 스타일드 컴포넌트를 사용했다.

<img width="544" alt="스크린샷 2023-05-11 00 04 21" src="https://github.com/soi-ha/UMC_Workbook/assets/77609591/4606b742-059c-4448-b694-4438fdfec06b">

<img width="730" alt="스크린샷 2023-05-11 00 04 39" src="https://github.com/soi-ha/UMC_Workbook/assets/77609591/0302e9d4-d52e-44d9-91d0-270ffbda6e3b">

PopupWrapper와 ButtonBox를 생성하여 스타일을 적용해줬고, button 스타일은 새로 생성하는 것이 아니라 태그에 스타일드 컴포넌트를 사용하여 스타일을 적용하였다.

<img width="1507" alt="스크린샷 2023-05-16 09 18 48" src="https://github.com/soi-ha/UMC_Workbook/assets/77609591/bb4bc6f4-30e8-43fb-9aaf-f14a0e0409cf">

화면에 잘 출력되는 것을 확인할 수 있다!

## **반응형 웹 구현**

---

<img width="441" alt="스크린샷 2023-05-11 02 06 50" src="https://github.com/soi-ha/UMC_Workbook/assets/77609591/8a8bfdf3-5a71-436b-a9fb-3d8d029d3ae4">

미디어 쿼리를 사용하여 화면이 1260px 이하 일 때, 다음과 같이 스타일이 변경되도록 설정하였다.

<img width="1510" alt="스크린샷 2023-05-16 09 19 36" src="https://github.com/soi-ha/UMC_Workbook/assets/77609591/2677519a-37ab-4ecf-b29e-d93e7ce4ba9d">

<img width="1512" alt="스크린샷 2023-05-16 09 16 09" src="https://github.com/soi-ha/UMC_Workbook/assets/77609591/1b4bb80b-4332-4444-86db-edc2cf704a32">

<img width="1512" alt="스크린샷 2023-05-16 09 16 23" src="https://github.com/soi-ha/UMC_Workbook/assets/77609591/0cde3e7a-28b0-427c-934a-b3cf64d4c615">

화면의 비율이 줄어들어도 아이템 잘림이 없이 잘 줄어든 것을 확인할 수 있다.

## **문제 및 해결**

---

- 중첩 적용 안됨 (미디어쿼리)
  - 문제
    content-right의 left를 36px로 설정했는데, 화면 사이즈가 1260px이 되었을 때, left 설정했던 것을 없애려고 left: 0;을 하였으나 중첩이 적용되지 않았다.
  - 해결
    중첩이 안되는 이유는 찾지 못했지만..
    해결은 `!important`를 사용하여 left: 0;이 무조건 적용하도록 했다.

## **소감**

---

미디어 쿼리는 사용해본 적이 있지만 styled-component는 처음 배우고 사용해 보았다. 처음에는 styled-component를 복잡하게 생각하고 느꼈는데, 새삼 이것처럼 간단하게 배울 수 있는건 없는 것 같다. 시작도 안하고 지레 겁먹는 것은 좀 고쳐야 겠다..!  
반응형 웹을 만들기 위해서 미디어쿼리를 사용했는데, 생각해줘야 할 것이 은근 많았다. 그래서 화면의 비율에 따라 잘 줄어드는 것을 보면서 뿌듯했다..!
