---
layout: post
categories:
  - TIL
title: 'Git: 한 로컬 파일에 여러개 원격 저장소 연결하기'
tags:
  - TIL
  - Git
---

프로젝트를 진행하면서 나의 깃허브에 잔디가 심어지지 않고 있다는 것을 발견했다.  
프로젝트 저장소는 Organization을 만들어서 해당 조직에 레포를 두고 있다. main 브랜치에 커밋하는 것은 나의 깃허브에 잔디가 심어지지만, 따른 브랜치 (나의 경우 soha 브랜치)에 커밋할 경우에는 잔디가 심어지지 않았다.

따라서, 잔디를 심어주기 위해서 해당 로컬 저장소에 두개의 원격 저장소를 연결하기로 했다!

1. **개인 깃허브에 새로운 repository 만들기**

깃허브 페이지에서 새로운 레포지토리를 만들어주기만 하면 된다.

_해당 순서를 진행하기 전에 우리는 기존에 a 로컬 저장소에 b 원격 저장소가 연결되어있다는 가정하에 해당 방법을 진행한다. 만약, 연결되어 있지 않다면, 일단 a 로컬 저장소에 b 원격 저장소를 연결하고 해당 방법을 진행하자._

2. **로컬 저장소에 새로운 원격 저장소 연결하기**

```bash
git remote add <단축이름> git@github.com:<깃허브 아이디>/<1에서 만든 레포지토리 이름>.git

# 예시
git remote add upstream git@github.com:soi-ha/Hello.git
```

원격 저장소를 연결해줬다면, 두개의 원격 저장소가 연결되었는지 확인한다.

```bash
git remote -v
```

<img width="631" alt="git remote를 실행한 화면" src="https://github.com/soi-ha/UNIVEUS/assets/77609591/5a5a361a-cc74-4f48-9551-a09f3cd466bc">

위와 같이 총 2개(4줄)가 떴다면 잘 연결된 것이다.

3. **새로운 레포지토리에 내용 넣어주기**

다른 분들을 보니 git clone을 사용해서 내용을 넣어주셨지만, 나는 다른 방법을 사용했다.

로컬 저장소에 간단한 변경사항(그냥 주석 추가 정도)을 준 뒤 변경 내용을 원격 저장소로 push 해주었다.

```bash
git status
git add .
git commit -m "커밋 내용"

# 기존 원격저장소
git push <단축이름> <브랜치명>
# ex) git push origin soha

# 추가 연결한 원격저장소
git push <단축이름> <브랜치명>
# ex) git push upstream soha
```

원격 저장소로 push 할 때, 두 저장소 모두 push 명령어를 작성해줘야 한다. 하나만 push 하면, 해당 저장소에만 push 된다.  
한 명령어로 두 원격저장소로 push가 가능하지만 (원격 저장소를 추가 연결할 때 upstream이 아닌 origin으로 이름을 하면 된다. 즉, 단축이름을 동일하게 설정해주면 된다는 뜻이다.) 추후에 헷갈릴 것 같아서 단축 이름을 다르게 해줬다.

그리고 push 할 때, 새로 연결한 원격 저장소의 경우에는 브랜치 명이 따로 저장된 것이 없어서 (기본 이름이 main/master 인 것일 뿐, 첫 push 이전까지는 브랜치로 만들어진 것은 아님) 첫 push할 때 명시해준 브랜치 명이 메인 브랜치가 된다.  
위의 예시에서 나는 `git push upstream soha`를 해줬는데, 메인 브랜치의 이름이 soha가 되었다.

4. **결과**

이전에 organization의 soha 브랜치에 push한 내용이 잔디가 안 심어졌는데, 해당 연결 이후에 이전에 push 했던 내용들 모두 잔디가 심어졌다.

<img width="102" alt="깃허브 잔디" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/e31d32f7-cab2-4d12-b431-6a1392ef971f">

> 참고  
> [여러 개의 Git Repository로 소스코드 Push하기](https://twofootdog.tistory.com/42)  
> [git 여러개 원격 저장소 사용해서 프로젝트 한개에 연결하기](https://velog.io/@adguy/git-%EC%97%AC%EB%9F%AC%EA%B0%9C-%EC%9B%90%EA%B2%A9-%EC%A0%80%EC%9E%A5%EC%86%8C-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%84%9C-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%95%9C%EA%B0%9C%EC%97%90-%EC%97%B0%EA%B2%B0%ED%95%98%EA%B8%B0)
