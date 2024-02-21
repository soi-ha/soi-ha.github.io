---
layout: post
published: true
categories:
  - TIL
title: "nvm use 사용: Your user's .npmrc file (${HOME}/.npmrc)..."
tags:
  - TIL
  - Error
  - Wooteco
---

우테코 교육이 시작되고, 온보딩 미션을 진행하던 중 node 버전을 변경해주기 위해서 `nvm use <버전>`을 실행했으나 실패했다.

## nvm 버전을 변경하려는데 실패..?

```bash
Your user's .npmrc file (${HOME}/.npmrc)
has a `globalconfig` and/or a `prefix` setting, which are incompatible with nvm.
Run `nvm use --delete-prefix v18.27.1` to unset it.
```

해당 메세지는 nvm을 사용하여 node 버전을 관리하는데 .npmrc 파일에서 globalconfig 또는 prefix 설정이 발견되었다는 것이다. globalconfig와 prefix는 nvm과 호환되지 않는다.

### 왜 호환되지 않는 걸까?

nvm과 prefix 및 globalconfig는 서로 다른 방식으로 node 버전을 관리한다.

- **prefix**  
  prefix는 특정 프로젝트에 대한 node 실행 파일과 패키지를 설치하는 위치를 가리킨다.  
  그러나, nvm은 설치된 **node의 버전을 관리**하기 위해 사용된다.  
  즉, prefix는 특정 프로젝트나 환경과 관련이 있어 nvm과는 별도로 작동된다.

- **globalconfig**  
  globalconfig는 node의 설정 파일을 가리킨다.
  nvm은 node **실행 파일 자체를 관리**하는 데 중점을 둔다.
  따라서 node의 실행 파일이나 버전을 변경할 때 globalconfig는 영향을 받지 않는다.

nvm은 시스템 전체의 **node 버전을 관리**하는 데 중점을 둔다.
globalconfig와 prefix 설정은 npm이 전역적으로 설치된 패키지의 위치나 구성 파일의 경로를 지정하는 역할을 한다.  
그렇기에 서로의 역할이 달라 prefix와 globalconfig는 nvm과 호환되지 않는다고 말할 수 있다.

### .npmrc?

.npmrc 파일은 npm에서 설정을 저장해두는 파일로, npm의 구성 옵션들이 포함되어 있다.

### 해결 방법

`nvm use -delete-presix v18.17.1`는 특정 버전(v18.17.1)의 node를 사용할 때 .npmrc 파일에서 prefix 설정을 삭제하는 명령어이다. 이렇게 하면 해당 버전의 node와 npm이 올바르게 작동된다.

```bash
> nvm use <버전>

Your user's .npmrc file (${HOME}/.npmrc)
has a `globalconfig` and/or a `prefix` setting, which are incompatible with nvm.
Run `nvm use --delete-prefix v18.27.1` to unset it.

> nvm use --delete-prefix <버전>

Now using node <버전>
```

<img width="581" alt="nvm prefix 설정 삭제 스크린샷" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/35df2b2c-0c7d-4127-bb93-d5b9243afff2">
