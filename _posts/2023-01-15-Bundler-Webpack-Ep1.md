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

## __entry, output__
---

[Concepts | 웹팩 Link](https://webpack.kr/concepts/)

webpack.config.js 파일은 브라우저에서 동작하는 것이 아니라 node.js 환경에서 동작한다. 우리가 저번에 봤던 것 처럼 파일을 내보낼 때 export가 아닌 module.exports를 이용해서 파일을 내보낸다.

여기서 entry와 output은 옵션이다.

- entry
  
  파일을 읽어들이기 시작하는 진입점을 설정하는 옵션이다.
  
  parcel의 경우 parcel index.html 작성하는 것으로 진입점을 설정할 수 있다.
  webpack의 경우에는 parcel처럼 cli 명령으로 하는 것이 아니라 구성옵션을 통해서 진입점 파일 설정을 해줘야 한다.
  
  entry 부분에 index.html 파일을 명시할 수 있겠지만, webpack은 기본적으로 html이 아닌 js를 진입점으로 사용한다.  
  그래서 우리가 현재 가지고 있는 js 폴더의 main.js 파일을 진입점으로 설정해준다.
  
  ```jsx
  module.exports = {
    // 파일을 읽어들이기 시작하는 진입점 설정
    entry: './js/main.js'
  }
  ```
    
- output
  
  entry 옵션을 통해서 읽어들인 파일의 기본적인 연결관계를 webpack이 분석을 해서 결과(번들)를 반환하는 설정하는 옵션이다.
  
  ```jsx
  // import
  const path = require('path')
  
  // export
  module.exports = {
    entry: './js/main.js',
    
    // 결과물(번들)을 반환하는 설정
    output: {
      path: path.resolve(__dirname, 'dist'),
      filename: 'main.js'
    }
  }
  ```
  
  - path
    
    경로. 어떠한 경로에 결과물을 만들어서 내어줄 것인지를 명시할 수 있다.  
    path 속성에는 node.js에서 요구하는 절대경로를 필요로 한다.
    
    우리는 node.js에서 언제든지 가져와서 사용할 수 있는 path라는 전역모듈을 require를 통해 가지고와서 path라는 변수에 할당을 한다.  
    이것을 아래쪽에서 사용하는데, path 변수에 resolve 메소드를 사용한다. resolve에는 첫번째 인수와 두번째 인수에 있는 기본적인 경로를 합쳐주는 역할을 한다. 
    
    이때 첫번째 인수의 __dirname은 node.js에서 전역적으로 사용할 수 있는 변수이다. (따로 설정 없이 언제든지 불러올 수 있다.)   
    __dirname은 현재 파일이 있는 그 경로를 지칭한다. 즉 webpack.config.js 파일이 있는 해당 경로가 __dirname의 경로인 것이다. 
    
    __dirname의 경로와 dist 폴더(두번째 인수)의 경로가 합쳐져서 절대경로가 만들어지는 것이다.
      
  - filename
    
    해당 결과를 main.js에 entry해서 읽어들이기 시작하는 파일의 이름과 동일하게 지정한다.
    
    dist 폴더에 main.js라는 파일이름으로, entry에서 진입점으로 사용한 main.js에 연결된 모든 내용들을 번들로 만들어서 합쳐서 내어준다.
      
  - **path와 filename의 default**
    
    두 옵션은 따로 명시해줄 필요가 없다.
    
    기본적으로 webpack은 path로 dist 폴더에 결과물을 만들어준다. 따라서 dist 폴더에 결과물을 넣을 것이라면 따로 입력하지 않아도 작동한다.
    
    filename의 경우 entry에 지정한 파일의 이름으로 만들어지기 때문에 이것 역시 따로 지정하지 않아도 된다면 해당 옵션을 명시하지 않아도 된다. 
      
  - 동작 확인
    
    내용이 잘 동작하는 지 확인한다.
    
    ```bash
    npm run build
    ```
    
    dev가 아닌 build를 통해 잘 동작하는지 확인을 해준다.
    
    처리가 완료되면 dist 폴더가 생성된다. 우리가 설정해뒀던 main.js 파일이 잘 들어와 있는 것을 확인할 수 있다.
      
  - **clean**
    
    우리가 내용을 바꿔서 저장한다면 어떻게 될까?
    
    폴더 이름은 public으로 변경하고 파일이름은 main.js로 설정한뒤 저장을 해준다. 그러면 public 폴더가 새로 생성되고 해당 폴더 안에 main.js 파일이 만들어진다.
    
    ```jsx
    module.exports = {
      entry: './js/main.js',
      output: {
        path: path.resolve(__dirname, 'public'),
        filename: 'main.js'
      }
    }
    ```
    
    폴더 명은 그대로 public으로 두고 파일 이름은 app.js로 변경하고 다시 저장하면 어떻게 될까?
    
    ```jsx
    module.exports = {
      entry: './js/main.js',
      output: {
        path: path.resolve(__dirname, 'public'),
        filename: 'app.js'
      }
    }
    ```
    
    public 폴더 안에 main.js 파일과 app.js 파일 두개가 동시에 존재하게 된다. 
    
    우리가 구성옵션을 바꾸게 되면 바꾸기 이전의 파일들이 그대로 존재하게 되는 문제가 발생하게 되었다. 이것을 해결하는 옵션이 clean 옵션이다.
    
    clean을 true로 설정하게 되면 기존에 만들었던 내용들을 제거하고 새롭게 설정한 내용만이 존재하게 된다.