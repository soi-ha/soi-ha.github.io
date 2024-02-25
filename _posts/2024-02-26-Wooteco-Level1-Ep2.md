---
layout: post
published: true
categories:
  - TIL
title: 'VSCode 파일 대소문자 충돌 해결'
tags:
  - TIL
  - Error
  - Wooteco
---

이번에도 어김없이 등장한.. 파일 대소문자 충돌 이슈...

처음에 파일을 생성할때 이름을 `OutPutView.js`로 지어버렸다.  
그래서 파일 이름을 `OutputView.js`로 변경해줬다.

그러나.. VSCode에서 `OutputView.js`로 작성하고 import를 해도 처음에 작성했던 `OutPutView.js`로 찾는 것이다.. 그러니 해당 파일을 찾을 수 없어 블라블라..

물론 나는 [git 대소문자 충돌 문제](https://soi-ha.github.io/til/2023/10/31/WoowaCourse-Ep8.html)는 이미 프리코스에서 겪어서 global로 설정해뒀다.

그러니 이건 순수히 VSCode의 문제..!!!

그래서 나는 전지전능한 ChatGPT에게 물어봤다.  
답변은 다음과 같았다.

### VSCode 대소문자 구분 무시

1. VSCode를 열고 설정 메뉴를 연다. (ctrl+ , 또는 Cmd + ,)

2. 검색 창에 "files.associations"를 입력한다.

3. "Files: Associations" 항목을 클릭하고 "Add Item"을 클릭하여 새 항목을 추가한다.

4. 추가된 항목에서 원래 파일 이름과 변경된 파일 이름을 입력한다.  
   `OutPutView` -> `OutputView`

<img width="725" alt="VSCode 설정 이미지" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/ce37f7a2-f668-4462-8e32-1eb01711b161">

5. 이제 변경된 파일 이름에 대한 대소문자 구분이 무시된다!

### 해당 방법의 문제점?

이건 글을 작성하다가 알게 된 것인데, 다른 프로젝트에서 VSCode 설정에 들어가서 항목을 추가했었다. 그런데, 해당 글을 작성하는 프로젝트에서 VSCode 설정에 들어가니 위 이미지처럼 그대로 추가된 항목이 존재했다.

ChatGPT는 해당 프로젝트에서만 적용된다고 했는데... 음...  
조금 더 완벽한 방법을 찾아봐야 할 것 같다.
