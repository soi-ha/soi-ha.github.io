---
layout: post
categories:
  - TIL
title: "Vue3: 저장 불가 에러 및 emmet 비활성화"
tags:
  - TIL
  - Vue.js
  - Error
---

## __문제__
---
분명 이틀전까지만 해도 vue파일에서 emmet과 저장을 매우 멀쩡하게 잘만 사용했었다.  
그런데.. 오늘 사용하려니까 갑자기 vue 확장자를 가진 파일들만 저장이 되지 않고!  
> ''Vetur', 'ESLint''(구성)에서 코드 동작을 가져오는 중입니다.  

위와 같은 문구들이 경고창으로 뜨면서 계속 로딩 표시만 나타났다..

그리고 다음 사진과 같은 경고창이 갑자기 생겼다..  

<img width="1272" alt="에러화면" src="https://user-images.githubusercontent.com/77609591/220590713-ba4c53e9-f008-4303-a29a-3f077f8d61f3.png">

vetur 파일이 tsconfig.json 파일 어쩌구 경고는 이전에도 계속 떴던거라 걍 무시했고, in the vue3 project 어쩌구 경고는 오늘부터 갑자기 뜨기 시작했다.  
~~경고창에 vue language server를 다운 받으라기에 일단 다운 받았었다.~~  
output에도 vue language server에 error가 뜨고...  
이 경고가 뜬 이후부터 나의 모든 vue파일들은 emmet이 작동되지 않고 파일이 제대로 저장되지 않았다...  

> In the vue 3 project, The Vue Language Features (Volar) is new recommended extension in VSCode.  

> Request textDocument/colorPresentation failed.
  Message: Request textDocument/documentColor failed with message: (0 , rCe.default)(…).filter is not a function
  Code: -32603 

## __해결__
---
아무리!!! 서칭을 해도 나오지가 않아서 `@volar-plugins/vetur`도 설치하구.. 설정 파일에서 emmet관련 코드도 추가했지만.. 도저히 제대로 작동하지가 않았다.  
그렇게 몇시간을 헤매던 중..  
vue3는 vetur가 아닌 volar이라는 것을 사용해야 한다는 걸 알게됐고.. 기존의 vetur은 비활성화를 해두고 volar만 사용을 하라는 글을 발견했다.  
그렇다. __이것이 정답이었다!!!!!__  

vetur는 다음 사진과 같이 비활성화를 하고 volar (vue language sever)
만 활성화를 해야지 위와 같은 오류들도 안 생기고, 정상적으로 vue를 사용할 수 있다.

<img width="1298" alt="vetur 비활성화" src="https://user-images.githubusercontent.com/77609591/220590683-1754e611-08cc-442f-bf0d-f1a84fa327b8.png">

사용안함(작업영역)으로 변경하면 된다. 나중에 또 사용할때는 사용으로 돌리면 된다!

## __정상작동__
---
<img width="1295" alt="emmet 작동" src="https://user-images.githubusercontent.com/77609591/220590709-31f89f45-0e34-4551-bab6-a8506a51d46f.png">

이제는 emmet도 정상적으로 작동하고 vue 파일 저장 역시 정상적으로 잘 작동한다.  
아무리 서칭해도 안 나오길래... 나같은 분을 위해서 (나만 바보인걸지도) 글을 적어본다.


