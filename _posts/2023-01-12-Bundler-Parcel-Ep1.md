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