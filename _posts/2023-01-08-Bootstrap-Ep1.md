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

## __버튼과 버튼 그룹 (button and botton group)__
---

### __Button__
다양한 색상의 버튼  
[Buttons Link](https://getbootstrap.com/docs/5.3/components/buttons/)
```html
<button type="button" class="btn btn-primary">Primary</button>
<button type="button" class="btn btn-secondary">Secondary</button>
<button type="button" class="btn btn-success">Success</button>
<button type="button" class="btn btn-danger">Danger</button>
<button type="button" class="btn btn-warning">Warning</button>
<button type="button" class="btn btn-info">Info</button>
<button type="button" class="btn btn-light">Light</button>
<button type="button" class="btn btn-dark">Dark</button>
<button type="button" class="btn btn-link">Link</button>
```

### __Button Group__
버튼들을 btn-group 클래스에 묶어서 넣으면 하나의 버튼 일렬로 출력된다.  
[Button group Link](https://getbootstrap.com/docs/5.3/components/button-group/)
```html
<div class="btn-group" role="group" aria-label="Basic example">
  <button type="button" class="btn btn-primary">Left</button>
  <button type="button" class="btn btn-primary">Middle</button>
  <button type="button" class="btn btn-primary">Right</button>
</div>
```
## __드롭다운과 리스트 (Dropdown and list)__
---

### __Dropdowns__
[Dropdowns Link](https://getbootstrap.com/docs/5.3/components/dropdowns/)
```html
<div class="dropdown">
  <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
    Dropdown button
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">Action</a></li>
    <li><a class="dropdown-item" href="#">Another action</a></li>
    <li><a class="dropdown-item" href="#">Something else here</a></li>
  </ul>
</div>
```

### __List Group__

[List group Link](https://getbootstrap.com/docs/5.3/components/list-group/)
```html
<ul class="list-group">
  <li class="list-group-item active" aria-current="true">An active item</li>
  <li class="list-group-item">A second item</li>
  <li class="list-group-item">A third item</li>
  <li class="list-group-item">A fourth item</li>
  <li class="list-group-item">And a fifth one</li>
</ul>
```

`aria-current`는 웹 접근성과 관련된 것으로 실제 기능 동작과는 연관되지 않은 부분이니 꼭 필요한 것이 아니라면 무시하면 된다.

`disabled` 클래스를 추가하면 해당 항목을 비활성화 될 수 있게 만들 수 있다.

```html
<div class="list-group">
  <a href="#" class="list-group-item list-group-item-action active" aria-current="true">
    The current link item
  </a>
  <a href="#" class="list-group-item list-group-item-action">A second link item</a>
  <a href="#" class="list-group-item list-group-item-action">A third link item</a>
  <a href="#" class="list-group-item list-group-item-action">A fourth link item</a>
  <a class="list-group-item list-group-item-action disabled">A disabled link item</a>
</div>
```
클래스에 `list-group-item-action`을 추가 작성하게 되면 hover나 disabled를 active 상태로 추가할 수 있다.  
즉, 마우스를 올렸을 때 동작 효과를 넣은 것이다.

## __양식 (Forms)__
---

### __Form__
[Forms Link](https://getbootstrap.com/docs/5.3/forms/overview/)

사용자에게 데이터를 얻는 양식을 말한다.  
boostrap에서 제공하는 양식을 통해 사용자에게 조금 더 깔끔한 형태로 데이터를 얻게 도움 받을 수 있다.

- form control
  
  **Sizing**  
  데이터를 입력받는 양식의 사이즈를 조절할 수 있다.
  
  **Disabled**  
  해당 속성을 통해서 입력받는 양식을 비활성화시킬 수 있다.
  
  **Readonly**  
  해당 속성을 통해서 읽기 전용 양식으로 만들 수 있다.  
  참고로, disabled랑 매우 유사하다. 그래도 약간 차이를 두고 사용하는 게 좋다.
  
  **File input**  
  버튼을 클릭해서 실제로 파일을 추가하고 삽입된 사항을 확인할 수 있는 양식도 존재한다. (매우 유용하게 사용할 듯)
  
- input group
  
  다양한 input 태그를 사용자가 보고 사용하기 편하도록 정리해 두었다.

## __모달 (Modal)__
---
해당 코드로 모달 창 내부에서 원하는 부분에 focusing 되도록 만들어준다.  
[Modal Link](https://getbootstrap.com/docs/5.3/components/modal/)

```jsx
const myModal = document.getElementById('myModal')
const myInput = document.getElementById('myInput')

myModal.addEventListener('shown.bs.modal', () => {
  myInput.focus()
})
```

```html
<!-- Button trigger modal -->
<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal">
  Launch demo modal
</button>

<!-- Modal -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalLabel">Modal title</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        ...
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

- button 태그의 data-bs-target 속성으로 원하는 모달의 id값을 입력하면, 버튼 클릭시 해당 모달창을 띄운다.
- modal-header에는 모달 창 내부의 header 내용
- modal-body는 내가 나타내고자 하는 핵심 내용
- modal-footer에는 주로 원하는 기능의 버튼 등


**Via JavaScript**  
오른쪽 사이드에 해당 목록을 클릭하면 모달에서 사용할 수 있는 자바스크립트의 기본적인 코드들을 확인할 수 있다.  
ex) show, hide …

**Events**  
다양한 이벤트를 통해서 모달의 상태를 제어할 수 있다.