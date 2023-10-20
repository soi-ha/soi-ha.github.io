---
layout: post
published: true
categories:
  - TIL
title: 'npm ERR! EACCES: permission denied, mkdir <경로>'
tags:
  - TIL
  - WoowaCourse
  - Node.js
---

[지난 글](https://soi-ha.github.io/til/project/2023/10/19/WoowaCourse-Ep2.html)에서 node.js 버전 업그레이드 오류를 해결했으나, 이번에는 `npm i`가 작동하지 않는 에러가 발생했다.. 이 오류 해결하는데 한시간 반은 걸린 것 같은데.. 아무리 서칭해서 적용해도 해결되지 않아서... ㅎ흙흑 겨우 해결방법 글을 발견하여 에러를 해결했다!! 미래의 내가 같은 에러를 마주치면 그때 쉽게 해결할 수 있도록 글을 적어본다..

**문제의 에러 화면**

<img width="491" alt="스크린샷 2023-10-20 02 08 56" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/e5233ab8-70b7-4232-aa7f-b75dde3269a6">

어떤 에러인지 확인해보자면 해당 파일에 권한이 없음! 과 파일 이미 존재하니까 삭제하고 다시 해보셈! 그리고 `--force` 사용해보셈! 인걸로 나는 이해했다.

<img width="491" alt="스크린샷 2023-10-20 01 28 08" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/3d4df3c0-3c68-4f46-92f3-c878d7237e2e">

그래서 `--force`를 붙여서 해봤다. (대부분 이렇게 하면 해결된다고 하더라) 응, 근데 난 해결이 안됐다 ^^! 그래서 파일도 삭제해봤지만.. 해결이 안됐다.

그러던 중 발견한 정말 나의 [구원글](https://helloinyong.tistory.com/191)..ㅜㅜㅜ 정말 친절히 작성해주셔서 덕분에 해결할 수 있었다.
_(자고 싶었는데 해결 안하면 도저히 잠이 안 올 것 같아서 해결방법을 찾던 나에게 자도 된다고 선언해준 글..)_

### permission denied 에러 해결

---

#### 원인

해당 에러가 발생한 이유는 이거였다. "접속중인 local 계정이 npm 설치 경로에 권한을 가지고 있지 않다."

아마 추측하기로는 다른 에러 해결한다고 sudo를 좀 많이(?)사용한 것 같은데 그래서 이 에러가 발생한 것 같다. (해당 글에서도 node를 sudo를 사용하여 설치해서 에러가 발생할 가능성이 높다고 했다.)

#### 해결방법

해당 에러를 해결하는 방법으로 택한 것은 "`npm install -g` 로 설치되는 디렉토리 경로를 현재 계정의 home directory로 변경" 하는 것이다. (참고한 글의 분이 선택한 방법이다!)

- npm global directory 생성

  `mkdir ~/.npm-global`

- 생성한 디렉토리에 npm config set 설정

  `npm config set prefix '~/.npm-global'`

- '.npm-global' 파일에 library path 설정 추가

  `nano ~/.profile`

  해당 명령어를 입력하면 편집기가 켜지는데, 파일에 다음과 같은 코드를 추가한다.  
  `export PATH=~/.npm-global/bin:$PATH`

- '.npm-global' 파일 설정 적용

  `source ~/.profile`

<img width="491" alt="스크린샷 2023-10-20 02 09 23" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/d1903e68-ee71-42fe-80b6-bafdb5b12414">

위 단계를 모두 한 후, npm 명령어를 실행했는데도 에러가 계속 발생한다면?

- default directory에 대한 권한을 다시 한번 변경

  `sudo chown -R $USER:$GROUP ~/.npm`

이 모든 명령어들을 수행 한 후, `npm i`를 실행했더니.....!!!! 설치가 됐다 ~~흠엄ㄴ럼ㅇ러구ㅜㅜ~~

<img width="491" alt="스크린샷 2023-10-20 02 09 06" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/c9c167c7-6d1f-4bcb-b09d-2f58e9dc554f">

참고로 나의 경우에는 default directory 권한을 변경하는 것까지 수행한 후에 에러가 해결 됐다..

### 마치며

---

정말... 험난했다.. 하.. 그래도 해결하니 기분 좋게 잘 수 있겠더라!ㅎㅎ 이런 맛에 코딩하는 거지... 항상 느끼는 거지만 코딩은 수학문제 같달까... 이 정답을 맞췄을 때의 쾌감.. 내가 이래서 수학을 좋아했지.. 이래서 코딩이 좋은가 보다..헿

> 참고  
> [[2019.09.01] 오늘의 TIL - npm install 중, permission denied 에러 해결방법](https://helloinyong.tistory.com/191)
