---
layout: post
published: true
categories:
  - TIL
title: 'jekyll Github 블로그 sitemap 파일 생성하기 & `ERROR: While executing gem ... (Gem::FilePermissionError)` 에러 해결방법'
tags:
  - TIL
  - ERROR
---

## 🗺️ sitemap이란?

sitemap이란 검색엔진이 크롤링하여 색인할 수 있도록 모든 웹페이지를 나열한 XML 파일이다. 검색엔진 크롤러가 접근하기 어려운 페이지를 포함한 모든 페이지의 정보를 제공하여 빠짐없이 색인될 수 있도록 돕는다.  
주의할 점은 sitemap이 직접적으로 검색엔진 결과의 순위를 올려주지는 않는다는 것이다.

## 💡 sitemap 생성 방법 (jekyll로 생성한 GitHub 블로그 기준)

아래 설명은 jekyll로 생성한 깃허브 블로그에 sitemap을 생성하는 것을 기준으로 한다. 만약 jekyll로 생성한 깃허브 블로그가 아니라면 아래 방법으로는 sitemap 파일을 생성할 수 없다.

### 1. 로컬의 `.github.io` 폴더의 `Gemfile` 파일에 다음 코드를 추가하기

```ruby
gem 'jekyll-sitemap'
```

여기서 `Gemfile`은 ruby 프로젝트에 필요한 gem의 목록과 버전을 관리해주는 파일이다. 프로젝트에서 사용하는 라이브러리(이를 gem이라고 부름)를 관리하는 역할을 한다.

`Gemfile`을 통해 프로젝트에서 사용할 gem들을 명시하고, 이를 `Bundler`라는 툴이 관리해준다. `Bundler`는 `Gemfile`에 명시된 gem들이 서로 호환되는 버전으로 설치되도록 도와준다.

### 2. `_config.yml`에 다음과 같이 플러그인을 추가하기

```yaml
plugins:
  - jekyll-sitemap
```

### 3. 터미널에서 `bundle` 명령어를 실행하기

```bash
bundle
```

`bundle`(=== `bundle install`) 명령어는 다음과 같은 일을 진행한다.

1. `Gemfile`을 읽어서 필요한 gem들을 확인하고
2. `Gemfile.lock` 파일이 있으면, 그 파일에 기록된 정확한 버전을 설치하고,
3. `Gemfile.lock` 파일이 없으면 호환 가능한 최신 버전으로 설치한 후 `Gemfile.lock` 파일을 생성해준다.

그래서 보통 Rails나 Ruby 프로젝트를 처음 설치하거나 업데이트할 때, 자주 쓰는 명령어이다.

즉, `bundle` 명령어는 Bundler를 이용해서 `Gemfile`에 적힌 gem들을 설치한다.

실행 결과 예시:

```bash
Bundle complete! 3 Gemfile dependencies, 47 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
```

터미널에 bundle 실행이 정상적으로 진행됐을 때 결과 이미지:

<img width="450" alt="bundle 명령어 실행 결과" src="https://github.com/user-attachments/assets/41945983-54e0-4e37-ad38-135e9e484560" />

