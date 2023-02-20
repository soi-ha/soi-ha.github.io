---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: BT, Container 사용자 지정 및 로딩 애니메이션 ..."
tags:
  - TIL
  - Vue.js
  - project
  - Bootstrap
---

## __Container 너비 사용자 지정__
---

> [BootStrap: Containers](https://getbootstrap.com/docs/5.3/layout/containers/)

```scss
$container-max-widths: (
  sm: 540px,
  md: 720px,
  lg: 960px,
  xl: 1140px,
  xxl: 1320px
);
```

bootstrap의 containers 내용에서 sass 부분을 확인하면 container의 너비가 지정되어져 있는 것을 볼 수 있다.   
해당 내용을 복사하여 src 폴더의 scss 폴더 내부의 main.scss 파일에 붙여넣는다. 그리고 원하는 너비로 변경해주면 된다.

**변경한 너비**
```scss
$container-max-widths: (
	sm: 540px,
  md: 704px,
  lg: 924px,
  xl: 1140px,
  xxl: 1364px
);
```

해당 내용을 variables 파일에 덮어써져서 사용된다.

## __에러 메세지 출력과 로딩 애니메이션__
---

### __로딩 애니메이션__
  
  > [BootStrap: Spinners](https://getbootstrap.com/docs/5.3/components/spinners/)
  
  class 이름으로 `"spinner-border text-primary"` 만 작성하면 간단하게 로딩 애니메이션을 사용할 수 있다.
  
  - 검색 시작과 끝에만 사용하기
    
    검색을 시작하는 부분인 searchMovie의 가장 윗부분에 loading 상태가 true가 되도록 작성한다.  
    loading 상태는 catch 구문이 실행되던 안되던 간에 무조건 끝나야 하기 때문에 finally를 통해 loading이 false가 되도록 한다.
      
  - 중복실행 방지
      
    다음과 같은 코드를 추가하여 loading 값이 true일때 return을 실행하게 하여 loading 값을 false로 반환하도록 한다.
    
    ```jsx
    if (state.loading) {
      return
    }
    
    commit('updateState', {
      message: '',
      loading: true
    })
    ```
    
    최초의 searchMovies는 loading값이 false이기 때문에 if 조건문이 실행되지 않고 아래의 로직이 실행되게 된다.   
    그러나, 최초의 searchMovies가 아직 실행되고 있는 상태(loading: true)에서 또 검색 버튼을 누르는 등의 행위를 통해 searchMovies 부분을 또 실행시키게 되면 loading 값은 아직 true인데 if 조건문으로 인해 retrun 값이 반환되면서 loading이 false상태가 되는 등의 에러가 발생하게 된다.  
    그렇게 되면 함수가 종료되면서 밑의 로직은 동작하지 않게 된다.
    
    이렇게 간단한 if 조건문을 통해서 사용자가 searchMovies를 여러번 실행하는 것을 방지하게 한다.
    
    그리고 정상적으로 로직이 종료되게 되면 finally를 통해 무조건적으로 loading은 false로 변하게 된다.  
    정상적으로 로직이 종료되면 다시 searchMovies를 정상적으로 실행이 가능하게 된다. loading이 false가 되었으니 if 조건문에 걸리지 않게 되기 때문이다.
    
### __에러 메세지 출력__
  
  에러 메세지가 출력되는 부분에 다음과 같은 코드를 추가한다.
  
  ```jsx
  :class="{ 'no-result': !movies.length }"
  ```
  
  해당 코드는 class: no-result를 추가하는 속성이다.  
  movies의 length(길이) 즉, 검색된 영화가 없을때 해당 class 이름을 추가하도록 한다. 
  movie.js 의 _fetchMovie의 catch에서 error가 발생하면 error 문구를 출력하도록 하는데, 검색된 영화가 존재하지 않는다는 error문구가 이때 출력된다.