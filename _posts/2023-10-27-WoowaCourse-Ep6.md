---
layout: post
published: true
categories:
  - TIL
title: 'Git reset: 원하는 시점으로 되돌리기 (feat. git push -f) '
tags:
  - TIL
  - WoowaCourse
  - Git
---

1주차 프리코스 기능 구현을 완료하고 리팩토링을 진행하고, 우테코 홈페이지에서 예제 테스트를 실행하니 "예기치"씨가 등장했다...  
어떤 것이 빌드를 실패하게 만든 것인지 찾기 위해 계속 커밋하고, 테스트 돌리고, 커밋하고를 반복했으나 찾을 수 없었다.  
그래서 나는... 예제 테스트가 잘 돌아가던 시점으로... 돌리기로 했다...

## `git reset`

---

이동하고 싶은 커밋 시점으로 되돌려주는 git 명령어이다.

- `git reset (--mixed)`

  기본 명령어로, 돌아간 이후 변경 내역이 남아 있으나 인덱스 값이 초기화된다.

- `git reset --soft`

  원하는 시점으로 돌아가고 이후의 커밋을 유지한다.

- `git reset --hard`

  원하는 시점으로 이동하고 이후의 커밋들은 모두 사라진다.

- 직전의 커밋으로 되돌리기

  `git reset --hard HEAD^`

- 원하는 커밋으로 되돌리기

  `git reset --hard <원하는 커밋의 해시값>`

### 이동하고 싶은 커밋의 해시값 확인하기

---

- `git log`

  git의 커밋 및 푸시 내역을 확인할 수 있다.

- Github 레포지토리에서 확인하기

  해당 저장소의 Commits에서 원하는 되돌리고 싶은 커밋의 해시 값을 간편하게 찾을 수 있다.

  <img width="1208" alt="깃허브 로그 화면캡쳐" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/bca7d830-9a6c-40eb-9366-542d006eacf3">

## `git reset` 후 pull 에러 해결

---

`git reset`이 잘 실행되어 이전으로 되돌아간 것을 확인한 후, push를 하려고 했더니 에러가 발생했다..  
push 하기 전에 pull 하라고 해서 했지만 그래도 응... 안됐다. 그래서 찾은 방법. **강제로 push** 해버리기!

### 강제로 push 하기

---

`git push -f origin <branch>`
push 명령어를 실행하지 못해도 강제로 원격 저장소에 push 한다.

> 참고  
> [Git에서 특정 커밋 시점으로 되돌리기](https://programforlife.tistory.com/13)  
> [Git 커밋 취소(reset), 커밋 되돌리기(revert), 덮어쓰기(amend)](https://www.lainyzine.com/ko/article/git-reset-and-git-revert-and-git-commit-amend/)
