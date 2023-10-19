---
layout: post
categories:
  - TIL
  - Project
title: 'Node.js: 버전 업그레이드 (feat. NPM)'
tags:
  - TIL
  - WoowaCourse
  - Node.js
---

본격 미션 시작하기에 앞서 기본 셋팅을 하던 중... 발생한 에러를 적고자 한다..

나의 브랜치에 fork랑 clone은 매우 간단하게 했고, Node.js와 NPM 버전을 업그레이드 하는 것이.. 시작전부터 날 힘들게 만들었다...

Node.js 버전 업그레이드는 매우 간단하게 성공했다.  
n (npm n)을 사용해서 Node.js 버전을 업그레이드 했었는데, 맨 처음에 Node.js를 설치했을 때 NVM으로 했어서 installed와 active 경로가 일치하지 않아 일치시키는 것까지 완료하여 매우 간단하게 해결했다. (미래의 내가 블로그에 글 작성해둔거.. 정말 잘했다 ㅜ)

근데? npm은 자동으로 업그레이드 되지 않았다. 그래서 `npm install -g npm` 명령어를 통해 업그레이드를 진행하려 했다. 그러나 아무리 터미널에 버전을 확인해도 업그레이드가 되지 않고 이전 버전이라고 나왔다.. 아무리 해도... 그리고 삭제 하고 다시 설치하려고 해도.. 에러 뜨고... 그야말로 1시간의 멘붕시간이었다.

~~아래 사진 에러 말고도 굉장히 다양한 에러가 나타났다~~
<img width="486" alt="스크린샷 2023-10-20 00 49 17" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/b1aea9aa-6d6f-4d49-b84a-84a2facce6d0">

그러다가 문뜩 떠오른 것이 Node.js의 installed(npm n)와 active(nvm)의 경로가 다른데.. 그냥 Node.js의 버전 업그레이드를 NVM으로 해보자!였다.

<img width="486" alt="스크린샷 2023-10-20 00 49 05" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/e8a40133-a047-4977-b5dc-4be2a240e4cd">

기존에는 n으로 설치하니까 자꾸 두 경로가 다르게 뜬다는 생각이 들었다. Node.js를 NVM으로 설치하면 installed 경로도 active처럼 NVM으로 될 것이라고 생각했다.

그리고 이 생각은 맞았다.

<img width="486" alt="스크린샷 2023-10-20 00 48 47" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/da884098-36b2-4e1d-bfe2-a9c06221d759">

NVM으로 Node.js를 설치하니 모든게 간.단.하게 해결되었다... Node.js 업그레이드와 NPM 업그레이드까지...

그렇다면 왜 NVM으로 Node.js를 설치했을때 NPM도 업그레이드 된 걸까?

### N과 NVM

---

#### N

n은 npm의 패키지로, npm을 이용해 n을 설치하고 n으로 Node.js 버전을 업그레이드한다. 여기서 주의할 점은 n으로 버전을 업그레이드하고 싶다면 일단 Node.js가 설치되어있어야 한다.
[n으로 Node.js 업그레이드](https://soi-ha.github.io/til/2023/04/11/Node-Version-Update.html)를 하는 방법은 해당 링크를 참고하자.

#### NVM

NVM은 n과 달리 npm의 패키지가 아니라 개별적인 존재이다.
그렇기에 NVM은 Node.js에 의존적이지 않다. 오히려 Node.js가 NVM의 의존적이다. (NVM이 Node Version Manager인 이유랄까...?)  
그리고 이번 에러를 통해 느낀 n과 달리 NVM의 가장 큰 장점은. NVM으로 Node.js를 업그레이드하면 NPM의 버전또한 같이 업그레이드가 된다는 것이다! (따로 업그레이드 안해도 되니까 너무 좋더라..)

#### NVM 사용 방법

- nvm 버전 확인  
  `npm -v`

- nvm 설치된 버전 확인  
  `npm ls`

- nvm으로 node.js 설치  
  `nvm install <원하는 node.js 버전/lts or 20(버전 숫자)>`

---

#### N과 NVM 둘 중 무엇을 사용?

이번에 느낀 건데, 둘 중에 뭘 사용하는 것이 좋냐?라고 묻는다면 난 NVM을 하겠다.. 일단 Node.js에 의존적이지 않아서 좋은 것 같다. 그리고 NPM도 같이 업그레이드가 되고...  
이번에 느낀건 사람들이 많이 쓰는데는 이유가 있다~^^ 라는 것이다.

### 마치며

---

미래의 나.. Node.js 업그레이드... N아니고 NVM으로 했다.. 자꾸 N으로 업그레이드 하지 말아라... 에러 또 날라..ㅜ

> 참고  
> [Node.js 버전 변경하기 (with NVM 사용법)](https://oingdaddy.tistory.com/481)  
> [node.js 버전 관리 매니저::nvm과 n 비교 및 nvm 설치 방법 정리 ](https://hoho325.tistory.com/457)
