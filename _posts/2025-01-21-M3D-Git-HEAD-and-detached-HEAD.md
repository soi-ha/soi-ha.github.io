---
layout: post
published: true
categories:
  - TIL
title: '매3개 | Git: HEAD와 detached HEAD'
tags:
  - Git
  - TIL
  - 매3개
---

## HEAD?

**HEAD**는 Git에서 현재 작업 중인 **브랜치를 가리키는 포인터**이다. 일반적으로 HEAD는 특정 브랜치(예: `main`, `develop`)를 가리키며, 해당 브랜치의 마지막 커밋을 기준으로 작업을 진행한다.

## detached HEAD?

**detached HEAD**는 HEAD가 브랜치를 가리키는 대신, **특정 커밋**을 직접 가리키는 상태를 말한다. 즉, **브랜치와 연결되지 않은 채** 특정 커밋에서 작업을 시작한다.

어떻게 detached HEAD 상태가 될 수 있는지 예시를 통해 알아보자.

### dtached HEAD 상태로 만들기

```bash
git checkout <commit-hash>
```

위 명령어를 실행하면 Git은 브랜치를 기준으로 작업하지 않고, `<commit-hash>`로 지정된 특정 커밋에서 작업을 시작한다. 이제 HEAD는 브랜치가 아닌 특정 커밋을 가리키게 된다.

그렇다면 detached HEAD 상태에서 작업을 하면 어떤 일이 일어나는 걸까?

### detached HEAD 상태에서 작업의 특징

#### 1. 임시 상태

새로운 커밋을 생성하면, 이 커밋은 **기존 브랜치와 연결되지 않는다.** 브랜치로 연결하지 않으면, 이후 Git 작업 중 **이 커밋을 잃어버릴 위험**이 있다.

#### 2. 기본 브랜치로 돌아가면 변경 사항이 사라질 수 있음

```bash
git checkout main
```

`detached HEAD` 상태에서 다른 브랜치로 이동하면, 이전에 작업한 내용이 그대로 버려질 수 있다.

```bash
git checkout <commit-hash>
echo "Changes" > file.txt
git add file.txt
git commit -m "detached commit"

git checkout main
```

만약 `detached HEAD` 상태에서 작업한 내용을 커밋하고 기본 브랜치로 돌아왔다면, 해당 커밋은 **보이지 않게** 될 수 있지만, **완전히 사라지지는 않는다.**

그렇다면 어떻게 `detached HEAD`에서 작업한 내용을 저장해둘 수 있을까?

### detached HEAD 상태에서 작업 보존

`detached HEAD` 상태에서 작업을 유지하려면 브랜치를 생성하거나 변경 사항을 명시적으로 저장해야 한다.

#### 방법 1: 새로운 브랜치 생성

```bash
git checkout -b <new-branch>
```

`detached HEAD` 상태에서 브랜치를 생성하면, 해당 브랜치에 현재 상태가 저장된다.

#### 방법 2: stash로 저장

```bash
git stash
```

작업 내용을 stash로 저장한 후, 다시 브랜치로 돌아가서 pop을 통해 복원할 수 있다.

이렇게 `detached HEAD`에 대해서 알아보았다. 그렇다면 이건 도대체 언제 사용하는 걸까?

### detached HEAD 상태가 필요한 경우

- 특정 커밋 기반으로 새로운 작업을 시작하고 싶을 때
- 과거 커밋의 상태를 확인하거나 임시로 작업하려 할 때
- 특정 태그나 커밋을 기반으로 빌드하거나 디버깅하려 할 때

위와 같은 경우에 `detached HEAD`를 사용하게 된다. 자, 이제 `detached HEAD`가 뭔지 알아보았으니 어떻게 사용하는지 예시를 통해 알아보자.

### 예시로 이해하기

1. **현재 브랜치 상태**

현재 HEAD는 `main` 브랜치를 가리킨다.

```bash
git checkout main
```

2. **detached HEAD 상태로 전환**

HEAD가 특정 커밋(`1234abcd`)을 가리키고, `main` 브랜치와의 연결이 끊긴다.

```bash
git checkout 1234abcd
```

3. **detached HEAD 상태에서 커밋 생성**

이 커밋은 브랜치와 연결되지 않아 나중에 잃어버릴 위험이 있다. 그러나 완전히 사라지는 것은 아니다.

```bash
echo "Test" > file.txt
git add file.txt
git commit -m "Detached HEAD commit"
```

4. **브랜치를 생성하여 커밋 보존**

커밋을 안전하게 새로운 브랜치에 저장할 수 있다.

```bash
git checkout -b <new-branch>
```

여기서 만약? `detached HEAD` 상태에서 작업한 커밋을 브랜치를 생성하여 바로 저장하지 않고 그냥 기존 브랜치(`main`)로 변경해버렸다면? 어떻게 다시 `detached HEAD` 상태에서 작업한 커밋을 찾아서 저장할 수 있을까?

### 잃어버린 커밋 찾고 저장하기

절차는 다음과 같다.

1. 잃어버린 커밋 해시 찾기
2. 해당 커밋 해시를 기반으로 새로운 브랜치 생성

끝이다! 2번 절차는 위에서 이미 알아봤으니 우리는 1번 절차만 어떻게 하는지 알면 된다.

#### 잃어버린 커밋 해시 찾기

```bash
git log

or

git reflog
```

`git log` 명령을 사용하여 최근 커밋들을 확인하거나, `git reflog`를 사용해 최근의 모든 작업을 확인할 수 있다.

- `git log`의 경우에는 HEAD되어 있는 브랜치에서 작업했던 내용들만 확인할 수 있다.
- `git reflog`는 최근에 작업했던 모든 내용들. 즉, 현재 HEAD된 브랜치가 무엇이던 간에 브랜치에 상관없이 작업했던 모든 내용(`checkout`하거나 `commit` 하거나 등)들과 hash 값을 확인할 수 있다.

이렇게 잃어버린 커밋의 hash 값을 찾았다면 이 것을 기반으로 새로운 브랜치를 생성하면 된다.

```bash
git switch -c <new-branch> <잃어버린 커밋의 hash>
```

## 정리

| 상태               | HEAD가 가리키는 위치   | 브랜치와의 연결 여부 |
| ------------------ | ---------------------- | -------------------- |
| 일반 브랜치 상태   | 특정 브랜치 (`main`)   | 연결됨               |
| detached HEAD 상태 | 특정 커밋 (`1234abcd`) | 연결되지 않음        |

## 참고

- [Git/번역: Detached HEAD](https://velog.io/@ss-won/Git-Detached-Head)
- [Git - Detached HEAD 당황하지 말자](https://castellan.tistory.com/78)
- [Detached Head](https://devcamus.tistory.com/6)
