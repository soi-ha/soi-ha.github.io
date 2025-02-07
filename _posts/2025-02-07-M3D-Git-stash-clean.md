---
layout: post
published: true
categories:
  - TIL
title: '매3개 | Git stash와 clean'
tags:
  - Git
  - TIL
  - 매3개
---

## 👛 Git stash

`git stash`는 현재 작업 중인 변경 사항을 **임시로 저장**하는 명령어다. 작업 도중 코드 변경 사항이 있지만 바로 커밋할 수 없는 상황에서 유용하게 사용할 수 있다.

### 언제 사용할까?

- **다른 브랜치로 이동해야 할 때:** 작업 도중 브랜치를 변경해야 하지만 아직 변경 사항을 커밋할 준비가 안 된 경우 stash를 사용한다.
- **작업 중 코드 백업:** 임시로 코드 변경 사항을 저장하고, 나중에 복원하여 이어서 작업할 때 사용한다.
- **실험적인 코드 작성:** 특정 코드 시도를 stash에 저장해두고 이후 필요에 따라 되돌리거나 삭제할 수 있다.

### 사용 방법

#### 변경 사항 임시 저장

변경 사항이 모두 stash에 저장되고 워킹 디렉터리가 깨끗한 상태가 된다.

```bash
git stash
```

아래는 `git stash`와 같이 사용할 수 있는 다양한 옵션들이다.

- `--keep-index` 옵션  
  스테이징된(인덱스된) 변경 사항은 유지하고 워킹 디렉터리 변경만 stash에 저장한다.  
  기본적으로 `git stash`는 스테이징된 변경 사항도 stash에 포함하지만, 이 옵션은 스테이징된 변경 사항을 보존하고 워킹 디렉터리 변경만 저장하는 특징이 있다.

- `--all`옵션  
  워킹 디렉터리의 모든 변경 사항(추적된 파일 + 추적되지 않은 파일)을 stash에 저장한다.  
  기본적으로 `git stash`는 추적된 파일만 저장하므로, 추가적으로 `.gitignore`에 포함된 파일이나 추적되지 않은 파일까지 임시 저장하려면 `--all` 옵션이 필요하다.

- `--include-untracked` or `-u` 옵션  
  추적 중이지 않은 파일을 같이 저장한다.  
  기본적으로 `git stash`는 추적 중인 파일만 저장한다.

