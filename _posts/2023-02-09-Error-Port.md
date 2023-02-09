---
layout: post
categories:
  - TIL
title: "Error: listen EADDRINUSE: address already in use..."
tags:
  - TIL
  - Vue.js
  - Port
  - Error
---
## __문제발생__
---
`npm run dev` 명령어를 실행하던 중 다음과 같은 에러가 발생했다.

<img width="520" alt="포트 에러 확인" src="https://user-images.githubusercontent.com/77609591/217744605-212453c7-9fba-43fe-8fa5-17d8cd4dc95e.png">


해당 에러는 찾아본 결과 해당하는 포트가 사용중이라 실행할 수 없다는 내용이었다.  
현재 내가 사용해야 하는 포트번호는 8080포트이다.  
그럼 즉, 해당 포트를 비워주면 서버를 실행할 수 있게 된다는 것이다.

## __문제 해결__
---

### __포트 PID 확인하기__

lsof 명령어를 사용하면 포트 목록들이 모두 나오게 된다. 그러니 옵션 -i를 통해 찾고자하는 포트 번호만을 검색한다.

```bash
sudo lsof -i :8080(찾고자하는 포트 번호)
```

<img width="503" alt="나의 포트 PID" src="https://user-images.githubusercontent.com/77609591/217743265-a2377f6b-7f73-4996-a1d7-4178fa9b7437.png">

나의 8080 포트는 PID는 71328이다.  
이제 해당 포트를 비울 수 있게 된다.

### __포트 비우기__

다음 명령어를 통해 포트를 비워준다.
```bash
sudo kill -9 71328(찾은 PID)
```

<img width="502" alt="포트 지우기" src="https://user-images.githubusercontent.com/77609591/217744046-56b56678-83ab-41db-81ba-11fa756d92ca.png">

포트를 비워준 후, 잘 비워졌는지 다시 확인해 본 후, 아무 반응도 없다면 잘 비워진 것이다.  

다시 서버를 실행하면 서버가 잘 돌아가는 것을 확인할 수 있다.

<img width="493" alt="서버 실행 완료" src="https://user-images.githubusercontent.com/77609591/217744398-b1d0a537-8ddb-468b-8f03-5508f60c851a.png">