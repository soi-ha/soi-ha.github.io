---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: flex-shrink, flex-grow, 가운데 점( · ) 추가, 사진 해상도 변경"
tags:
  - TIL
  - Vue.js
  - project
  - css
---

### __flex-shrink__
    
  컨테이너의 크키가 부족해질 때, 아이템의 크기가 얼마나 줄어들 수 있는지를 나타낸다.  
  기본값은 1이다. 값이 1일때, 컨테이너 사이즈에 따라 자동으로 축소된다.  
  아이템의 크기가 축소되지 않게 하려면 값을 0으로 바꿔주면 된다.
    
### __flex-grow__
    
  컨테이너의 공간이 남을 경우 각 아이템들의 크기가 얼마나 더 할당이 가능한지 타나낸다.  
  만약 컨테이너 사이즈가 600px이고 아이템 1은 100px, 2는 200px일때, 컨테이너는 300px이 남는다. 아이템들에 flex-grow 값을 1은 1, 2는 2로 할당하면 남은 컨테이너 크키만큼 각 아이템들이 커진다. 따라서 1은 200px, 2는 400px이 된다.

---
    
### __가운데 점 ( · ) 추가하기__
    
  ```scss
  span {
    &::after {
      content: "\00b7";
      margin: 0 6px;
    }
    &:last-child::after {
      display: none;
    }
  }
  ```
  
  content의 “\00b7”은 css entities in numeric order이다.
  기호를 숫자로 나타낼 수 있도록 하는 것이다. 
  
  span태그 뒤에 가운데 점을 추가하고 싶기 때문에 가상요소 after을 통해 점을 만들어준다.
  
  맨 마지막 span 태그 뒤에는 점이 필요하지 않기 때문에 없애주기 위해 last-child를 통해 display를 none으로 만들어줌으로 점을 보이지 않게 한다.

---
    
### __사진 고해상도로 변경__
  
  실시간 이미지 리사이징
  
  서버에 저장이 된 특정한 이미지를 특정한 url 주소로 요청을 할때, 사이즈를 명시해서 요청을 전송한다. 실시간으로 이미지를 요청 받은 사이즈로 리사이징해서 사용자에게 전달한다.
  
  >[AWS Lambda@edge로 실시간 이미지 리사이징(updated)](https://heropy.blog/2019/07/21/resizing-images-cloudfrount-lambda/)
  
  이미지의 주소를 봤을 때, 특정 부분의 숫자를 변경해주면 더 큰 사이즈의 고해상도 이미지를 얻을 수 있다고 판단했을 때 사용이 가능하다.
  
  이미지의 뒷부분을 확인해 봤을 때 SX300이라고 되어있다. 300부분을 700으로 변경해보면, 700사이즈의 고해상도 이미지가 출력되는 것을 확인할 수 있다. 
  
  ```jsx
  methods: {
    requestDiffSizeImage(url, size=700) {
      return url.replace('SX300', `SX${size}`)
    }
  }
  ```
  
  script 태그에 methods를 추가하여 requestDiffSizeImage함수를 만든다. 해당 함수는 원하는 size를 넣어서 해당 size의 이미지로 변경하게 만들어주는 함수이다.
  
  replace를 통해서 SX300을 SX${size}로 변경해준다.  
  이때, 때마다 입력하는 사이즈를 다르게 변경해야하기 때문에 size 변수를 만든 것이다. size를 넣기 위해서는 보간을 통해 넣어야 한다.
  
  ```jsx
  :style="{ backgroundImage: `url(${requestDiffSizeImage(theMovie.Poster)})` }"
  ```
  해당 함수를 poster 이미지를 출력하는 부분에 넣어주면 원하는 사이즈(700)로 출력되는 것을 볼 수 있다.