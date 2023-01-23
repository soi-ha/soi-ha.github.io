---
layout: post
categories:
  - TIL
title: "Bundler: Webpack Ep.3"
tags:
  - TIL
  - Bundler
  - Webpack
---

## __Autoprefixer(PostCSS)__
---

autoprefixer: 공급업체 접두사를 자동으로 만들어 주는 것

- 패키지 설치
  
  ```bash
  npm i -D postcss
  npm i -D autoprefixer
  npm i -D postcss-loader
  ```
  
  - postcss: 스타일의 후처리를 도와주는 패키지
  - autoprefixer: postcss 안에서 공급업체 접두사를 자동으로 붙여주는 플러그인
  - postcss-loader: webpack에서 위 플러그인이 동작해야 하기 때문에 postcss를 동작시켜줄 수 있는 loader
- webpack.config.js
  
  스타일을 sass-loader를 통해서 해석을 해야지만 제대로 해석이 될 것이다.  
  해석된 내용을 postcss-loader를 통해서 공급업체 접두사를 적용한다. 혹은 그 외의 postcss 플러그인들을 적용할 수 있다.  
  다음에 이 부분의 내용을 css-loader로 읽어 드려서 마지막에 style-loader로 index.html에 style 태그로 삽입해준다.
  
  ```jsx
  module: {
      rules: [
        {
          test: /\.s?css$/,
          use: [
            'style-loader',
            'css-loader',
            'postcss-loader',
            'sass-loader'
          ]
        }
      ]
    }
  ```
    
- package.json
  
  현재 우리 프로젝트가 어떤 대상의 브라우저를 지원하는지 명시한다.
  
  ```json
  "browserslist": [
      "> 1%",
      "last 2 versions"
    ]
  ```
    
- .postcssrc.js 파일 생성
  
  구성옵션의 내용은 parcel과 동일하다.   
  module.exports를 통해서 할당된 내용을 밖으로 내보낸다.  
  plugins 옵션에 postcss의 plugins로 사용할 autoprefixer 패키지를 require 함수를 통해 가져와서 직접적으로 연결해준다.
  
  ```jsx
  module.exports = {
    plugins: [
      require('autoprefixer')
    ]
  }
  ```

## __babel__
---

- 패키지 설치
  
  ```bash
  npm i -D @babel/core @babel/preset-env @babel/plugin-transform-runtime
  ```
    
- .babelrc.js 생성
  
  ```jsx
  module.exports = {
    presets: ['@babel/preset-env'],
    plugins: [
      ['@babel/plugins-transform-runtime']
    ]
  }
  ```
  
  presets: 우리가 일일이 명시해야 하는 자바스크립트 기능을 한번에 지원해주는 패키지
  
  plugins: 비동기 처리를 위해서 plugin-transform-runtime 패키지 설치.  
  plugins는 배열안에 배열이 들어가 있다. 이것을 2차원 배열이라고 부르기도 한다.
    
- webpack.config.js
  
  .js로 끝나는 모듈을 매칭해서 그 부분에 패키지들을 사용할 것이다.
  해당 패키지는 babel-loader이며, js로 끝나는 확장자들을 webpack에서 babel-loader로 읽어들여서 실제로 babel이 적용될 수 있게 한다.
  
  ```jsx
  module: {
      rules: [
        {
        },
        {
          test: /\.js$/,
          use: [
            'babel-loader'
          ]
        }
      ]
    }
  ```
    
- babel-loader 패키지 설치
  
  ```bash
  npm i -D babel-loader
  ```

## __Netlify 배포__
---

- github에 배포하기 원하는 폴더 업로드 (해당 폴더만 존재하는 repository로)
- netlify에 사이트 생성을 클릭한 후, 배포하기 원하는 레퍼지토리 선택
- npm 프로젝트는 일반 배포와 다르기 때문에 옵션을 선택해야 한다.
  - Owner, Branch to deploy: 설정 변경할 필요 없음
  - Basic build settings
  Build command ⇒ npm run build  
  Publish directory ⇒ dist/   
  제품화 시킬 명령어를 작성하고 제품화된 것이 어떤 폴더에 들어가져 있는지를 명시한다. 기본적으로는 dist 폴더지만 만약 폴더 명을 다른 것으로 했다면 변경한 폴더명으로 작성해준다.

## __NPX, Degit__
---

parcel, webpack 템플릿을 터미널을 통해서 손쉽게 다운로드 하기

- 다운 받기를 원하는 경로로 이동
- npx degit
  
  degit 명령을 통해서 깃허브 저장소에 있는 특정한 저장소를 현재 경로로 다운로드 받는다. degit 명령어를 사용하려면 degit을 다운로드해야 하는데, 설치하지 않고 동작시킬 수 있도록 npx 명령어를 사용한다.
  
  npx는 node.js에서 쓸 수 있는 명령어이다. node와 npm을 다운했기에 npx도 사용이 가능하다.
  
  ```bash
  npx degit 불러오고자-하는-깃허브-저장소 다운받은-저장소-담을-폴더명
  ```
    
- 다운받은 경로를 확인해보면 명시했던 이름으로 새로운 폴더가 만들어진 것을 확인할 수 있다.
- 해당 방법으로 템플릿을 다운받을 때의 장점
  
  해당 방법으로 다운로드하면 버전관리가 전혀되어있지 않은 상태이다.  
  즉, git init을 하지 않을 상태! 그렇기 때문에 git init을 통해서 처음부터 다시 버전 관리를 할 수 있다. 
  
  degit은 버전관리가 되어있지 않은 상태로 불러오고, git clone은 해당하는 저장소가 가지고 있는 버전의 내역까지 불러온다.
  
  따라서 해당 템플릿(폴더 내용)을 가지고 새로운 프로젝트를 만들 것이라면, git clone 명령은 적합하지 않다. 이럴때 degit 명령어가 유용하게 사용된다.