- `--patch` 옵션  
  수정된 모든 사항을 저장하지 않는다. 대신 대화형 프롬프트가 뜨며 변경된 데이터 중 저장할 것과 저장하지 않을 것을 지정할 수 있다.

  ```bash
  $ git stash --patch
  diff --git a/lib/simplegit.rb b/lib/simplegit.rb
  index 66d332e..8bb5674 100644
  --- a/lib/simplegit.rb
  +++ b/lib/simplegit.rb
  @@ -16,6 +16,10 @@ class SimpleGit
          return `#{git_cmd} 2>&1`.chomp
        end
      end
  +
  +    def show(treeish = 'master')
  +      command("git show #{treeish}")
  +    end

  end
  test
  Stash this hunk [y,n,q,a,d,/,e,?]? y

  Saved working directory and index state WIP on master: 1b65b17 added the index file
  ```

#### 특정 메시지를 추가해 저장

저장된 stash 목록에 설명이 추가되어 구분하기 쉽다.

```bash
git stash save "작업 내용 설명 메시지"
```

#### 저장된 stash 목록 확인

각 stash에 대한 식별 정보가 출력된다.

```bash
git stash list
```

#### stash 복원

가장 최근 stash를 워킹 디렉터리에 적용한다.

```bash
git stash apply
```

- `--index` 옵션  
  Staged 상태까지 적용한다.  
  기본적으로 stash를 적용할 때 Staged 상태였던 파일을 자동으로 다시 Staged 상태로 만들어 주지 않는다. 따라서 해당 옵션을 사용해야 원래 작업하던 상태로 돌아올 수 있다.

#### 특정 stash 적용

stash 목록에서 특정 인덱스(`stash@{번호}`)에 해당하는 항목을 적용한다.

```bash
git stash apply stash@{2}
```

#### stash 삭제

특정 stash 항목을 삭제한다.  
`apply` 옵션은 단순히 stash를 적용하는 것뿐이다. stash는 여전히 목록에 남아 있다.` git stash drop` 명령을 사용하여 해당 stash를 제거한다.

```bash
git stash drop stash@{0}
```

#### stash 복원 후 제거

stash를 적용한 후 해당 stash를 목록에서 제거한다.

```bash
git stash pop
```

#### 모든 stash 삭제

모든 stash 항목을 삭제한다.

```bash
git stash clear
```

## 🧹 Git clean

`git clean`은 Git 저장소에서 **추적되지 않은 파일(Untracked files)을 삭제**하는 명령어다. 추적되지 않은 파일이란 Git이 관리하지 않는 파일(예: 새로 생성한 파일 또는 빌드 결과물)을 의미한다.

### 언제 사용할까?

- **빌드 파일 제거:** 빌드 과정에서 생성된 파일을 깔끔하게 제거할 때 사용한다.
- **불필요한 파일 정리:** 프로젝트에 더 이상 필요하지 않은 임시 파일이나 테스트 파일을 제거할 때 유용하다.
- **코드 환경 초기화:** 작업 환경을 깔끔하게 초기화하여 불필요한 파일이 문제를 일으키지 않도록 할 때 사용한다.

### 사용하는 방법

#### 추적되지 않은 파일 삭제

untracked 파일을 삭제한다.

```bash
git clean -f
```

`-f(force)` 옵션은 파일 삭제를 강제하는 옵션이다.

기본적으로 `git clean`은 `-f` 옵션 없이 동작하지 않기 때문에 실제로 사용하는 경우가 없다. Git은 실수로 파일을 삭제하는 것을 방지하기 위해 이 명령어를 반드시 강제 옵션과 함께 사용하도록 설계했다.

#### 디렉터리까지 삭제

디렉터리와 파일 모두 삭제한다. 해당 명령 옵션은 하위 디렉토리까지 모두 지워버린다.

```bash
git clean -f -d
```

`-d (directory)` 옵션은 디렉터리 삭제를 허용한다. 이 옵션이 없으면 파일만 삭제되며 디렉터리는 남는다.

#### 삭제 대상 확인

삭제될 파일 목록만 미리 확인할 수 있다.

```bash
git clean -n
```

`-n(dry run)` 옵션은 실제로 파일을 삭제하지 않고 어떤 파일이 삭제될지 미리 보여준다.

#### 강제 삭제

`.gitignore`에 포함된 파일과 디렉터리도 삭제한다.

```bash
git clean -f -d -x
```

`-x(exclude .gitignore)` 옵션은 `.gitignore`에 포함된 파일도 강제로 삭제한다.

#### 특정 경로만 삭제

지정된 경로(예: `build` 폴더)만 정리한다.

```bash
git clean -f ./build
```

## 🍽️ stash와 clean 비교 정리

| 구분          | git stash                       | git clean                          |
| ------------- | ------------------------------- | ---------------------------------- |
| **목적**      | 변경 사항 임시 저장             | 추적되지 않은 파일 삭제            |
| **작업 대상** | Git이 추적하는 변경 파일        | Git이 추적하지 않는 파일           |
| **명령어**    | `git stash`, `git stash pop` 등 | `git clean -f`, `git clean -df` 등 |
| **사용 상황** | 브랜치 이동 또는 코드 백업      | 불필요한 파일 제거 및 환경 초기화  |
| **영향**      | 변경 사항이 보존됨              | 삭제된 파일은 복구 불가            |

## 📝 정리

- stash는 변경 사항을 보존하며 나중에 복원.
- clean은 추적되지 않은 파일을 깔끔하게 삭제하여 작업 환경을 정리하는데 적합.

## 📚 참고

- [Git 도구 - Stashing과 Cleaning](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Stashing%EA%B3%BC-Cleaning)
