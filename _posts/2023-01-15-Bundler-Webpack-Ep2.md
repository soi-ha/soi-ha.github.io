---
layout: post
categories:
  - TIL
title: "Bundler: Webpack Ep.2"
tags:
  - TIL
  - Bundler
  - Webpack
---

## __정적 파일 연결__
---

- 기본적인 파일 구성
  
  루트 경로에 static 폴더  
  favicon.ico 파일과 images 폴더  
  images 폴더 안에 logo.png 파일 
  
- index.html에 img 태그
  
  ```html
  <body>
    <img src="./images/logo.png" alt="Soha">
  </body>
  ```
  
  logo.png 파일을 찾으려면 static의 images 폴더 안으로 들어가는 경로로 해야 될텐데 static 폴더를 작성 안해도 잘 작동하는가? ⇒ Yes!
  
  npm run dev를 통해서 해당 파일들이 dist 폴더로 들어가게 되는데, 이때 logo.png 파일의 위치가 dist 폴더 안에서 images 폴더 안에만 존재하게 바뀌기 때문에 해당 경로로 작성해도 문제가 발생하지 않는다!
  
  다만, 저렇게 경로를 작성하기 위해서 구성옵션을 추가해야 한다.
    
- copy-webpack-plugin 패키지 설치
    
  ```bash
  npm i -D copy-webpack-plugin
  ```
    
- CopyPlugin
  
  ```jsx
  const CopyPlugin = require('copy-webpack-plugin')
  
  ...
  plugins: [
      new HtmlPlugin({
        template: './index.html'
      }),
      new CopyPlugin({
        patterns: [
          { from: 'static'}
        ]
      })
    ]
  ```
  
  plugins에 new 키워드를 통해서 생성자함수를 실행해주고 옵션으로 patterns를 추가하고 배열데이터를 넣어준다. 옵션의 첫번째 아이템으로 객체 데이터를 넣어주고 from 속성에 static이라는 우리가 만든 폴더 이름을 명시해준다.
  
  CopyPlugin을 통해서 static 안에 들어있는 내용이 copy되어 dist 폴더에 들어갈 수 있게 만들어 준다.
  
  from을 통해 어디서부터 복사해서 넣을 것인지 patterns 옵션에 넣어 설정한다. 이것이 배열이라는 것은 해당 형식으로 여러개 경로들을 명시할 수 있다.