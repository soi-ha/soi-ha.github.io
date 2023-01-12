---
layout: post
categories:
  - TIL
title: "Bundler: Parcel Bundler Ep.1"
tags:
  - TIL
  - Bundler
  - Parcel
---
## __번들러 개요__
---

**번들러 (Bundler)**

html, css, js를 가지고 웹을 만드는데 이것만으로 만들기에는 비효율적인 면이 있다. 그래서 우리는 typescript, sass, react, svelte 등과 같은 것을 이용해서 코딩을 한다.  
이 기능을 이용해서 웹을 만들려면 변환을 해줘야 하는데 이때 우리는 Parcel과 같은 번들러를 사용해서 변환을 해준다.  

번들러는 변환하기 위한 패키지를 모아둔 것으로 sass, postcss, typescript 등을 하나하나 설치해서 변환을 해줘야 하는데 이것을 하나로 묶어서 관리하는 것이 번들러이다.  

**Parcel과 Webpack의 차이점**

- Parcel
  
  구성 없는 단순한 자동 번들링. 소/중형 프로젝트에 적합하다.
  
- Webpack
  
  매우 꼼꼼한 구성. 중/대형 프로젝트에 적합하다.

## __정적 파일 연결__
---

- favicon.ico 파일 만들기
    
  [ICO Converter Link](https://www.icoconverter.com/)
  
  해당 링크에서 ico 파일로 만들기를 원하는 이미지를 올려 convert하면 ico 파일을 만들 수 있다.
  
  만든 파일을 favicon으로 잘 실행되는지 보면 실행되지 않는다. (favicon.ico 파일은 현재 루트경로에 존재
    
- favicon 자동으로 dist 폴더에 생성하기
  
  dist 폴더를 확인하면 우리가 작성했던 파일들이 변환되어져 있는 것을 확인할 수 있다. 그러나 dist 폴더에 favicon 파일은 존재하지 않는다.  
  dist 폴더에 파일을 이동하면 간단하게 해결되지만, dist 폴더는 parcel bundler의 개발서버 실행을 통해서 생성되고 삭제되기 때문에 dist 폴더로 파일을 그냥 옮기는 것은 좋지 않다.  
  그렇기에 favicon.ico 파일이 dist폴더에 자동으로 실행하게 하는 방법이 가장 옳은 방법이라고 본다.
  
  - **parcel plugin static files copy 패키지**
      
    favicon.ico 파일을 dist 폴더에 생성하기 위해서 패캐지의 도움을 받을 것이다.  
    google에 parcel plugin static files copy로 검색하여 해당 사이트에 접속한다.  
    [parcel-plugin-static-files-copy Link](https://www.npmjs.com/package/parcel-plugin-static-files-copy)
    
    ```bash
    # parcel plugin static files copy 설치
    npm i -D parcel-plugin-static-files-copy
    ```
    
    package.json 파일 최하단에 해당 코드를 추가한다.
    
    ```json
    "staticFiles": {
        "staticPath": "static"
      }
    ```
    
    해당 코드는 parcel-plugin-static-files-copy 패키지를 통해서 static 폴더를 dist 폴더로 복붙해주게 된다.
    
    루트경로에 static 폴더를 생성해준 후, favicon.ico 파일을 옮겨주면 끝이다.

## __autoprefixer__
---

  <img width="291" alt="설명 사진" src="https://user-images.githubusercontent.com/77609591/212019936-3dfc4398-3c26-4dfa-b7a8-391fa8ce901d.png">

- ~~display: —webkiy-box~~; 같은 것들은 무엇일까?
    
  우리가 알고 있는 웹의 표준이 권고안으로 나오기 전에 어느정도 기술 범위가 공개가 된다. 이런 부분들을 브라우저를 제작하는 벤더사(ms, google 등)에서 미리 차용하여 자신들의 브라우저에 동작할 수 있도록 구조를 만들어 둔다.  
  하지만 이런 기술들은 아직 표준기술은 아니기 때문에 webkit이나 ms 접두사를 붙여서 사용하는 방식을 제공한다.
  
  그러나, 일부 유저는 표준기술이 동작하지 않는 구형 브라우저를 사용하는 경우가 존재한다. 이런 경우에 `display: flex;` 같이 우리가 알고 있는 표준기술은 동작하지 않고, 예전에 접두사가 붙어 시험적으로 적용되었던 flex 기술들이 사용자의 구형 브라우저에서 동작을 할 수 있다. 
  
  특정 브라우저에서 표준 동작이 가능하면 접두사가 붙은 다른 동작들은 작동하지 않게 되어 취소선 표시가 되어 있는 것이다.  
  반대로, 표준기술이 동작할 수 없는 브라우저라면 접두사 붙은 코드들이 동작하고 표준 기술에 취소선 표시가 존재하게 된다.  
  
  비교적 신기술이 적용되지 않는 구형 브라우저에서도 최신의 css 기술이 동작할 수 있도록 일종의 보험을 들어두는 방법이 webkit, ms 같은 접두사를 붙여 사용하는 것이다. ms, webkit 등을 **공급 업체의 접두사 (Vender Prefix)**라고 한다.
  
  그러나 우리가 공급 업체의 접두사를 각 속성부분에 어떻게 적용할 지 모두 외워서 사용할 수 없다. 이것을 자동으로 붙여주는 패키지가 존재한다. 이 패키지가 **autoprefixer**이다.
    
- autoprefixer 설치
  
  ```bash
  # postcss 설치
  npm i -D postcss
  
  # autoprefixer 설치
  npm i -D autoprefixer
  ```
  
  package.json 파일 최하단에 추가 코드 작성. 
  
  ```json
  "browserslist": [
    "> 1%",
    "last 2 versions"
  ]
  ```
  
  browserslist 옵션은 현재 NPM 프로젝트에서 지원할 브라우저의 범위를 명시하는 용도이다. 이 명시를 autoprefixer 패키지가 활용하게 된다.
  
  해당 코드의 의미는 browsersList 옵션을 통해서 `> 1%` 전 세계의 점유율 1퍼센트 이상인 모든 브라우저의 `last 2 versions` 두개 버전까지는 지원을 하겠다는 의미이다.
  
  해당 옵션을 꼭 작성을 해줘야 한다. 이 옵션을 통해 설치한 패키지들이 css 속성에 공급업체 접두사를 붙여주는 기능을 구현할 수 있다.
    
- .postcssrc.js 생성
    
  뒤에 rc (Runtime Configuration)가 붙은 파일은 구성 파일을 의미한다.
  파일 이름 앞에 마침표를 붙여준다는 것을 구성옵션이라던가 숨김파일을 의미한다.
  
  해당 이름으로 파일을 만들어 줘야 parcel bundler가 구성옵션으로 찾아서 사용할 수 있다.
  
  우리가 기존에 내보내기 불러오기를 할때 import, export 키워드를 사용하는데 이 방식을 ESM (ECMA Script module)이라 부른다. ECMA Script는 js의 표준 명칭이고 거기서 사용하는 자바스크립트의 모듈 개념이라는 의미이다.
  
  node.js에서는 ESM을 지원하지 않고 **CommonJS 방식**을 제공한다.
  그렇기에 node.js에서는 import 대신에 `require()`, export 대신에 `module.exports` 를 사용한다.
  
  .postcssrc.js는 어떤 내용들을 번들러를 통해서 변환하는 용도로 사용하는 것이기 때문에 브라우저에서 동작하는 개념이 아닌, node.js 환경에서 동작하는 개념이다. 그래서 CommonJS 방식을 사용해야 한다.
  
  ```jsx
  // ESM
  // CommonJS
  
  // import autoprefixer from 'autoprefixer'
  const autoprefixer =  require('autoprefixer')
  
  // export {
  //  plugins: [
  //    autoprefixer
  //  ]
  // }
  module.exports = {
    plugins: [
      autoprefixer
    ]
  }
  ```
  
  더욱 간단하게 간소화시키기  
  autoprefixer를 따로 변수에 담지 않고 plugins 배열 데이터에 바로 담아서 사용한다.
  
  ```jsx
  module.exports = {
    plugins: [
      require('autoprefixer')
    ]
  }
  ```
  
- **PostCSS plugin autoprefixer requires PostCSS 8. 에러**
  
  지금까지의 과정대로 하고 실행을 해보면 해당 에러가 발생한다.   
  postcss의 플러그인 autoprefixer가 필수적으로 postcss의 8버전이 필요하다는 에러이다.
  
  package.json을 확인하면 autoprefixer은 10버전, postcss는 8버전이다.  
  두개의 버전이 달라서 충돌이 일어나게 되었다. (생각보다 흔히 일어난다.)
  이 충돌을 해결하는 방법을 둘이 호환이 되도록 버전을 맞춰주는 것이다.  
  autoprefixer의 10버전을 9버전으로 낮춰서 postcss의 8버전과 호환되도록 다운그레이드를 진행해준다.
  
  ```bash
  # autoprefixer 9버전으로 다운그레이드 
  npm i -D autoprefixer@9
  ```
    
- 대부분의 프론트엔드 개발을 할 때 일종의 스타일 보험 형태로 postcss와 autoprefixer을 적용하면 좋다.