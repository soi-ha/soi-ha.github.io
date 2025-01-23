---
layout: post
published: true
categories:
  - TIL
title: '매3개 | Git 저장소의 구조와 흐름'
tags:
  - Git
  - TIL
  - 매3개
---

## 🌿 Git?

Git은 분산 버전 관리 시스템으로, 개발자가 코드를 효율적으로 관리하고 협업할 수 있도록 돕는다. 이런 Git 저장소는 총 4가지로 작업 디렉터리, 스테이징 영역, 지역 저장소, 원격 저장소가 있다.

이 중에서 주요 영역은 작업 디렉터리, 스테이징 영역, 지역 저장소 3가지로 Git의 데이터 흐름을 파악하기 위해 꼭 알아야 한다. 이 3가지 영역은 Git의 주요 데이터 관리 구조로 Git이 추적(관리)하는 파일과 추적하지 않는 파일을 구분하고, 추적하는 파일들의 상태를 구분짓는다.

일단 Git의 데이터 흐름을 알아보기 전에 Git의 저장소 4가지가 어떤 역할을 하는지 알아보자.

## 🗄️ Git 저장소의 구조

### 1. 작업 디렉터리(Working Directory)

작업 디렉터리(Working Directory)는 작업 중인 파일들이 실제로 있는 공간으로, **사용자가 직접 수정하고 편집하는 파일이 위치**한다.

`git init` 명령어를 통해 현재 디렉터리(컴퓨터에서 작업 중인 폴더)를 Git 저장소로 초기화한다. 초기화가 완료되면 Git은 변경된 파일을 추적하기 시작한다.

작업 디렉터리는 Working Tree라고도 부르는데, 이 두 가지 용어는 동일한 의미로 사용된다. Git 공식 문서에서도 두 용어가 같은 의미로 사용되며 혼용된다.

<img width="641" alt="working tree Git 공식문서 설명" src="https://github.com/user-attachments/assets/21e9781e-d541-498e-a9c4-41dfc9f93a44" />

_working tree Git 공식문서 설명_

이는 Git의 상태를 트래킹하고 파일 시스템의 특정 상태를 나타내는데 있어서 같은 역할을 하기 때문이다. 따라서, 작업 디렉터리와 작업 트리 모두 "사용자가 직접 편집하는 파일들이 위치한 영역"을 지칭한다.

<img width="250" alt="Image" src="https://github.com/user-attachments/assets/3079a540-d67b-49ee-92b9-06ff3290ada0" />

_작업 내용이 없을 경우 `git status` 명령을 했을 때 띄워지는 문구_

### 2. 스테이징 영역(Staging Area)

스테이징 영역은 커밋(commit)하기 전에 변경된 파일을 **임시로 저장하는 공간**이다.

`git add` 명령어를 사용하여 작업 디렉터리에서 변경된 파일을 스테이징 영역으로 추가할 수 있다. 이를 통해 커밋 시 어떤 변경 사항이 포함될지 선택적으로 관리할 수 있다.

### 3. 지역 저장소(Local Repository)

지역 저장소는 사용자의 컴퓨터에 위치하며, **모든 버전 히스토리를 저장**한다.

`git commit` 명령어를 통해 스테이징 영역에 있는 변경 사항을 지역 저장소에 기록한다. 지역 저장소는 `.git` 디렉터리로 관리되며, 버전 기록을 안전하게 보관한다.

### 4. 원격 저장소(Remote Repository)

원격 저장소는 GitHub, GitLab과 같은 플랫폼에서 호스팅되며, 협업을 위해 사용된다.

`git push` 명령어를 사용하여 **지역 저장소의 데이터를 원격 저장소로 업로드**할 수 있다. 반대로, 원격 저장소에 있는 최신 데이터를 지역 저장소로 가져오려면 `git pull` 명령어를 사용한다. 새로운 저장소를 처음 복제할 때는 `git clone` 명령어를 사용하여 원격 저장소의 내용을 지역 저장소로 복사한다.

#### 쉽게 기억하기

- 작업 디렉토리: 내가 실제로 일하는 공간.
- 스테이징 영역: 커밋하기 전에 임시로 저장하는 곳.
- 지역 저장소: 내 작업을 안전하게 저장하는 곳.
- 원격 저장소: 협업 및 백업을 하기 위해 작업을 저장하는 곳.

## 🌊 Git의 데이터 흐름

Git 저장소에 대해 설명하면서 Git의 데이터 흐름에 대해서도 설명을 했다. 그러나 이렇게 글로 설명해서는 잘 이해되지 않을 것이다. 아래 그림을 보면서 간략하게 Git의 데이터 흐름에 대해서 다시 한번 살펴보자.

<img width="1392" alt="git 저장소의 구조와 흐름" src="https://github.com/user-attachments/assets/d51a1640-bbde-4cb7-ab3d-4b4a3d66a759" />

