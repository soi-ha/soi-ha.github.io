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