---
layout: post
published: true
categories:
  - TIL
title: '우테코: 프리코스 1주차 미션 복기'
tags:
  - TIL
  - WoowaCourse
---

2주차가 곧 끝나가는 시점이지만.. 지금이라도 1주차 미션을 복기해보려고 한다.

나는 그간 React를 조금 한 것 말고는 바닐라 JS로 기능을 구현해본 적이 없다. 있다면 그나마, 학교 강의시간에 간단한 기능 구현정도만? (정말 간단한 기능들...)

그렇기에 처음 1주차를 시작할때는 정말.. 쉽지 않았다. 솔직히 좀 어려웠다. 그나마 프로젝트를 진행한다고 node나 git 등을 다뤄봐서 다행이지, 한번도 사용해보지 않았더라면 프로젝트 설정에서 막혔을 것이다.

그리고 처음 보는 것들이 많았어서 그거에 대한 궁금증 때문에 정작 기능구현은 그리 빨리 시작하지 못했다. 1주차를 하면서 정말 크게 느낀 것은 선택과 집중을 잘 해야 한다는 것이었다.  
다른 것을 신경쓰다가 가장 중요한 기능 구현은 실패헀을 수도 있다..

다행이도.. 그간 코딩좀 타탁거렸던 실력으로.. 정말 많은 난관들이 있었지만.. 잘 해결해서 테스트 실행을 통과시킬 수 있었다.

<img width="785" alt="스크린샷 2023-10-31 23 56 56" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/10ab6f0a-d828-413e-b601-b049de4f5b50">

서론은 여기서 마치고 1주차 미션을 진행하면서 발생했었던 일들에 대해 작성해보고자 한다.

- 예기치 못한 ~

  > new App() 코드문 삭제
  > 파일명 변경(카멜)으로 이전 파일(파스칼)이 적용돼서 빌드 실패

- 테코 실패
  > error 출력문 문제
  > <img width="460" alt="스크린샷 2023-10-24 01 36 20" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/3522b123-ba42-4b4c-932a-797d37832da7"> > <img width="550" alt="스크린샷 2023-10-24 17 21 20" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/ee800b2b-a640-48be-8b14-746f7b3ed6e6">
  > pickNumber 뭐시기

배운점

- 좋은 깃 커밋 메세지
- npm permission denined 에러
- git reset -hard 경로 : 원하는 시점으로 되돌리기 (pull 에러 발생 / git push -f origin 브랜치 로 해결)
- throw를 통한 예외처리
- typeof number
  <img width="1020" alt="스크린샷 2023-10-23 18 01 22" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/ad673a6b-6189-44dd-9984-54881869690e">

아쉬웠던 점

- 클래스 제대로 활용 못함
