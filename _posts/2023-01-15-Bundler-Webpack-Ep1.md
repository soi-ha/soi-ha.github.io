---
layout: post
categories:
  - TIL
title: "Bundler: Webpack Ep.1"
tags:
  - TIL
  - Bundler
  - Webpack
---

## __프로젝트 생성__
---

- 패키지 설치
    
  ```bash
  npm init -y
  
  # webpack 설치
  npm i -D webpack webpack-cli webpack-dev-server@next
  ```
  
  webpack을 설치하면서 webpack-cli 패키지와 webpack-dev-server 패키지도 같이 설치를 해준다.  
  이때 webpack-dev-server 패키지는 webpack-cli 패키지와 버전을 일치시켜야 하기 때문에 뒤에 **@next** 키워드를 추가로 작성한다.
  
- package.json
  
  ```json
  "scripts": {
      "dev": "webpack-dev-server --mode development",
      "build": "webpack --mode production"
    }
  ```
  
  개발용 webpack으로 서버를 열때는 webpack-dev-server 명령을 통해서 동작을 시킨다. 그리고 뒤에 —mode 옵션을 추가하고 옵션 값으로 development를 넣어준다. 현재 개발모드라는 것을 명시해 준 것이다.
  
  제품용은 webpack 명령어의 mode 값으로 production을 넣어준다.   
  제품모드라는 것을 명시한 것이다.
  
  - webpack
    
    번들러가 동작하기 위한 핵심적인 패키지
    
  - webpack-cli
      
    커맨드 라인 인터페이스(터미널에서 입력하는 여러가지 명령들)를 지원하는 패키지. parcel에서는 기본적으로 지원했던 것이지만 webpack은 따로 설정해야 했기에 해당 패캐지를 설치한 것이다. 
    
    해당 패키지 설치로 터미널에서 webpack 명령어 사용이 가능해진다.
      
  - webpack-dev-server
    
    dev 명령어를 통해서 개발서버를 오픈할 때(저장) 페이지를 바로 새로고침할 수 있도록 해주는 패키지이다.
    
    해당 패키지도 터미널에서 명령이 동작해야 하기 때문에 webpack-cli에 영향을 받는다. 그래서 패키지를 설치할 때 webpack-cli와 버전이 일치시켜야 했던 것이다.
        
- webpack.config.js
  
  parcel과는 다르게 서버를 열어주려면 구성파일을 제공해줘야 한다.  
  webpack.config.js 라는 이름의 파일을 만들어 준다.
  
  해당 파일에 webpack의 구성옵션들을 작성해주면 된다.  
  parcel은 구성옵션을 따로 정리할 필요가 없이 자동으로 되어있었지만 webpack은 자유도가 높은만큼 개발자가 따로 구성옵션을 작성해서 사용해야 한다.