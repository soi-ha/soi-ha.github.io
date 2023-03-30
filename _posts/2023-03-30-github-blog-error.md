---
layout: post
categories:
  - TIL
title: "github blog 포스팅 에러 해결"
tags:
  - TIL
  - Error
---

분명 28일까지만 해도 잘 포스팅이 되던 블로그가.. 갑자기..! 포스팅이 안됐다 ㅜ..

그간 알고 있던 해결법은 추측하기로는 github blog의 서버 시간이 미국 시간인 것 같아서 새벽에 포스팅을 하면 항상 일자를 전날로 했었다. 그러면 에러없이 잘 올라갔었다.

그런데...? 이렇게 날짜를 바꿔도 업로드가 안되는 것이다..! 오타가 났는지.. 파일 위치를 잘 못 했는지.. 아무리 봐도 틀린점이 없었다.

그래서 서칭 끝에 해결한 방법

- \_config.yml 파일에 `futrue: true` 추가하기!

나는 이 방법을 통해서 무사히 해결 할 수 있었다..

**github blog 업로드가 안 될 때**

- 날짜를 하루 전날로 변경한다.
- \_config.yml에 `futrue: true`를 추가한다.

> 참고!  
> [참고한 페이지 링크](https://devyuseon.github.io//github%20blog/githubblog-post-not-shown/)
