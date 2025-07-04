---
layout: post
published: true
categories:
  - TIL
title: '오픈소스 라이브러리, 정말 안전할까?'
tags:
  - TIL
  - Security
---

## 오픈소스 라이브러리, 왜 그리고 어떻게 쓰일까?

프론트엔드 개발에서는 라이브러리, 프레임워크, 빌드 툴 등을 자주 사용한다.  
이 중 많은 도구들이 **오픈소스 소프트웨어(OSS)** 로 개발되어 소스 코드가 공개되어 있으며, 누구나 자유롭게 사용할 수 있다.

오픈소스는 개발 속도를 높이고 커뮤니티의 기여를 받을 수 있다는 장점이 있지만,  
**누구나 코드에 접근할 수 있다는 특성은 곧 보안 취약점으로 악용될 가능성도 내포하고 있다.**

실제로 악성 코드가 삽입된 라이브러리를 무심코 설치해 사용자의 브라우저에서 코드가 실행되는 사례들이 발생하고 있다.

---

## 라이브러리에 숨어 있는 보안 위협들

### 1. 서드파티 라이브러리를 경유하는 공격

당사자가 아닌 제3자가 개발한 오픈소스 라이브러리에 **의도적으로 악성 코드를 삽입**하는 방식이다.  
이를 이용해 해당 라이브러리를 사용하는 수많은 사용자에게 악성 행위를 퍼뜨릴 수 있다.

### 2. 코드 리뷰가 부족한 프로젝트에 대한 공격

오픈소스 프로젝트는 누구나 버그를 수정하거나 기능을 추가할 수 있다.  
이 점을 악용해, **리뷰가 충분히 이뤄지지 않는 프로젝트에 악성 코드를 병합**하는 사례도 존재한다.

### 3. 계정 탈취를 통한 악성 코드 배포

라이브러리 개발자 또는 유지보수자의 **npm/GitHub 계정을 탈취**해, 악성 버전을 공식적으로 배포하는 방식이다.  
*2018년 ESLint 패키지 유지보수자의 계정이 탈취되어 악성 코드가 배포된 사례*가 대표적이다.  
이후 GitHub와 npm은 2단계 인증(2FA)을 필수로 적용하게 되었다.

### 4. 의존성 라이브러리를 통한 간접 감염

우리가 사용하는 라이브러리(A)는 안전하더라도, A가 의존하는 라이브러리(B)에 **악성 코드가 포함된 경우** 간접 감염이 발생할 수 있다.

### 5. CDN에서 콘텐츠가 변조된 경우

CDN을 통해 불러온 라이브러리가 **제공 과정에서 변조**되어 악성 코드가 삽입되는 사례도 있다.

### 6. CDN에서 취약한 버전의 라이브러리를 불러오는 경우

CDN에서 최신 버전이 아닌, **보안 취약점이 존재하는 구버전을 참조**하는 경우도 있다.  
이 문제를 방지하기 위해서는 CSP(Content Security Policy) 설정을 활용할 수 있다.

```js
Content-Security-Policy: script-src cdn.example
```

---

## 오픈소스 라이브러리, 안전하게 사용하는 방법

### 1. 취약점 확인 도구와 서비스 활용

라이브러리의 취약점을 조기에 발견하기 위해 **자동화 도구**를 활용한다.

#### 1-1. 커맨드라인 도구: `npm audit`

```bash
npm audit
```

- 의존성에 알려진 취약점이 있는지 확인할 수 있다.
- `npm audit fix`로 자동 수정이 가능하다.
- `--production` 옵션으로 production 환경 의존성만 검사할 수 있다..

#### 1-2. GitHub Dependabot

- GitHub에서 제공하는 의존성 취약점 점검 도구이다.
- `package-lock.json`, `yarn.lock` 등을 분석하여 위험 라이브러리 사용 여부를 알려준다.
- 취약 패키지 자동 업데이트 PR도 생성 가능하다.

#### 1-3. 외부 보안 서비스

- [Snyk](https://snyk.io), [yamory](https://yamory.io)와 같은 서비스도 있다.

### 2. 유지보수가 활발한 라이브러리 사용

보안 취약점은 시간이 지남에 따라 발견되므로, **지속적인 유지보수**가 이뤄지는 라이브러리를 선택해야 한다.
커뮤니티가 활발하고 릴리즈 주기가 일정한 프로젝트를 우선 고려하자.

### 3. 항상 최신 버전 유지

보안 패치는 주로 최신 버전에 포함되므로,
라이브러리와 의존성 파일(`package-lock.json`, `yarn.lock` 등)을 **주기적으로 업데이트**하는 습관이 중요하다.
대표적인 자동화 도구로는 **Renovate**가 있다.

### 4. Subresource Integrity(SRI)를 통한 변조 방지

외부 CDN을 통해 스크립트나 스타일시트를 불러올 경우,
**Subresource Integrity(SRI)** 를 사용해 해당 파일이 변조되지 않았는지 검증할 수 있다.

```html
<script src="https://cdn.example.com/lib.js" integrity="sha384-abc123" crossorigin="anonymous"></script>
```

브라우저는 지정된 해시값과 실제 리소스를 비교해 불일치 시 로드를 차단한다.
CDN 서버는 CORS 헤더(`Access-Control-Allow-Origin`)도 반드시 포함해야 한다.

### 5. CDN에서 버전 명시하여 사용하기

CDN을 사용할 때는 `latest`나 `main` 같은 **가변 경로 대신, 고정된 버전 번호**를 사용하는 것이 보안에 안전하다.

```html
<!-- Bad -->
<script src="https://cdn.example.com/library.js"></script>

<!-- Good -->
<script src="https://cdn.example.com/library@1.2.3.js"></script>
```

---

## 마무리

오픈소스 라이브러리는 프론트엔드 개발을 빠르고 유연하게 만들어주는 큰 도구다. 하지만 이런 라이브러리로 인해 발생하는 보안 취약점은 결국에는 라이브러리 사용자가 책임을 지게 된다. 그렇기에 사용하는 라이브러리의 보안이 안전한지 반드시 점검해야 한다는 것을 이번에 책을 읽으면서 배우게 되었다.

라이브러리 설치 전 "정말 믿을 수 있는가?"를 한 번 더 점검하는 습관이 앞으로의 나의 개발에 있어서 프로젝트의 신뢰성과 안전성을 지키는 강력한 방법이 될 것이다.

## 📚 참고

- 도서: 프런트엔드 개발을 위한 보안입문
