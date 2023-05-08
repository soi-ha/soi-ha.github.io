---
layout: post
categories:
  - TIL
title: 'UMC: 마켓컬리 클론코딩, 라이프사이클'
tags:
  - TIL
  - UMC
---

## **useEffect를 사용하여 팝업창 띄우기**

### **팝업창 만들기 (스타일)**

---

팝업창은 position을 fixed로 설정하여 스크롤을 해도 화면에 고정되도록 한다.

z-index를 사용하여 화면의 최상단에 위치하도록 한다. (z-index는 position 값이 존재해야 적용된다. 단, position:static은 불가)

<img width="278" alt="스크린샷 2023-05-04 00 49 26" src="https://user-images.githubusercontent.com/77609591/236725855-07b6c95b-ee33-452c-9d28-a09c742ee926.png">

### **팝업창 만들기 (기능)**

---

팝업창의 닫기와 오늘 하루 안 보기 기능을 만들기 위해서는 localStorage에 대해 알아야 한다.

- localStorage

  localStorage는 웹 스토리지 객체이다. 브라우저 내에서 키-값 형태로 저장할 수 있다.
  sessionStorage는 페이지를 새로고침하기 전까지의 정보를 저장하고, localStorage는 브라우저를 다시 실행해도 정보를 저장한다.

  - `setItem()`

    localStorage에 아이템을 추가할 때 사용한다. 즉, 데이터를 저장한다.

    ```jsx
    localStorage.setItem(key, value);
    ```

  - `getItem()`

    localStorage의 아이템을 읽기 위해서 사용한다. 즉, 데이터를 불러온다.

    ```jsx
    localStorage.getItem(key);
    ```

  - `removeItem()`

    localStorage에 있는 아이템을 삭제한다. 데이터를 골라서 삭제한다.

    ```jsx
    localStorage.removeItem(key);
    ```

  - `clear()`

    localStorage에 있는 모든 데이터를 지운다.

    ```jsx
    localStorage.clear();
    ```

    > [[Javascript] localStorage 사용법 (읽기, 쓰기, 삭제, 키목록 등)](https://hianna.tistory.com/697)  
    > [[React] #15. 리액트에서 간단하게 로그인 구현해보자](https://velog.io/@exoluse/React-15.-리액트에서-간단하게-로그인-구현해보자)

localStorage를 사용하여 브라우저를 다시 실행해도 정보를 저장하도록 한다. `localStorage.setItem`으로 데이터를 저장하는데, key는 homeVisited로 value는 팝업창을 띄운 시간에 24시를 더한 값(expires)이다.

<img width="565" alt="스크린샷 2023-05-04 01 39 37" src="https://user-images.githubusercontent.com/77609591/236725864-6bd2cc84-2426-44ed-b452-93e38625e7e4.png">

<img width="976" alt="스크린샷 2023-05-04 01 38 52" src="https://user-images.githubusercontent.com/77609591/236725863-7b5a3314-b215-4fd9-b143-c7e4cb5817a1.png">

useState를 통해 state를 관리한다.  
기본값은 false로 setShowMainPop(false)가 되면서 팝업창 노출이 없다.  
만약, 기본 값을 true로 할 경우, ‘오늘 하루 안보기’를 클릭하여도 리렌더링을 하면 팝업창이 나타난다.

HOME_VISITED 정수에 localStorage에서 homeVisited를 가져온다.

useEffect를 사용하여 ‘오늘 하루 안보기’ 버튼을 클릭하면, 리렌더링을 하여도 팝업창이 뜨지 않도록 한다.

today는 오늘의 시간(즉, 현재 시간)을 담은 정수이다.

handleMainPop은 화살표 함수에서 실행한 값을 저장한다.  
만약, localStorage에 저장된 시간이 존재(true)하고 현재 시간(today)이 localStorage의 시간보다 작다면(true) return을 한다.  
이때, return은 useState에서 기본값으로 세팅한 false이다. 즉, setShowMainPop(false)가 되면서 팝업창 노출이 없다.

_localStorage의 시간: 이전에 팝업창이 떴을 때, ‘오늘 하루 보지 않기’를 클릭한 시간 + 24시간_

다른 조건으로는, 만약 localStorage에 저장된 시간이 없거나(false) localStorage에 저장된 시간이 현재 시간(today)보다 작다면 setShowMainPop(true)를 실행하여 팝업창이 노출된다.

window.setTimeout에 실행할 함수는 handleMainPop이며 지연 시간은 0.1초이다. 0.1초 후에 해당 함수를 실행하도록 한다.

_setTimeout()은 코드를 바로 실행하지 않고 일정 시간을 기다린 후에 실행할 때 사용한다. 첫번째 인자는 실행할 코드를 담은 함수, 두번째 인자는 지연 시간(ms)_

> ❤️‍🔥 **한 줄 요약**  
> setShowMainPop(false) → 팝업창 노출 X  
> setShowMainPop(true) → 팝업창 노출 O  
> localStorage를 통해 ‘오늘 하루 안 보기’를 클릭한 시간 저장

> 참고  
> [[React] 오늘 하루 안보기/ 24시간 동안 안보기 팝업 구현](https://jinist.tistory.com/76)  
> [자바스크립트의 setTimeout()과 setInterval() 함수](https://www.daleseo.com/js-timer/)  
> [React 'n일 동안 보지 않기' 팝업 만들기](https://darrengwon.tistory.com/1100)

### **종합 소감**

마켓컬리 클론코딩에 어떤 라이프사이클을 적용할 지 고민을 많이 했다.  
그래서 라이프사이클을 통해 팝업창을 띄우기로 했다.
사실 직접 만들어서 해보려고 했지만... 나의 능력 부족으로 인해 ㅎ.. 다른 분의 코드를 참고하여 작성하였다.
대신의 해당 코드를 면밀히 이해하려고 했다. 그래서 내가 작성한 코드는 아니지만 어느정도 나의 지식으로 습득할 수 있었다.
