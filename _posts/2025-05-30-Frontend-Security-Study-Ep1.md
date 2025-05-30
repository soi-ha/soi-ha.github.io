---
layout: post
published: true
categories:
  - TIL
title: '왜 HTTP와 HTTPS 혼용 사용이 위험할까?: Mixed Content'
tags:
  - TIL
  - Security
---

## HTTP와 HTTPS

웹에서 데이터를 주고받을 때 사용하는 대표적인 프로토콜이 바로 HTTP와 HTTPS이다.

| 구분             | HTTP             | HTTPS                           |
| :--------------- | :--------------- | :------------------------------ |
| 데이터 전송 방식 | 평문(Plaintext)  | 암호화(Encrypted, TLS/SSL 사용) |
| 보안성           | 낮음             | 높음                            |
| 주요 기능        | 기본 데이터 전송 | 암호화 + 무결성 + 인증 제공     |

**HTTP(HyperText Transfer Protocol)** 는 데이터를 암호화하지 않고 전송한다. 즉, 평문(Plaintext)으로 전송되기 때문에 중간에서 누군가가 데이터를 가로채거나 조작할 수 있는 위험이 있다.

**HTTPS(HyperText Transfer Protocol Secure)** 는 HTTP에 보안 기능이 추가된 프로토콜이다. TLS(SSL)를 통해 데이터를 **암호화(Encryption)** 하여 전송한다. 그렇기 때문에 보안성이 훨씬 뛰어나다. 따라서 사용자 정보, 로그인, 결제 등의 중요한 데이터를 다루는 서비스는 반드시 HTTPS를 사용해야 한다.

이렇게 HTTP https에 대해서 알게 되었는데, HTTP와 HTTPS에 대해 배우다 보면 HTTP와 HTTPS를 혼용하지 말라는 이야기를 듣는다. 왜 일까? HTTP와 HTTPS를 혼용하면 어떤 일이 일어나기에 혼용하여 사용하지 말라는 것일까?

> <i style="color:gray; font-weight:600;">TLS(SSL)란?</i>  
> <i style="color:gray;">TLS(SSL)는 데이터를 암호화하여 주고받을 수 있도록 도와주는 기술로, 인터넷에서 주고받는 정보를 안전하게 지켜주는 역할이다. (SSL은 TLS의 옛 이름으로, 현재는 TLS라는 이름으로 더 많이 불린다.)</i>

## http와 https를 혼용 사용하면 발생하는 일

