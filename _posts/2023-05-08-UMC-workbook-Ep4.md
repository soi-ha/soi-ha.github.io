---
layout: post
categories:
  - TIL
title: 'UMC: 마켓컬리 클론코딩, Routing'
tags:
  - TIL
  - UMC
---

## **Router 사용하기**

---

router의 적용 순서는 다음과 같다.  
BrowserRouter 연결 → 페이지 컴포넌트 만들기 → Route path 연결 → Link 컴포넌트 연결

1. **BrowserRouter 연결**

App.js 파일에 react-router-dom에 내장되어 있는 BrowserRouter 컴포넌트를 불러온다.

2. **페이지 컴포넌트 만들기**

Market과 Beauty 페이지 컴포넌트를 만들었다.

3. **Route path 연결**

Route 컴포넌트로 해당하는 URL을 지정한다. URL 지정은 path를 통해 지정한다.

  <img width="536" alt="스크린샷 2023-05-03 23 15 03" src="https://user-images.githubusercontent.com/77609591/236723918-b46412f9-1790-4828-9270-6f354a53e39e.png">
    
4. **Link 컴포넌트 연결**
    
  Link 컴포넌트는 클릭하면 다른 주소로 이동시켜준다.
  
  나는 페이지를 이동할 버튼이 Header 컴포넌트에 존재하기 때문에 Header.js에서 Link 컴포넌트를 연결했다.

  <img width="449" alt="스크린샷 2023-05-03 23 14 57" src="https://user-images.githubusercontent.com/77609591/236723932-3915583f-0298-489a-928d-e1bc070ceff0.png">
  
  <img width="586" alt="스크린샷 2023-05-03 23 14 51" src="https://user-images.githubusercontent.com/77609591/236723933-446473f4-af2e-4d09-ad50-c7ed2ee3f6bf.png">

## **문제 및 해결**

Link 컴포넌트 사용시 밑줄 제거

- 문제
  Link 컴포넌트를 사용하면 link 기본 스타일이 적용된다.
- 해결
  Link 컴포넌트에 `style={{ textDecoration: “none”}}` 을 사용하면 Link의 기본적인 스타일 속성이 사라진다.

> 참고  
> [[React] Link to 밑줄 없애기](https://florescene.tistory.com/229)

## **종합소감**

이번 umc 4주차 워크북 미션은 기존에 만들었던 프로젝트에 라우팅을 적용해보는 것이었다. 이전에 강의로 공부를 한 적이 있긴 하지만... 영상의 도움없이 혼자 적용해 보는 것은 처음이라, 이전에 정리했던 정리글도 다시 확인하고 여러 글들을 서칭했다.  
이번에 라우팅을 적용하고 다른 팀원들에게 적용하는 방법을 설명하면서 완벽하게 학습을 할 수 있었다. 혼자 서칭하면서 적용하기에 성공하고, 내가 학습한 내용을 다른 사람에게 설명까지 하니 완벽하게 나의 지식이 되어 만족스러웠다.
