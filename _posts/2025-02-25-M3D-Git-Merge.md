---
layout: post
published: true
categories:
  - TIL
title: '매3개 | Git의 다양한 Merge 방법 (merge, rebase, squash)'
tags:
  - Git
  - TIL
  - 매3개
---

github에서 pr을 생성하고 해당 pr들을 merge를 하려고 클릭하면 다양한 방식의 merge 방법이 존재하는 것을 볼 수 있다.

<img width="345" alt="깃허브 merge 선택 화면" src="https://github.com/user-attachments/assets/b99d46ee-0fda-43eb-8f08-0217892299f2" />

각 merge 방법들이 무엇인지 알아보도록 하자.

## 📋 목차

- [🫱🏻‍🫲🏼 Git Merge](#-git-merge-feat-fast-forward)
- [🔖 Git Rebase and Merge](#-git-rebase)
- [✊ Git Squash and Merge](#-git-squash-merge)

---

## 🫱🏻‍🫲🏼 Git Merge (feat. fast-forward)

`git merge`는 서로 다른 브랜치의 변경 사항을 하나의 브랜치에 통합하는 명령이다.  
이 방식은 두 브랜치의 히스토리를 모두 보존하며, 보통 새로운 merge 커밋을 생성한다.

단, 대상 브랜치(main)가 feature 브랜치의 기반(commit) 상태로 아무런 추가 변경 사항이 없을 경우에는 **`fast-forward merge`** 가 발생한다. fast-forward merge는 **별도의 merge 커밋 없이** **브랜치 포인터만 앞으로 이동**시켜 히스토리를 단순하게 유지한다.

### ✍🏻 커밋 기록

일반 Merge의 경우, feature 브랜치의 모든 커밋이 그대로 남고 새로운 merge 커밋이 추가된다.

반면, fast-forward Merge의 경우는 main 브랜치의 포인터만 이동되므로, merge 커밋이 생성되지 않고 feature 브랜치의 커밋들이 그대로 이어진다.

### 🪢 커밋 기록 예시

#### 일반 Merge

**Merge 전:**

```
A --- B --- C        (main)
      \
      D --- E --- F  (feature)
```

**Merge 후:**

```
A --- B --- C --- M   (main)
      \         /
      D --- E --- F  (feature)
```

여기서 M 커밋은 merge 커밋으로, feature 브랜치의 커밋(D, E, F)과 main의 커밋(C)을 모두 포함한다. 따라서 main 브랜치에는 **feature 브랜치의 각 커밋이 그대로 기록**되어 있다.

#### fast-forward Merge:

fast-forward merge가 가능할 경우에 옵션을 사용하여 원하는 형태로 merge가 가능하다.

**Merge 전:**

```
A --- B --- C   (main)
        \
          D --- E  (feature)
```

**Merge 후:**

- **fast-forward 병합 (`--no-ff` 옵션 미사용):**

  ```
  A --- B --- C --- D --- E   (main)
  ```

  위 경우, merge 커밋이 생성되지 않는다.

- **`--no-ff` 옵션 사용**:

  ```
  A --- B --- C --- M   (main)
                /
        D --- E  (feature)
  ```

  여기서 M 커밋은 feature 브랜치의 변경사항을 포함하는 merge 커밋으로, feature가 병합되었다는 사실을 명확하게 기록한다.

### 💬 명령어

#### 일반 Merge

```bash
git checkout main
git merge feature
```

위 명령어는 feature 브랜치의 변경 사항을 main 브랜치에 병합하며, fast-forward가 가능한 경우 별도의 merge 커밋 없이 포인터만 이동한다.

#### 강제 Merge (fast-forward 방지)

```bash
git checkout main
git merge --no-ff feature
```

위 명령어는 fast-forward가 가능한 상황에서도 merge 커밋을 강제로 생성하여 병합 기록을 명확하게 남긴다.

### 📝 실제 커밋 로그 예시

#### 일반 Merge (merge 커밋 생성)

```
*   3e1d2f0 (HEAD -> main) Merge branch 'feature'
|\
| * 9c8b7a6 (feature) Add feature part 3
| * 8b7a6c5 Add feature part 2
| * 7a6c5b4 Add feature part 1
* | 6d5c4b3 Fix typo in main file
* | 5c4b3a2 Update documentation
* | 4b3a291 Initial commit
```

#### fast-forward Merge (merge 커밋 없이 포인터 이동)

```
* 9c8b7a6 (HEAD -> main, feature) Add feature part 3
* 8b7a6c5 Add feature part 2
* 7a6c5b4 Add feature part 1
* 6d5c4b3 Fix typo in main file
* 5c4b3a2 Update documentation
* 4b3a291 Initial commit
```

---

## 🔖 Git Rebase

`git rebase`는 feature 브랜치의 커밋들을 대상 브랜치(main)의 최신 커밋 이후로 재배치하는 명령이다.  
이를 통해 커밋 히스토리가 선형으로 정리되어 깔끔하게 보인다. 다만, 이미 공유된 커밋에 대해 rebase를 수행하면 충돌이나 협업에 문제가 발생할 수 있으므로 주의하여 사용해야 한다.

### ✍🏻 커밋 기록

Rebase를 사용하면 feature 브랜치의 커밋들이 새로운 커밋(해시가 변경됨)으로 재작성되어 main 브랜치에 순서대로 기록된다. 이때 merge 커밋은 생성되지 않으며, 커밋 간의 관계가 단순한 선형 구조를 이룬다.

### 🪢 커밋 기록 예시

**Rebase 전:**

```
A --- B --- C        (main)
      \
      D --- E --- F  (feature)
```

**Rebase 후 (feature 브랜치가 main의 최신 커밋 뒤로 재배치됨):**

- feature 브랜치가 main 브랜치의 최신 커밋(C) 뒤로 재배치됨을 강조

  ```
  A --- B --- C        (main)
              \
              D' --- E' --- F'  (feature)
  ```

- feature의 구조

  ```
  A --- B --- C --- D' --- E' --- F'  (feature)
  ```

  feature 브랜치는 선형 구조를 가지게 된다.

이후 fast-forward 방식으로 main에 병합한다면, main 브랜치는 아래와 같이 된다.

```
A --- B --- C --- D' --- E' --- F'  (main)
```

feature 브랜치의 각 커밋(D', E', F')이 하나씩 순서대로 main 브랜치에 들어간다.

#### rebase and merge시, 충돌 주의

다른 개발자가 로컬에서 아직 rebase 이전의 feature 브랜치를 기반으로 작업하여 추가 커밋을 진행한 상태라고 가정해보자.

rebase된 feature 브랜치(D', E', F')와 기존 로컬 브랜치(D, E, F)의 히스토리 차이로 인해 병합 시 충돌이나 혼란이 발생할 수 있다. feature 브랜치의 커밋들이 rebase를 통해 해시가 변경된 새로운 커밋을 가지기 때문이다.

다른 개발자가 가진 rebase가 실행되지 않은 feature 브랜치에 새로운 커밋을 찍고 rebase된 브랜치(공유됨)에 merge를 실행할 경우 커밋의 해시 값이 다르기 때문에 충돌이 발생한다.

### 💬 명령어

- **Rebase 수행**

  ```bash
  git checkout feature
  git rebase main
  ```

  위 명령어는 feature 브랜치의 커밋들을 main 브랜치의 최신 커밋 뒤로 재배치한다.

- **충돌 해결 후 rebase 계속 진행**

  ```bash
  git rebase --continue
  ```

  만약 rebase 과정에서 충돌이 발생하면, 충돌을 해결한 후 위 명령어로 rebase를 이어간다.

- **Rebase 완료 후 main에 통합 (fast-forward)**

  ```bash
  git checkout main
  git merge feature
  ```

  rebase가 완료된 feature 브랜치는 main 브랜치에 fast-forward 방식으로 병합할 수 있다.

### 📝 실제 커밋 로그 예시

```
* 9c8b7a6 (HEAD -> main, feature) Add feature part 3 (rebased)
* 8b7a6c5 Add feature part 2 (rebased)
* 7a6c5b4 Add feature part 1 (rebased)
* 6d5c4b3 Fix typo in main file
* 5c4b3a2 Update documentation
* 4b3a291 Initial commit
```

---

## ✊ Git Squash Merge

`git squash merge`는 feature 브랜치의 여러 커밋들을 **하나의 커밋으로 압축**하여 대상 브랜치에 병합하는 방식이다.  
이 방법을 사용하면 기능 개발 과정의 중간 커밋들을 합쳐서 **깔끔한 커밋 이력**을 유지할 수 있다. 단, **개별 커밋의 세부 기록은 남지 않으므**로, 상세한 변경 이력을 확인하기 어렵다는 단점이 있다.

### ✍🏻 커밋 기록

Squash Merge를 수행하면, feature 브랜치의 여러 커밋이 하나의 커밋으로 압축되어 main 브랜치에 반영된다. 이 경우, feature 브랜치의 세부적인 커밋 내역은 사라지고, 하나의 통합 커밋으로 나타난다.

### 🪢 커밋 기록 예시

**Squash Merge 전:**

```
A --- B --- C         (main)
      \
      D --- E --- F   (feature)
```

**Squash Merge 후:**

```
A --- B --- C --- S   (main)
```

여기서 S 커밋은 feature 브랜치의 변경 사항(D, E, F)을 모두 합친 하나의 커밋이다. 따라서 main 브랜치에는 feature 브랜치의 개별 커밋들이 아닌, 하나의 압축된 커밋만 기록된다.

### 💬 명령어

```bash
git checkout main
git merge --squash feature
git commit -m "Squash merge: feature 브랜치의 변경 사항 통합"
```

위 명령어는 먼저 main 브랜치로 전환한 후 feature 브랜치의 모든 변경 사항을 하나로 합치고, 마지막으로 커밋 메시지를 작성하여 하나의 커밋으로 통합한다.

### 📝 실제 커밋 로그 예시

```
* abcdef0 (HEAD -> main) Squash merge: feature 브랜치의 변경 사항 통합
* 6d5c4b3 Fix typo in main file
* 5c4b3a2 Update documentation
* 4b3a291 Initial commit
```

---

위와 같이 Git Merge, Rebase, Squash Merge 방식은 각각의 목적과 상황에 따라 사용되며, 커밋 기록 방식에도 차이가 있다. 프로젝트의 관리 방식과 기록의 세부사항 유지 여부에 따라 적절한 방식을 선택하여 사용하면 된다.

## 📝 정리

| 병합 방식    | 설명                                                     | 장점                          | 단점                                 |
| ------------ | -------------------------------------------------------- | ----------------------------- | ------------------------------------ |
| git merge    | 두 브랜치의 변경사항을 병합하며 별도의 merge commit 생성 | 작업 내역이 명확하게 남음     | 히스토리가 복잡해질 수 있음          |
| rebase merge | 커밋들을 대상 브랜치 위에 재배치하여 선형 히스토리 유지  | 깔끔하고 추적이 쉬운 히스토리 | 공유된 커밋에 사용 시 혼란 발생 가능 |
| squash merge | 여러 커밋을 하나로 합쳐 이력을 단순하게 정리             | 커밋 이력이 간결해짐          | 세부 변경 내역 확인이 어려워짐       |

## 📚 참고

- [Merge vs. Rebase vs. Squash](https://blog.outsider.ne.kr/1704)
- [Git Rebase란?](https://velog.io/@kwonh/Git-Rebase%EB%9E%80)
- [3.6 Git 브랜치 - Rebase 하기](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)
- [3.2 Git 브랜치 - 브랜치와 Merge 의 기초](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EC%99%80-Merge-%EC%9D%98-%EA%B8%B0%EC%B4%88)
- [5.2 분산 환경에서의 Git - 프로젝트에 기여하기](https://git-scm.com/book/ko/v2/%EB%B6%84%EC%82%B0-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C%EC%9D%98-Git-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90-%EA%B8%B0%EC%97%AC%ED%95%98%EA%B8%B0)
