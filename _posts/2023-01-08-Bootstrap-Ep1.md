---
layout: post
categories:
  - TIL
title: "BootStrap: CSS 프레임워크 Ep.1"
tags:
  - TIL
  - Bootstrap
  - CSS
  - Framework
---
# [Bootstrap](https://getbootstrap.com/)

## __CDN 프로젝트 생성__
---

### __CSS__
jsdelivr에서 bootstrap css 파일을 원격으로 제공한다.
해당 코드를 복사해서 붙여 넣어주기만 하면 된다.

```css
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-GLhlTQ8iRABdZLl6O3oVMWSktQOp6b7In1Zl3/Jr59b6EGGoI1aFkw7cmDA6j6gD" crossorigin="anonymous">
```

### __JS__
- bundle
  
  Bootstrap JS는 외부에서 Popper JS패키지를 가져와서 활용하고 있다.  
  bundle은 Popper 패키지와 bootstrap을 묶어서 파일을 제공한다.
  ```jsx
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.bundle.min.js" integrity="sha384-w76AqPfDkMBDXo30jS1Sgez6pr3x5MlQ1ZAGC+nuZB+EYdgRZgiwxhTBTkF7CXvN" crossorigin="anonymous"></script>
  ```
    
- Separate
  
  만약, 프로젝트에서 이미 Popper JS패키지를 사용하고 있다면 이중으로 사용할 필요가 없다.   
  그럴때, Popper 패키지와 묶이지 않은 별도의 Bootstrap 파일을 제공한다.
  
  위와 같이 Popper JS패키지와 Bootstrap을 따로 호출할 때, Poper JS파일을 먼저 호출해야 한다.
  ```jsx
  <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.11.6/dist/umd/popper.min.js" integrity="sha384-oBqDVmMz9ATKxIep9tiCxS/Z9fNfEXiDAYTujMAeBAsjFuCZSmKbSSUnQlmh/jp3" crossorigin="anonymous"></script>
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0-alpha1/dist/js/bootstrap.min.js" integrity="sha384-mQ93GR66B00ZXjt0YO5KlohRA5SY2XofN4zfuZxLkoj1gXtW8ANNCe9d5Y3eG5eD" crossorigin="anonymous"></script>
  ```

### __Popper__
Popper JS패키지는 팝업을 조금 더 쉽게 만들어 줄 수 있도록 도와준다.  
[Poper Home Link](https://popper.js.org/)