위와 같이 명령어 실행 결과가 나타났다면, 와우! 당신은 이제 더이상 할게 없다. [바로 4번을 실행](#4-jekyll-sitemap-설치)해주면 된다.

그러나 만약 bundle 명령어를 실행했는데 아래 이미지와 같이 에러가 발생다면?

   <img width="482" alt="bundle 명령어 실행 에러" src="https://github.com/user-attachments/assets/283232ee-7fbc-43aa-a1b9-522372f55917" />

그리고 `gem install bundler` 명령어를 실행해봤는데 또 에러가 발생했다면? 글을 계속 내려 에러를 해결해보자.

  <img width="506" alt="gem bundler 설치 에러" src="https://github.com/user-attachments/assets/2b60699b-08a3-48bb-8b97-7c63eb9753ae" />

---

## 🚨 `ERROR: While executing gem ... (Gem::FilePermissionError)` 에러 해결 방법

### 에러 발생 원인

해당 에러는 macOS에 기본으로 설치되어 있는 ruby의 gem 디렉토리(`/Library/Ruby/Gems/2.6.0`)에 쓰기 권한이 없어서 발생한 것이다. macOS의 기본 ruby는 보안상 사용자에게 gem 설치 권한을 제공하지 않는다.

따라서 gem 설치 시(아래 명령어) 권한 에러가 발생한다.

```bash
gem install bundler
```

에러 발생 이미지:

<img width="506" alt="gem bundler 설치 에러" src="https://github.com/user-attachments/assets/2b60699b-08a3-48bb-8b97-7c63eb9753ae" />

해당 에러의 해결 방법은 두 가지가 있다.

#### 1️⃣ 관리자 권한으로 설치하기

권장하지 않는 방법이다.

```bash
sudo gem install bundler
```

단, sudo를 사용하면 관리자 권한으로 명령어가 실행되기 때문에, 만약 설치하려는 gem이나 명령어에 문제가 발생하면 시스템 전체에 영향을 줄 수 있어 위험할 수 있다. 따라서 해당 방법은 권장되지 않는다.

#### 2️⃣ Ruby 버전 관리 도구(`rbenv`)를 활용하기

가장 안전하고 권장되는 방법이다.

`rbenv`같은 ruby 버전 관리 도구를 사용하면 사용자 디렉토리 내에서 ruby를 별도로 관리할 수 있어서, 시스템 디렉토리에 접근할 필요가 없다. 이 방법을 사용하면 권한 문제를 피할 수 있다.

이 글에서는 `rbenv`를 활용하여 에러를 해결할 것이다.

### 😎 `rbenv`를 활용하여 에러 해결하기

- **`rbenv` 설치**

  homebrew를 활용하여 `rbenv`와 `ruby-build`를 설치한다.

  ```bash
  brew update
  brew install rbenv ruby-build
  ```

  여기서 `ruby-build`는 `rbenv`와 함께 사용되는 플러그인으로, 다양한 ruby 버전을 쉽게 설치할 수 있도록 지원해준다. `rbenv`와 함께 `ruby-build`를 설치하면 ruby 버전 관리와 함께 새로운 ruby 버전 설치도 한 번에 준비할 수 있어서 더 편리하다.

- **설치한 `rbenv`와 ruby 버전 확인하기**

  꼭 필요한 단계는 아니지만, 본격적으로 `rbenv`를 사용하여 ruby 버전을 관리하기 전의 상태가 어떤지 확인해줬다.

  ```bash
  > rbenv version
  * system

  > ruby --version
  ruby 2.6.10p210
  ```

  `rbenv`를 통해 ruby 버전을 관리하기전 설치되었던 ruby의 버전은 `2.6.10`이다. 나는 글 작성 기준 가장 안정된 버전인 `3.4.2`로 ruby의 버전을 관리할 계획이다.

  실제 터미널 실행 결과 이미지:

  <img width="438" alt="rbenv 설치 버전 없음" src="https://github.com/user-attachments/assets/68122962-4dd5-4f4a-969b-0cab7889ccb2" />

- **`rbenv`를 활용하여 Ruby 버전 설치 (버전: `3.4.2`)**

  ```bash
  rbenv install 3.4.2
  ```

  실제 터미널 실행 결과 이미지:

  <img width="493" alt="rbenv 버전 설치" src="https://github.com/user-attachments/assets/a121d90d-9656-477c-b08f-5e794f83364a" />

  설치가 정상적으로 되었다면, version 확인을 했을 때 아래와 같이 출력된다.

  ```bash
  > rbenv version
  system
  * 3.4.2
  ```

- **`rbenv`로 global 버전을 `3.4.2`로 설정 및 확인**

  `rbenv`로 global 버전 또한 `3.4.2`로 설정해준다.

  ```bash
  > rbenv global 3.4.2

  > rbenv version
  3.4.2 (set by /Users/사용자이름/.rbenv/version)
  > ruby --version
  ruby 3.4.2 (2025-02-15 revision d2930f8e7a)
  ```

  global 버전 설정 이전에 ruby 버전을 확인해보면 이전에 설치되어있던 버전으로 뜬다. (`rbenv`의 버전을 `3.4.2`로 설치한 후 임에도.)  
  global 버전 설정을 한 후 ruby 버전을 다시 확인하면 `3.4.2` 버전으로 설치되어 있는 것을 확인할 수 있다.

  실제 터미널 실행 결과 이미지:

  <img width="431" alt="ruby 설치 버전 확인" src="https://github.com/user-attachments/assets/e483ebe3-5600-46bc-8e74-05b62224b295" />

- **쉘 설정 파일에 `rbenv` 설정 추가**

  쉘 설정 파일을 수정하기 위해 아래 코드를 터미널에 실행하여 쉘 설정 파일을 vscode로 열어준다.

  ```bash
  code ~/.zshrc
  ```

  그리고 `.zshrc` 파일을 열어 아래 코드를 추가한다.

  ```bash
  [[ -d ~/.rbenv ]] && \
    export PATH=${HOME}/.rbenv/bin:${PATH} && \
    eval "$(rbenv init -)"
  ```

  #### 코드 설명

  <img width="506" alt="쉘 파일 코드 추가" src="https://github.com/user-attachments/assets/86e204d4-82f4-493b-bb28-94b7cb8866f2" />

  - `[[ -d ~/.rbenv ]]`: rbenv가 설치되어 있는지 확인한다. rbenv가 설치되어 있지 않다면 이후 명령어들이 실행되지 않아 불필요한 오류를 방지할 수 있다.
  - `export PATH=${HOME}/.rbenv/bin:${PATH}`: rbenv 명령어를 터미널에서 인식할 수 있도록 경로를 추가한다.
  - `eval "$(rbenv init -)"`: rbenv 초기화 스크립트를 실행하여 Ruby 버전 관리 기능을 활성화한다.

  이 코드는 `rbenv`가 설치된 경우에만 `rbenv`의 경로를 추가하고 초기화를 진행하여, 이후에 ruby 버전 관리를 원활하게 할 수 있도록 도와준다.

  즉, 이 코드를 쉘 설정 파일(`.zshrc` 등)에 추가하면, 매번 터미널을 열 때마다 `rbenv`가 자동으로 초기화되어 ruby 버전을 쉽게 관리할 수 있게 된다. 만약 이 코드를 추가하지 않으면, `rbenv` 명령어를 인식하지 못하거나, ruby 버전 전환 등의 기능을 제대로 사용할 수 없게 된다.

- **변경한 쉘 설정 적용**

  ```bash
  source ~/.zshrc
  ```

자, 이제 모든 작업은 끝이 났다. 이제 정상적으로 작동하는지 확인해보자.

### `bundle`이 정상적으로 실행되는 지 실행하기

이제 다시 블로그 루트 디렉토리에서 `bundle` 명령어를 실행한다.

```bash
> bundle
Bundle complete! 3 Gemfile dependencies, 47 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
```

위와 같은 실행 결과가 나타났다면 정상적으로 `bundle` 설치가 완료된 것이다!

---

### 4. `jekyll-sitemap` 플러그인 설치하기

아래 명령어를 실행하여 `jekyll-sitemap` 플러그인을 설치해보자. 해당 플러그인은 jekyll의 sitemap을 자동으로 만들어준다.

```bash
gem install jekyll-sitemap
```

아래와 같이 응답 결과가 출력되었다면 정상적으로 플러그인이 설치가 된 것이다.

```bash
Successfully installed jekyll-sitemap-1.4.0
1 gem installed
```

실제 실행 결과 이미지:

<img width="467" alt="jekyll-sitemap 플러그인 설치 성공" src="https://github.com/user-attachments/assets/30ca6dbf-1de7-4c34-a325-c094568aa018" />

### 5. GitHub에 변경사항을 커밋하고 푸시하기

변경사항을 github에 push하면 자동으로 사이트맵이 생성된다.  
로컬에서는 파일이 보이지 않을 수 있지만, 실제 블로그 사이트에서 확인을 해보면 `sitemap.xml` 파일이 정상적으로 생성된 것을 확인할 수 있다.

<img width="1415" alt="Image" src="https://github.com/user-attachments/assets/95ae0f25-87dd-4c01-92f1-8893e0d0efd2" />

## 📚 참고

- [사이트맵(sitemap.xml) 쉽게 만들고 제출하기!](https://seo.tbwakorea.com/blog/how-to-create-and-submit-a-sitemap/)
- [GitHub Pages: Jekyll 플러그인 사용하여 sitemap.xml 생성하기 (구글 콘솔에 등록 성공!)](https://velog.io/@fitf_/GitHub-PagesJekyll-%ED%94%8C%EB%9F%AC%EA%B7%B8%EC%9D%B8-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-sitemap.xml-%EC%83%9D%EC%84%B1%ED%95%98%EA%B8%B0-%EA%B5%AC%EA%B8%80-%EC%BD%98%EC%86%94%EC%97%90-%EB%93%B1%EB%A1%9D-%EC%84%B1%EA%B3%B5)
- [ERROR: - While executing gem ... (Gem::FilePermissionError)에러 해결(Mac에서 Gem::FilePermissionError 에러 해결)](https://ccomccomhan.tistory.com/282)
