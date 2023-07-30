---
layout: post
categories:
  - TIL
title: 'CSS: textarea 하단 줄 스타일 없애기(resize)'
tags:
  - TIL
  - CSS
---

`<textarea>` 태그에는 기본적으로 우측 하단에 줄 세개가 존재한다. 이번 프로젝트를 진행하면서 디자인에 해당 줄은 들어가지 않기 때문에 줄을 없애는 방법을 서칭했고 찾아냈다.  
방법은 매우매우매우 간단했다.

```css
.textarea {
	resize: none;
}
```

해당 속성만 입력해주면 줄이 깔끔하게 사라지는 것을 볼 수 있다.

- `resize:none;` 적용 전

  <img width="617" alt="textarea 기본 스타일" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/ce6505ad-2e6c-4014-97f9-e3a2cf5c6682">

- `resize:none;` 적용 후

  <img width="605" alt="textarea 줄 없애고 난 후" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/68752088-da8d-4cd9-98d3-753a47450723">

프로젝트를 통해 새로운 것을 또 알아간다..!

> 참고  
> [(즐거웅코드) CSS textarea 우측 하단 세줄 표시 없애기](https://ungdoli0916.tistory.com/680)
