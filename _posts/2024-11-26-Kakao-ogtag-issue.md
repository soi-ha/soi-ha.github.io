---
layout: post
published: true
categories:
  - TIL
title: '카카오톡 수정한 OGTag 미반영 문제 해결 방법'
tags:
  - 카카오톡
  - TIL
---

오랜만에 블로그를 조금 손 보기로 했다.  
검정색이던 블로그 색상을 파랑으로 바꾸고, favicon이랑 OGTag도 수정해줬다.  
수정하는 것은 매우 수월했다. 그리 어려운 작업도 아니었으니까. 그러나, 문제는 수정한 내용을 확인하는 과정에서 발생했다.

_글을 읽기 전 OG가 뭔지 모르겠다면? 간단하게 [여기](#틈새-학습-og-open-graph란)를 읽고 오자._

## 문제

블로그 색상과 favicon은 정상적으로 반영되었고 OGTag 또한 잘 반영됐다. 근데, 카카오톡에 해당 url을 공유하면 수정 전의 OG로 나타나는 것이다! 다른 곳에서는 모두 정상적으로 새롭게 반영한 OG로 나타났지만 오직 카카오톡에서만 이전 값으로 나타났다.

## 원인

그래서 나는 카카오톡에 원인이 있다고 판단했다. 아마도 캐시가 남아서 그런거지 않을까?하고 PC 카톡과 모바일 카톡의 캐시를 지워보고자 했다. 지운것 같은데도 여전히 이전 OG로 나타났다!ㅜㅜ

<img width="315" alt="카카오톡_og태그_문제2" src="https://github.com/user-attachments/assets/123a78ad-cfe6-4b37-a563-be670e2c31e9">

여러 서칭을 통해 카카오톡(카카오스토리)의 캐시는 kakao developer 페이지에 접속해서 지울 수 있다는 것을 알게 되었다. 서버 효율을 높이기 위해서 일정기간 OG를 캐싱하여 OGTag를 수정해도 이전 데이터 값으로 나왔던 것이었다.

<img width="612" alt="카카오 Dev Talk 캐싱 삭제 방법 설명" src="https://github.com/user-attachments/assets/2777191f-cab9-4f7f-a326-eee817a4ce8c">

_[🔗 이미지 내용 원본 링크](https://devtalk.kakao.com/t/topic/16724)_

## 해결

[해당 링크](https://developers.kakao.com/tool/debugger/sharing)에 접속하여 URL란에 캐시 초기화를 하고 싶은 url을 입력하면 끝이다!

<img width="666" alt="카카오톡_og태그_문제해결2" src="https://github.com/user-attachments/assets/71fa4ce1-928a-4b4b-bfd1-d88e13929811">

단, 해당 서비스를 이용하려면 카카오 로그인이 필수이다.

## 결과

`https://soi-ha.github.io/` url과 그 하위 url까지 모두 정상적으로 변경한 OG가 잘 반영된 것을 확인할 수 있다!

<p style="display:flex; gap:10px; overflow-x: auto;">
  <img width="50%"  alt="카카오톡_og태그_문제해결" src="https://github.com/user-attachments/assets/ecfe09e7-79e7-4429-ac6c-5a230d591f03"/>
  <img width="50%" alt="카카오톡_og태그_문제해결2" src="https://github.com/user-attachments/assets/b5b9e834-72c1-43b4-9560-da8c423d7216"/>
</p>

## 틈새 학습: OG (Open Graph)란?

Open Graph란, SNS로 웹 페이지를 공유할 때 해당 페이지를 미리 볼 수 있도록 해주는 것이다.  
Open Graph 설정을 통해 페이지의 이미지, 내용, 제목이 미리보기 형식으로 확인할 수 있다.

### OG 왜 사용함?

Open Graph 사용을 통해 사용자의 클릭률을 높일 수 있기 때문이다. 미리보기를 통해 웹 페이지의 내용을 추측하여 사용자가 원하는 글에 접근할 확률을 높여준다. 만약 OG가 없는 상황에서 사용자가 url을 클릭했는데 원했던 내용이 아니라면? 사용자는 더이상 공유된 url을 클릭할 확률이 줄어든다. 그렇기에 OG로 사용자의 클릭률을 높일 수 있다.

또한, 미리보기를 통해 url만 존재할 때 보다 사용자의 눈길을 끌기 쉽다. 미리보기 이미지가 같이 제공되기 때문에 사용자의 눈길을 잡기 수월할 것이다.

## 참고

- [카카오스토리 공유시 이미지 캐쉬는 어떻게 지우나요?](https://devtalk.kakao.com/t/topic/16724)
- [오픈 그래프(Open Graph) 개념부터 적용 후 확인까지 총정리](https://idearabbit.co.kr/%ea%b2%80%ec%83%89-%ec%97%94%ec%a7%84-%ec%b5%9c%ec%a0%81%ed%99%94-%eb%b0%a9%eb%b2%95/open-graph/)
- [What is Open Graph and how can I use it for my website?](https://www.freecodecamp.org/news/what-is-open-graph-and-how-can-i-use-it-for-my-website/)
