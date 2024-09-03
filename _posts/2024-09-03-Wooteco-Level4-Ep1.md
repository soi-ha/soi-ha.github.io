---
layout: post
published: true
categories:
  - TIL
title: 'fatal: the remote end hung up unexpectedly'
tags:
  - Wooteco
  - Error
  - Git
---

## npm run deploy 실행시 에러 발생!

우테코 Level4 개인 미션 시작 전, 사전 작업인 api key 발급과 `.env` 설정, node 버전을 모두 올바르게 맞췄음에도 `npm run deploy`를 실행하면 다음과 같은 에러가 발생했다.

<img width="713" alt="에러 발생 터미널" src="https://github.com/user-attachments/assets/b804385f-6094-4feb-a78c-cbccd9ee3016">

## `npm run deploy`?

`npm run deploy`는 `npm run build:prod && npx gh-pages -d dist`를 실행한다. build는 정상적으로 작동하지만 `npx gh-pages -d dist` 명령어를 실행하면 에러가 발생했다.

`npx gh-pages -d dist` 명령어는 `gh-pages` 패키지를 사용하여 GitHub Pages에 프로젝트를 배포한다. 이때, `-d dist` 옵션은 배포할 디렉토리를 dist로 지정하는 것이다.

> `gh-pages` 패키지?  
> `gh-pages`는 GitHub Pages에 웹사이트를 배포하는 데 도움을 주는 도구이다.

> GitHub Pages?  
> GitHub에서 호스팅되는 웹사이트를 쉽게 배포할 수 있는 서비스이다.

## 에러 원인 발견 및 해결!

즉, **gh-pages 브랜치를 push 할 때 문제가 발생하는 것**이다.

난생 처음 보는 문제로… gpt에게 물어봐도 이상한 소리만 답으로 줬다..ㅎ
그래서 나는 구글링을 했다.

[이 글](https://2bdbest-ds.tistory.com/entry/git-fatal-오류-해결-the-remote-end-hung-up-unexpectedly-Everything-up-to-date) 에서 도움을 얻었다.

자, 에러를 잘 뜯어보자 RPC 뭐시기 뭔지는 모르겠지만 send-pack에서 뭔 sideband packet 뭐시기가 뜬다. 이것도 뭔 소리인지 모르겠다. 그렇다면 그 밑을 보자.

`fatal: the remote end hung up unexpectedly`는 “원격 끝에서 예기치 않게 연결이 종료되었습니다”라는 말이다.

해당 에러가 발생하는 이유는 post buffer의 사이즈 문제라고 한다.  
git에서 기본적으로 1개의 파일의 최대 용량이 1MB로 설정되어 있다. 그런데 이 사이즈를 초과한 파일을 push하려고 할 때 발생하는 에러였다. 때문에 git의 post buffer (즉, push === post) 사이즈를 늘려줬다. 그러니 해결됐다!!

### 문제 해결 명령어

```bash
git config http.postBuffer 1048576000
```

아, 참고로 기본 값은 1048576이다.

### 무사히 deploy가 실행된 터미널!

<img width="557" alt="정상적으로 deploy가 된 터미널" src="https://github.com/user-attachments/assets/699103c3-89ca-4158-a6b8-b890e66ee6ae">

> 참고 자료  
> [git fatal 오류 해결: the remote end hung up unexpectedly Everything up-to-date](https://2bdbest-ds.tistory.com/entry/git-fatal-오류-해결-the-remote-end-hung-up-unexpectedly-Everything-up-to-date)  
> [Git, fatal: The remote end hung up unexpectedly](https://stackoverflow.com/questions/15240815/git-fatal-the-remote-end-hung-up-unexpectedly)  
> [git fatal: the remote end hung up unexpectedly 오류 조치](https://happy-jjang-a.tistory.com/222#google_vignette)
