---
layout: post
published: true
categories:
  - TIL
title: '.npmrc 파일에서 node 버전 제한하기'
tags:
  - TIL
  - WoowaCourse
  - Node.js
---

1주차 프리코스 미션을 진행하기 전에 처음 보는 파일이 있어서 확인했더니 이상한(?) `engine-strict = true` 한줄 코드가 적혀있었다. 그래서 정체를 알아보기 위해서 서칭해보았다.

### .npmrc?

---

npmrc 파일은 npm에서 설정을 저장해두는 파일이다.  
우테코 프리코스에서는 npmrc 파일을 통해 node와 npm의 버전을 제한하기 위해서 사용했다.

### 버전 제한하기

---

- .npmrc 파일에 다음과 같이 코드 작성  
  `engine-strict = true`
- package.json에 engines 속성 추가
  ```json
  "engines": {
  	"npm": ">=9.6.7",
  	"node": ">=18.17.1"
  }
  ```
  까지 하면 끝..!

#### package.json

---

package.json에 작성한 코드를 살펴보자

```json
  "engines": {
  	"npm": ">=9.6.7",
  	"node": ">=18.17.1"
  }
```

`"engines"`는 프로젝트가 동작할 때 필요한 실행 환경의 버전을 지정한다.

`"npm": ">=9.6.7"`는 프로젝트가 사용해야 하는 npm 버전을 지정한다. `">=9.6.7"`는 최소한 9.6.7 버전 이상이어야 한다는 것이다.

`"node": ">=18.17.1"`프로젝트가 실행될 때 필요한 Node.js 버전을 지정한다. `">=18.17.1"`은 최소한 18.17.1 버전 이상이어야 한다는 것이다.

위 코드를 통해 프로젝트가 필요로 하는 버전을 명시적으로 지정할 수 있다. 작성한 버전에 맞지 않을 경우 npm 혹은 node가 버전이 맞지 않다는 문구와 함께 실행되지 않는다.

> 참고  
> [Node.js 버전 강제하기](https://remocon33.tistory.com/698)  
> [npmrc 파일과 npm config 커맨드](https://www.daleseo.com/js-npm-config/)  
> Chat GPT
