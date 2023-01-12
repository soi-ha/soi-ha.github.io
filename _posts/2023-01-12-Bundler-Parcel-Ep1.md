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
    // package.json 파일
    "staticFiles": {
        "staticPath": "static"
      }
    }
    ```
    
    해당 코드는 parcel-plugin-static-files-copy 패키지를 통해서 static 폴더를 dist 폴더로 복붙해주게 된다.
    
    루트경로에 static 폴더를 생성해준 후, favicon.ico 파일을 옮겨주면 끝이다.