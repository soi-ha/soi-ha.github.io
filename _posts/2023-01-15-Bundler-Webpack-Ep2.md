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

## __module__
---

- 별도의 파일 관리
  
  css 폴더를 static 안에 만들어서 관리한다.  
  정적 파일 연결에서 static 폴더 안에 있는 내용들은 dist 폴더에 들어가기 때문에 실제로 실행했을 때, 경로를 css 폴더 안의 main.css 파일 찾기로 해도 잘 출력이 되는 것을 확인할 수 있다.
  
  ```html
  <link rel="stylesheet" href="./css/main.css">
  ```
    
- webpack을 활용한 파일 관리
  
  css 폴더를 루트 경로로 옮긴다.
  
  main.js 파일에 다음과 같은 코드를 작성한다.
  
  ```jsx
  import '../css/main.css'
  ```
  
  import를 통해 main.css 파일을 불러온다. 변수명과 from은 사용하지 않아도 된다. 변수에 담아도 사용할 수 없기 때문이다. 그렇기에 단순하게 파일만 불러와도 된다.
  
  우리가 webpack의 진입점으로 설정해 놓은 것은 main.js 파일. 따라서 webpack은 무조건 main.js 파일부터 읽어나가기 시작한다.  
  이때, main.js에 css 파일이 연결되어져 있으니 webpack이 main.js와 연결된 main.css도 읽을 수 있게 된 것이다.
  
  webpack이 main.css를 읽어서 index.html과 섞어서 다시 dist 폴더에 내어주는 구조가 된다.  
  어떻게 보면 복잡한 구조지만 해당 구조로 다른 많은 것들을 실행할 수 있기 때문에 해당 방법을 사용하는 것이 좋다.
  
  단, 해당 방법은 문제점이 존재하는데 webpack은 css 파일을 읽을 수 없다는 것이다. 그래서 우리는 따로 패키지를 설치해줘야 한다.
    
- CSS 파일 리딩 패키지 설치
  
  ```bash
  npm i -D css-loader
  npm i -D style-loader
  ```
    
- module 옵션
  
  ```jsx
  module: {
      rules: [
        {
          test: /\.css$/,
          use: [
            'style-loader',
            'css-loader'
          ]
        }
      ]
    }
  ```
  
  test에는 정규표현식을 작성한다. 해당 정규표현식으로 .css 로 끝나는 확장자를 찾는다.   
  $의 의미는 해당 기호 앞에 있는 문자로 끝나는 문자를 찾는 것을 말한다.
  
  use의 경우에는 배열 데이터로 작성한다. 해당 배열 데이터 안에 설치한 패키지 이름을 작성한다. 
  
  위 내용을 종합하자면, .css 확장자를 가지고 있는 모든 css 파일은 test라는 속성으로 해당하는 내용을 매칭해서 작성한 패키지들을 사용(use)한다는 의미이다.
  
  - 패키지의 역할과 작성 순서
    
    먼저 해석되는 것은 css-loader이다.  
    css-loader는 main.js에서 import 키워드를 통해 main.css 파일을 가져오고 있는데 js에서는 css를 해석할 수 없으니 해당 내용을 해석할 수 있게끔 해주는 패키지이다.
    
    style-loader는 해석된 내용을 사용하는 패키지로, html의 style 태그 부분에 style-loader가 해석된 내용을 삽입해 주는 역할을 한다.
    
    작성 순서는 style-loader를 먼저 작성해줘야 한다.

## __SCSS__
---

작성했던 css들을 모두 scss로 변경한다. 

단, webpack.config.js 파일에서 정규표현식 부분에 해당 내용을 추가한다.

```js
{
  test: /\.s?css$/,
  use: [
    'style-loader',
    'css-loader'
  ]
}
```

s뒤의 물음표의 의미는 s가 있을 수도 있고, 없을 수도 있다는 것이다.   
혹시라도 우리가 css 파일을 연결하게 되었을 때 발생할 문제를 방지하기 위함이다.  
따라서, 해당 정규표현식으로 css, scss 파일 모두 찾을 수 있다.

하지만 해당 패키지들로는 scss 파일을 읽을 수가 없다. 추가 패키지를 설치해야 한다.

- scss 리딩 패키지 설치
    
  ```bash
  npm i -D sass-loader sass
  ```
    
- 패키지의 역할 및 순서
  
  sass-loader를 통해서 webpack에서 해당하는 css 파일을 읽어낼 수 있다.  
  sass는 실제로 해당 내용을 읽을 때, 문법을 해석해 줄 수 있는 실제 sass 모듈이다.
  
  패키지의 순서는 sass-loader가 가장 먼저 읽어져야 하기 때문에 css-loader 다음에 둔다.
  
  ```jsx
  module: {
      rules: [
        {
          test: /\.s?css$/,
          use: [
            'style-loader',
            'css-loader',
            'sass-loader'
          ]
        }
      ]
    }
  ```