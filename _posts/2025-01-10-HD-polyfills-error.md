---
layout: post
published: true
categories:
  - TIL
  - Project
title: 'Error | webpack < 5 used to include polyfills for node.js core modules by default'
tags:
  - Wooteco
  - 행동대장
---

## 문제 발생 상황

때는, 행동대장 회원 탈퇴 기능 구현에서 google spreadsheets api를 사용하기 위해서 google-auth-library를 활용하여 코드를 작성하던 중.. 이런 어마무시한 에러의 늪에 빠져버렸다.

<img width="611" alt="webpack_v5_error_1" src="https://github.com/user-attachments/assets/0834ea75-cd07-43bf-bfbd-cae50c1a8c79" />

## 이 에러가 무슨 에러인데?

`webpack < 5 used to include polyfills for node.js core modules by default.
This is no longer the case. Verify if you need this module and configure a polyfill for it.`

위 에러를 해석하면 다음과 같다.

`이 에러 메시지는 Webpack 5부터 Node.js의 코어 모듈(예: http, https, fs, 등)에 대한 폴리필(polyfill)이 기본적으로 제공되지 않는다는 것을 의미함.`

Webpack 4까지는 브라우저 환경에서도 Node.js 코어 모듈이 동작할 수 있도록 **자동으로 폴리필을 포함**했다. 그러나 **Webpack 5부터는 이를 자동으로 포함하지 않으며**, 사용자가 필요한 폴리필을 **직접 설정**해야 하도록 바뀌었다. 우리 행동대장의 Webpack의 버전은 **`^5.93.0`**... 그렇기에 폴리필을 직접 설정을 해줘야 해당 에러가 해결된다.

## 근데 폴리필이 뭐예요?

폴리필(Polyfill)은 최신 기술이나 특정 기능이 일부 환경에서 지원되지 않을 때, **그 기능을 대신 구현하여 호환성을 제공하는 코드나 라이브러리**를 말한다.  
쉽게 말해, **오래된 브라우저나 특정 환경에서도 새로운 기능을 사용할 수 있도록 도와주는 "백업" 코드**이다.

### 그렇다면 폴리필이 왜 필요할까?

JavaScript는 빠르게 진화하지만, 모든 브라우저나 환경이 새로운 기능을 지원하는 것은 아니다.  
예를 들어 `Array.prototype.includes`는 최신 브라우저에서는 지원되지만, 구형 브라우저(ex, IE)에서는 작동하지 않는다. 이 경우, 폴리필을 사용하면 `Array.prototype.includes`와 동일한 기능을 추가할 수 있다.

### Webpack에서 폴리필은 어떻게 설정해야 하죠?

Webpack은 브라우저 환경에서 동작하도록 설계되었지만, 일부 Node.js 기능을 사용할 때 폴리필이 필요하다. 그렇기에 필요한 Node.js 코어 모듈(`http`, `fs` 등)을 직접 설치하고 설정해주면 되겠다!

## 에러 해결

아래 이미지 외에도 많은 폴리필 에러가 발생했다.. 에러 메세지를 확인하면 설치해야 할 라이브러리를 알려주고 있어 설치하라는 라이브러리를 모두 설치하고 설정해주었다.

<img width="991" alt="webpack_v5_error_2" src="https://github.com/user-attachments/assets/be3916d9-0941-4355-a4f3-a645eac1dc85" />

- **에러 메시지의 라이브러리**
  - url
  - crypto-browserify
  - stream-http
  - https-browserify
  - os-browserify
  - stream-browserify
  - path-browserify
  - querystring-es3
  - assert

위 라이브러리를 설치한 후 webpack 파일의 `resolve.fallback`에 값을 설정해줬다.

또한 추가로 reqire를 사용할 수 있도록 createRequire를 생성했다.

```jsx
import {createRequire} from 'module';

const require = createRequire(import.meta.url);

export default {
	// ...
  resolve: {
    fallback: {
      url: require.resolve('url'),
      fs: false,
      crypto: require.resolve('crypto-browserify'),
      http: require.resolve('stream-http'),
      https: require.resolve('https-browserify'),
      os: require.resolve('os-browserify/browser'),
      stream: require.resolve('stream-browserify'),
      path: require.resolve('path-browserify'),
      querystring: require.resolve('querystring-es3'),
      assert: require.resolve('assert/'),
    },
  // ...
};
```

### 결과

아래 이미지를 통해 폴리필과 관련된 에러가 모두 사라졌음을 확인할 수 있다! ~~(근데 난 다른 에러가 남았지!)~~

<img width="839" alt="webpack_v5_error_3" src="https://github.com/user-attachments/assets/a580b9d3-696d-4d63-a450-f731fcc75a5c" />

## 참고

- [React - BREAKING CHANGE: webpack < 5 used to include polyfills for node.js core modules by default](https://mr-son.tistory.com/153)
