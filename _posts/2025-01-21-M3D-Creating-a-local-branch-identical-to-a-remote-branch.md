---
layout: post
published: true
categories:
  - TIL
title: '매3개 | Git: checkout, switch, restore (부제: 원격 브랜치를 로컬에 그대로 생성하기)'
tags:
  - Git
  - TIL
  - 매3개
---

때는 행동대장 프로젝트를 진행하면서...  
새로운 이슈를 처리하기 위해 원격에서 만들어둔 브랜치를 로컬로 가져오려고 시도했다. 그러나 만들어진 브랜치의 마지막 log를 확인해보니 원격 브랜치의 log와 달랐다! 나는 원격 브랜치를 그대로 로컬로 가져오는 방법을 검색했고.. git의 checkout을 통해 성공할 수 있었다.

도대체 이 checkout이 무엇을 수행하는 명령어이길래 원격 브랜치를 그대로 로컬에 생성할 수 있는지 알아보도록 하자. 그리고 checkout과 유사한 일을 수행하는 switch와 restore은 무엇인지도 같이 알아보자!

## ✅ checkout?

checkout 명령어는 브랜치 변경, 특정 커밋 체크아웃, 파일 복원 등 여러 작업에 사용된다.

### checkout으로 할 수 있는 기능

#### 1. 브랜치 변경

현재 작업 중인 브랜치를 다른 브랜치로 변경하는 데 사용된다.

```bash
git checkout <branch-name>
```

- 현재 브랜치를 `branch-name`으로 변경한다.
- 작업 디렉토리는 변경된 브랜치의 마지막 commit 상태로 업데이트된다.
- 단, commit 혹은 stash하지 않은 작업이 존재할 경우(즉, working directory에 존재) 다른 브랜치로 변경할 수 없다.

#### 2. 새 브랜치 생성 및 이동

브랜치를 생성하고 바로 이동할 때 사용한다.

```bash
git checkout -b <new-branch-name>
```

- 새로운 브랜치를 생성하고 해당 브랜치로 이동한다.
- 만약 **원격 브랜치를 추적하는 로컬 브랜치를 생성하고 이동**하고 싶을 경우 다음과 같다.

  ```bash
  git checkout -b <new-branch-name> <remote>/<remote-branch-name>

  or

  git checkout --track <remote>/<remote-branch-name>
  ```

  예로, origin에 있는 feature1이라는 브랜치를 로컬에 바로 생성하고 이동하고 싶다면 다음과 같다.

  ```bash
  git checkout -b feature1 origin/feature1

  or

  git checkout --track origin/feature1
  ```

  origin의 브랜치 이름과 다른 이름을 사용할 수 있다. feature1이 아니라 feat1로 만들고 싶다면

  ```bash
  git checkout -b feat1 origin/feature1
  ```

#### 3. 특정 커밋 체크아웃

특정 커밋으로 working directory를 업데이트한다.

```bash
git checkout <commit-hash>
```

- `commit-hash`로 지정된 커밋의 파일들을 작업 디렉토리에 반영한다.
- 이 상태는 "detached HEAD" 상태라고 불리며, 이후 작업은 브랜치가 아닌 **해당 커밋을 기준**으로 진행된다.

#### 4. 파일 복원

특정 파일을 이전 커밋의 상태로 복원한다.

```bash
git checkout <branch-name> -- <file-path>
```

- `<file-path>`에 있는 파일을 지정된 브랜치 또는 커밋의 상태로 되돌린다.
- 이 명령은 주로 잘못된 수정 내용을 되돌릴 때 유용하다.

### checkout의 기능이 분리?

Git 2.23 이후, `checkout`의 일부 기능이 **`switch`**(브랜치 변경)와 **`restore`**(파일 복원)로 분리되었다. 각각의 명령은 특정 작업에 초점이 맞춰져 있어 더 직관적이고 명확하게 사용할 수 있다. 그렇지만 `checkout`은 여전히 사용 가능하다.

하지만 기능이 분리되었기 때문에, 이제는 switch와 restore를 사용하는 것을 더 권장한다. (아무래도 checkout은 책임 분리가 덜 된 구조였기 때문인 듯 하다.)