예를 들어, 우리가 어떤 은행 웹사이트(https://mybank.com)에 접속했다고 가정해보자. 이 사이트는 HTTPS로 안전하게 암호화되어 있으니까, 로그인이나 송금 같은 민감한 정보를 안전하게 입력할 수 있을 것이다.

그런데 이 안전한 페이지에서 불러오는 이미지나 자바스크립트 파일이 **HTTP(보안되지 않은 주소)** 로 되어 있다면 어떻게 될까?  
로그인 버튼에 연결된 HTTP 스크립트를 공격자가 중간에 가로채서, 내 입력값(아이디, 비밀번호)을 훔쳐보거나 다른 서버로 보내게 만들 수도 있다.
혹은 HTTP 이미지가 변조되어, 정상적인 안내 이미지 대신 피싱 사이트로 유도하는 광고 배너로 바뀔 수도 있다.

즉, HTTPS 페이지라고 해서 무조건 안전한 게 아니라, **그 안에 있는 모든 리소스도 HTTPS로 안전하게 불러와야 진짜 안전한 것**이다!

조금 더 이해하기 쉽게 일상생활을 비유로 설명해보자면, 피크닉을 가기 위해 도시락을 챙기기로 했다.

이때, HTTPS 페이지는 안전한 도시락이다. 그런데 그 안에 들어있는 반찬(이미지, JS, CSS 등)을 **여름철 길거리 노점에서 산 음식(HTTP 리소스)** 으로 가져왔다면? 도시락 통은 멀쩡하지만 안에 있는 반찬이 상했거나 변조됐다면 먹는 사람이 배탈이 날 수도 있다.

마찬가지로, HTTPS 페이지 안에 HTTP 리소스가 섞이면 전체 보안이 깨질 수 있는 것이다. 이처럼 http와 https를 섞어 사용하면 보안적으로 매우 위험한 상황이 발생할 수 있는데, 우리는 이런 상황을 **Mixed Content(혼합 콘텐츠)** 라고 부른다.

## Mixed Content(혼합 콘텐츠)?

**Mixed Content** 는 HTTPS 페이지에서 HTTP 리소스를 함께 사용하는 것을 말한다. 예를 들어, https로 접속한 페이지에서 아래처럼 http로 스크립트나 이미지를 불러오는 경우이다.

```html
<!-- HTTPS 페이지에서 HTTP 이미지 로드 -->
<img src="http://example.com/image.png" />

<!-- HTTPS 페이지에서 HTTP 스크립트 로드 -->
<script src="http://example.com/script.js"></script>
```

### Mixed Content 패턴의 종류

Mixed Content는 다음 두 가지로 구분된다.

| 유형                               | 설명                                                            | 브라우저 반응                        | 위험도    |
| ---------------------------------- | --------------------------------------------------------------- | ------------------------------------ | --------- |
| **패시브 콘텐츠(Passive Content)** | 이미지, 오디오, 비디오 등 페이지의 동작과 직접 관련 없는 콘텐츠 | 일부 브라우저는 로드하지만 경고 표시 | 낮음      |
| **액티브 콘텐츠(Active Content)**  | JS, CSS, iframe 등 페이지의 동작에 관여하는 리소스              | 대부분 브라우저에서 자동 차단        | 매우 높음 |

**Pssive Content**는 이미지와 오디오, 비디오와 같은 리소스가 Mixed Content를 발생시키는 패턴이다. 이런 리소스는 변경되면 잘못된 정보가 표시될 수는 있지만 브라우저에서 실행되는 코드는 포함하지 않기 때문에 위험도가 낮고 영향이 적다.

**Active Content**는 JS, CSS 등 브라우저에서 실행되는 코드에 대한 Mixed Content 패턴이다. 해당 코드가 변경되면 보안 공격과 같은 문제가 발생할 위험이 높아 큰 피해를 발생시킬 위험성이 있다. 그렇기에 대부분의 브라우저에서 다른 사이트에서 전송되는 Active Content의 하부 리소스 접근이 차단되도록 되어있다. 따라서 HTTP로 전송되는 JS와 CSS는 변경되지 않았어도 차단되어 제대로 동작하지 않을 때도 있음을 개발할 때 주의해야 한다.

### Mixed Content의 위험성

HTTPS 페이지에서 HTTP 리소스를 함께 사용하면 다양한 보안 문제가 발생할 수 있다.

- 중간자 공격(MITM): 공격자가 HTTP 리소스를 가로채서 악성 스크립트를 삽입하거나 페이지 동작을 변조할 수 있다.

- 무결성 훼손: 전송 중 리소스가 변조되어 사용자가 원래와 다른 정보를 보게 될 수 있다.

- 쿠키 및 사용자 정보 탈취: HTTP 요청을 통해 인증 쿠키나 입력한 개인정보가 공격자에게 노출될 수 있어 세션 탈취와 개인정보 유출의 위험이 있다.

- 브라우저 경고 및 사이트 신뢰도 하락: 브라우저는 Mixed Content를 탐지하면 경고를 띄우거나 리소스를 차단하며, 이는 사용자 신뢰도 저하와 검색 엔진 최적화(SEO)에도 부정적인 영향을 준다.

## Mixed Content를 방지하는 다양한 해결 방법

Mixed Content로 인해 발생할 수 있는, 다양한 보안 위험성을 방지하기 위한 해결 방법은 아래와 같다.

- **모든 리소스 HTTPS로 통일**  
  이미지, JS, CSS 등 외부 리소스 URL을 HTTPS로 수정하고, 상대 경로나 프로토콜 상대 URL(`//` , `ex) //example.com/script.js`)을 사용해 불필요한 HTTP 참조를 방지한다.

- **Content Security Policy(CSP) 적용**  
  브라우저가 모든 리소스를 HTTPS로만 로드(`default-src 'self' https:`)하거나, HTTP 요청을 자동으로 HTTPS로 업그레이드(`upgrade-insecure-requests`)하도록 CSP를 설정한다.

  ```jsx
  Content-Security-Policy: default-src 'self' https:; upgrade-insecure-requests;
  ```

- **HSTS 활성화**  
  브라우저가 해당 사이트는 무조건 HTTPS로 접속하도록 강제하는 HSTS 정책을 설정한다. 응답 헤더에 Strict-Transport-Security 헤더를 추가하여, 브라우저가 HTTPS를 사용하도록 강제한다.

  ```jsx
  strict-transport-security: max-age=31536000; includeSubdomains; preload
  ```

- **개발 단계에서 Mixed Content 검사**  
  개발자 도구 콘솔, 자동화된 테스트, Lint 검사, Lighthouse 등을 통해 HTTP 링크를 사전에 점검한다.

- **CDN, 외부 리소스 관리**  
  HTTPS를 지원하는 CDN을 사용하고 HTTPS를 지원하지 않는 리소스는 자체 호스팅하거나 다른 대안을 찾는다.

- **서비스 워커 활용**  
  서비스 워커에서 HTTP 요청을 가로채 HTTPS로 리다이렉트하거나 차단하는 로직을 추가한다.

- **모니터링 및 사용자 피드백 확인**  
  Sentry, Datadog 등 모니터링 툴로 Mixed Content 에러를 실시간 확인하고 사용자 피드백도 주기적으로 검토한다.

## 마무리 말

보안은 단 한 번의 실수로도 무너질 수 있는 중요한 부분이라고 생각한다. 개발 단계부터 배포 후 유지보수까지, Mixed Content를 방지하기 위한 꼼꼼한 관리와 점검은 선택이 아닌 필수이다. 작은 습관이 큰 사고를 예방한다는 마음으로 항상 안전한 웹사이트를 만들어 나갈 수 있도록하는 개발자가 되도록 할 것이다!

## 📚 참고

- 도서: 프런트엔드 개발을 위한 보안입문
- [MDN Web Docs: Mixed Content](https://developer.mozilla.org/ko/docs/Web/Security/Mixed_content)
- [MDN Web Docs: HTTP 개요](https://developer.mozilla.org/ko/docs/Web/HTTP/Guides/Overview)
- [MDN Web Docs: Strict-Transport-Security](https://developer.mozilla.org/ko/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security)
- [MDN Web Docs: 컨텐츠 보안 정책 (CSP)](https://developer.mozilla.org/ko/docs/Web/HTTP/Guides/CSP)
