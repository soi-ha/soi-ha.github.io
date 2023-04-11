---
layout: post
categories:
  - TIL
title: 'Error: Cannot find module loader 에러 해결'
tags:
  - TIL
  - Error
---

React로 프로젝트를 만들던 중.. sass를 설치하고 나서 갑자기 Cannot find module loader 에러가 발생하였다..!  
길게 에러 메세지가 떴는데 loader 뒤에도 babel 뭐시기 경로.. 이러는 걸 보면.. sass를 설치하고 뭔가 내부적으로 꼬인게..아닐까..? 싶은 추측중이다.

쨋든! 정확히 뭐가 문제인지는...! 잘 모르겠지만.. 서칭 끝에 해결 방법을 찾았다...!

- 캐시 제거
  ```bash
  npm cache clean --force
  ```
- node_modules 폴더 삭제
- package-lock.json 파일 삭제
- node 재설치
  ```bash
  npm i
  ```
  재설치를 하면 삭제했던 node_modules 폴더와 package-lock.json 파일이 다시 설치된다

해당 방법을 다 하고 npm을 다시 실행했더니! 잘 작동하게 됐다 ㅜ...!