그렇다면 `checkout`에서 사용하던 기능을 switch와 restore로 어떻게 대체할 수 있는지 알아보자!

## 🕹️ switch

브랜치를 이동하거나 새로 생성하는 작업에만 사용된다. `checkout`이 한꺼번에 처리하던 다양한 작업 중 브랜치와 관련된 작업만 분리한 명령이다.

### 주요 기능

#### 1. 브랜치 전환

현재 작업 중인 브랜치를 `<branch-name>`으로 변경한다.

```bash
git switch <branch-name>
```

#### 2. 새 브랜치 생성 및 이동

새 브랜치를 생성하고 해당 브랜치로 이동한다.

```bash
git switch -c <new-branch-name>
```

- 만약 **원격 브랜치를 추적하는 로컬 브랜치를 생성하고 이동**하고 싶을 경우 다음과 같다.

  ```bash
  git switch -c <new-branch-name> <remote>/<remote-branch-name>

  or

  git switch --track <remote>/<remote-branch-name>
  ```

#### 3. 강제 전환

작업 중인 변경 사항을 무시하고 강제로 브랜치를 변경한다.

```bash
git switch -f <branch-name>
```

## ♻️ restore

특정 파일을 이전 상태로 복원하는 작업에 초점을 맞춘 명령이다. 이는 `checkout`의 파일 복원 기능을 대체한다.

### 주요 기능

#### 1. 작업 디렉토리에서 파일 복원

스테이징된 변경 사항이나 working directory의 변경 사항을 마지막 커밋 상태로 되돌린다.

```bash
git restore <file-path>
```

#### 2. 스테이징 영역에서 복원

스테이징 영역(Staging Area)에 있는 파일을 언스테이징한다.

```bash
git restore --staged <file-path>
```

#### 3. 특정 커밋 기준으로 파일 복원

`<commit-hash>`에 해당하는 커밋 상태로 파일을 복원한다.

```bash
git restore --source=<commit-hash> <file-path>
```

## 🥊 checkout vs switch & restore

| **기능**                 | **checkout**                           | **switch**               | **restore**                          |
| ------------------------ | -------------------------------------- | ------------------------ | ------------------------------------ |
| 브랜치 변경              | O                                      | O                        | X                                    |
| 새 브랜치 생성 및 이동   | `git checkout -b <branch>`             | `git switch -c <branch>` | X                                    |
| 파일 복원                | `git checkout <branch> -- <file-path>` | X                        | `git restore <file-path>`            |
| 스테이징 영역 복원       | `git checkout -- <file-path>`          | X                        | `git restore --staged <file-path>`   |
| 특정 커밋 기반 파일 복원 | `git checkout <commit-hash> -- <file>` | X                        | `git restore --source=<commit-hash>` |

## 📝 정리

- **`checkout`**: 브랜치 변경, 특정 커밋 체크아웃, 파일 복원 등 여러 작업에 사용.
- **`switch`**: 브랜치를 전환하거나 생성하는 데만 사용.
- **`restore`**: 파일 복원 및 스테이징 관련 작업에 사용.
- `switch`와 `restore`는 Git 2.23 이상에서만 사용할 수 있다. 구버전에서는 `checkout`을 사용해야 한다.
- `switch`와 `restore`를 사용하면 작업의 목적과 의도가 명확히 드러나므로, 사용을 권장한다.

## 📚 참고

- [Git -git-checkout Documentation](https://git-scm.com/docs/git-checkout)
- [Git -git-switch Documentation](https://git-scm.com/docs/git-switch)
- [Git -git-restore Documentation](https://git-scm.com/docs/git-restore)
- [Git 체크아웃에 대한 설명: Git에서 브랜치를 체크아웃(변경 또는 전환)하는 방법](https://www.freecodecamp.org/korean/news/git-cekeuause-daehan-seolmyeong-giteseo-beuraencireul-cekeuaus-byeongyeong-ddoneun-jeonhwan-haneun-bangbeob/)
- [Git 깃: git checkout](https://zoosso.tistory.com/729)
