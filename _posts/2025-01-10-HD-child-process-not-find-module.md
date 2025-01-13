---
layout: post
published: true
categories:
  - TIL
  - Project
title: 'Error | Module not found: Error: Can't resolve child_process'
tags:
  - Wooteco
  - 행동대장
---

## 에러

[폴리필 에러를 해결](https://soi-ha.github.io/til/project/2025/01/10/HD-polyfills-error.html)했지만 아직도 우리에겐 에러가 남았다.. 에러 내용은 다음과 같다.

<img width="840" alt="error_child_process" src="https://github.com/user-attachments/assets/96e01988-837f-4c80-b9bc-73a65338f48e" />

### 에러 해석

`Module not found: Error: Can't resolve 'child_process' in '/Users/soha/soha/wooteco/2024-haeng-dong/client/node_modules/google-auth-library/build/src/auth’`

1. `Module not found`: 특정 모듈을 찾지 못하겠다.
2. `Error: Can't resolve 'child_process'`: Node.js의 내장 모듈인 `child_process`를 찾을 수 없다는 뜻. 이 모듈은 일반적으로 서버 환경(Node.js)에서 사용되며, 브라우저 환경에서는 사용할 수 없음.....
3. `in '/Users/soha/soha/wooteco/2024-haeng-dong/client/node_modules/google-auth-library/build/src/auth'`: 에러가 발생한 위치는 `google-auth-library` 패키지 내부의 코드에서 `child_process`를 사용하려고 시도한 부분임.

### 종합하자면?

`google-auth-library`는 Google API 인증을 위해 사용되는 라이브러리이다. 이 라이브러리는 서버 환경(Node.js)을 위해 설계되었으므로, 브라우저 환경에서 사용하려고 하면 `child_process`와 같은 Node.js 전용 모듈을 찾지 못해 에러가 발생한다!

## 결론

너! Node.js에서 실행되어야 하는거 왜 브라우저에서 하셈? 그거 브라우저에서는 실행 못하니 돌아가.
