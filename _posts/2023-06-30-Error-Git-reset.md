---
layout: post
categories:
  - TIL
title: 'Git Error: "Already up to date"/강제로 리셋시키기'
tags:
  - TIL
  - Git
  - Error
---

이번에도 어김없이 프로젝트를 진행하며 pull을 하려던 중, 다음과 같은 에러(?)가 발생했다...
[저번에 작성했던 글]('https://soi-ha.github.io/til/2023/06/22/Error-Git-ffonly.html')의 방식대로 시도해봤으나 실패..^^..  
그래서 열심히 서칭한 끝에 찾아낸 방법은 다음과 같다.

### **git reset --hard [원격저장소]**

---

pull을 하려는 브랜치로 이동한 뒤, 다음과 같이 명령어를 작성한다.

```bash
git fetch --all
git reset --hard [원격저장소]
# git reset --hard origin/main
```

해당 명령어는, 모든 원격 저장소를 fetch한 후, --hard 옵션을 주어 원격저장소의 내용으로 reset 시키는 것이다.  
즉, main을 origin/main의 내용으로 강제 리셋시키는 것이다.

<img width="447" alt="Git Error" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/47ecad8f-ce97-4c84-a7c3-d864ea5aef78">

해당 방법을 사용하면 웬만한 pull 에러는 다 해결할 수 있다. 지난번의 에러도 해당 방법으로 해결이 가능하다.  
**단, 문제점은 로컬 저장소의 내용이 날아갈 수 있다..^^.. (갈 수 있는게 아니라 날아간다.)**  
나의 main 브랜치의 경우에는 로컬 main에 작성하는 내용이 없어서 원격 저장소의 main 내용을 reset을 통해 로컬 저장소로 가져와도 됐다. (main 브랜치는 각자 개발한 내용을 모으는 브랜치로, 따로 로컬에서 작성하는 내용이 없음)

그러니 해당 방법을 사용할 경우, 로컬의 내용이 날아가도 되거나... 어딘가에 따로 저장을 해두고 하자.. (아님 다른 방법을 찾자..)

> 참고  
> [해결방법 참고 블로그]('https://programming119.tistory.com/109')  
> [Git도구(head,index,워킹디렉토리)와 reset]('https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0')