1. `git init`을 통해 현재 디렉토리를 Git 저장소로 초기화하고 변경된 파일을 추적한다.
2. 작업 디렉터리에서 코드를 수정하고 변경 사항을 생성한다.
3. `git add` 명령어로 변경 사항을 스테이징 영역에 추가한다.
4. `git commit` 명령어로 스테이징 영역의 변경 사항을 지역 저장소에 저장한다.
5. `git push` 명령어를 사용해 지역 저장소의 변경 사항을 원격 저장소로 전송한다.
6. 다른 작업자의 변경 사항을 통합하려면 `git pull` 명령어를 사용한다.

+) 만약 새로운 저장소를 작업 중인 컴퓨터로 복제하고 싶은 경우, `git clone` 명령어를 사용하여 원격 저장소의 내용을 지역 저장소로 복사한다.

Git의 저장소 구조와 데이터 흐름을 이해했다면, 이제 작업을 진행하며 파일의 상태가 어떻게 변화하는지 알아볼 차례다. 파일의 상태 변화는 Git이 파일을 관리하는 과정을 이해하는 핵심으로, 효율적인 버전 관리를 위해 반드시 알아야 할 개념이다.

## 🕊️ Git에서의 파일 상태 변화

Git에서 파일의 상태 변화는 파일이 Git에서 어떻게 관리되고 있는지를 보여주는 중요한 요소다. 이 과정은 파일이 작업 디렉터리(Working Directory)와 스테이징 영역(Staging Area) 사이를 이동하며 상태가 변화하는 방식으로 나타난다.

아래 그림을 참고하여 어떻게 파일 상태 변화가 나타나는지 알아보자.

<img width="1312" alt="git의 파일 상태 변화" src="https://github.com/user-attachments/assets/fe95cc25-5171-4548-a9ce-8924aa7a350d" />

### 1. Untracked 상태

작업 디렉터리에 새로 추가된 파일은 처음에는 Git에 의해 추적되지 않는 상태인 **Untracked**로 분류된다. 이 상태는 Git이 파일을 아직 버전 관리에 포함하지 않았음을 의미한다.

<img width="483" alt="untracked file" src="https://github.com/user-attachments/assets/cd0296db-ae7c-4b6b-b3d4-70ed2c5a0e71" />

- **→ `git add` 명령어**

  `git add` 명령어를 실행하면 Untracked 상태의 파일이 스테이징 영역에 추가되고, **Tracked 상태**로 전환된다.

### 2. Tracked 상태

Tracked 상태는 Git이 해당 파일을 버전 관리하고 있음을 의미한다.

<img width="389" alt="tracked file" src="https://github.com/user-attachments/assets/2d396235-5f26-4182-acfb-464b1a2f3fe1" />

Tracked 상태의 파일은 아래 세 가지 하위 상태로 나뉜다.

- **Unmodified**: 마지막 커밋 이후 파일에 변경이 없는 상태. `git add`를 실행하지 않은 상태의 파일이 여기에 해당.
- **Modified**: 파일이 수정되었으나 아직 스테이징 영역에 반영되지 않은 상태.
- **Staged**: 수정된 파일이 `git add` 명령어로 스테이징 영역에 추가된 상태.

### 3. Unmodified → Modified

Tracked 상태의 파일이 수정되면, 파일은 **Modified 상태**로 변경된다. 이 상태는 파일이 변경되었으나 아직 스테이징 영역에 반영되지 않았음을 나타낸다.

<img width="456" alt="modified file" src="https://github.com/user-attachments/assets/ed1eb917-c86f-4b70-9fd7-3058383d9ace" />

### 4. → `git add` 명령어

수정된 파일에 대해 다시 `git add` 명령어를 실행하면, 파일은 다시 **Staged 상태**로 전환된다. Staged 상태의 파일은 이후 커밋(commit) 시 기록된다.

## 📝 정리

- **작업 디렉터리 (Working Directory)**: 실제로 파일을 수정/편집하는 공간
- **스테이징 영역 (Staging Area)**: 커밋 전 변경 사항을 임시 저장하는 공간
- **지역 저장소 (Local Repository)**: 모든 버전 히스토리를 저장하는 공간
- **원격 저장소 (Remote Repository)**: 협업 및 백업용 저장 공간 (GitHub, GitLab 등)
- **Git의 핵심 흐름**: 작업 디렉터리 → 스테이징 영역 → 지역 저장소 → 원격 저장소
- **Untracked**: Git이 추적하지 않는 새 파일
- **Tracked 상태**
  - **Unmodified**: 마지막 커밋 이후 변경 없음
  - **Modified**: 파일 수정됨
  - **Staged**: 수정 파일이 `git add`로 스테이징 영역에 반영
