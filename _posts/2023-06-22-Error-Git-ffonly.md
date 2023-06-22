---
layout: post
categories:
  - TIL
title: 'Git Error: "fatal: Not possible to fast-forward, aborting"'
tags:
  - TIL
  - Git
  - Error
---

프로젝트를 진행하던 중, main 브랜치의 내용을 soha 브랜치로 가져오려고 pull을 하던 중 다음과 같은 에러가 발생했다.  
`fatal: Not possible to fast-forward, aborting`  
뜻은 ff-only를 사용 불가능해서 중단됐다.. 뭐 이런 소리 같다.
<img width="631" alt="에러 화면" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/73e09f67-febc-4370-8e4d-0087b10e5cba">

해결 방법을 엄청 서칭한 결과 rebase를 해서 confilct를 해결하고 브랜치로 다시 push를 하면 된다~~ 라고 해서 열심히 해봤지만..? 안됐다.. 그래서 [stack overflow](https://stackoverflow.com/questions/13106179/error-fatal-not-possible-to-fast-forward-aborting)의 다른 해결 방법을 통해 해결했다!

`git pull --no-ff` 해당 명령어를 입력해주고 다시 add와 commit을 하니 정상적으로 잘 pull 되었다..ㅜ

<img width="631" alt="에러 수정" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/6e6c97d5-ba2b-479a-992c-7b75f56aae8f">
<img width="631" alt="에러 해결" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/7afd9917-e485-4022-8004-63c0636bb9b0">

매번 느끼는거지만 git은 참.. 어렵다.. 알다가도 모르게 만들어.. 열심히.. 공부해야겠다..
