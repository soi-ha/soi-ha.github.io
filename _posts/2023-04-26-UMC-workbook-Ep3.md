---
layout: post
categories:
  - TIL
title: 'UMC: 마켓컬리 클론코딩, React로 만들기'
tags:
  - TIL
  - UMC
---

## **Swiper 기능 넣기**

---

기존에 만들었던 함수를 작성하려고 했으나, 너무 복잡해지는 것 같아서 swiper 모듈을 설치하여 사용하기로 했다.

  <img width="643" alt="스크린샷 2023-04-12 00 05 05" src="https://user-images.githubusercontent.com/77609591/234813793-f22684e8-72ca-4c41-8ac8-2820cf610c55.png">

  <img width="709" alt="스크린샷 2023-04-12 00 05 02" src="https://user-images.githubusercontent.com/77609591/234813776-20f09c5a-a74c-4663-8711-de4fba56ae0f.png">
  
  - 구현할 기능
    
    5초 후에 자동으로 다음 배너 이미지로 변경하기,
    화살표를 클릭하면 앞, 뒤로 배너 이미지 바꾸기
      
  - swiper 설치
    
    ```bash
    npm i swiper
    ```
      
  - swiper 및 css 불러오기
      
    ```jsx
    import { Swiper, SwiperSlide } from 'swiper/react';
    import { Autoplay, Navigation, Pagination } from 'swiper';
    import 'swiper/css';
    import 'swiper/css/navigation';
    import 'swiper/css/pagination';
    ```
    나는 5초 뒤에 자동으로 다음 슬라이드로 변경되게 할 것이기 때문에 Autoplay를 불러왔다.  
    그리고 화살표를 클릭하여 슬라이드를 이동하게 할 것이기 때문에 Navigation과 Pagination을 불러왔다.
      
  - swiper 적용하기
    
    ```jsx
    export default function BannerContainer() {
      ...
      return (
        <div className="banner">
          <div className="banner-img">
            <Swiper
              modules={[Navigation, Pagination, Autoplay]}
              spaceBetween={50}
              slidesPerView={1}
              autoplay={{ delay: 5000 }}
              navigation
              pagination={{ clickable: true }}
              onSlideChange={() => console.log('slide change')}
              onSwiper={(swiper) => console.log(swiper)}
            >
              <SwiperSlide>
                <Banner {...banner1} />
              </SwiperSlide>
              <SwiperSlide>
                <Banner {...banner2} />
              </SwiperSlide>
              <SwiperSlide>
                <Banner {...banner3} />
              </SwiperSlide>
              <SwiperSlide>
                <Banner {...banner4} />
              </SwiperSlide>
              <SwiperSlide>
                <Banner {...banner5} />
              </SwiperSlide>
            </Swiper>
          </div>
        </div>
      );
    }
    ```
      
    이전 주차에 만들어뒀던 스타일이 존재했기 때문에 최대한 살리고자 `.banner` 와 `.banner-img` 박스는 그대로 두었다. 해당 박스 안에 Swiper 컴포넌트를 부르고 해당 컴포넌트안에 SwiperSilde를 원하는 갯수만큼 생성하면 된다.
    
    Swiper에서 보일 슬라이드의 갯수는 1(sildePerview), 5초 뒤에 자동으로 슬라이드 이동(autoplay), 버튼을 통해 슬라이드를 이동하는 것은 true(navigation, pagination)로 설정했다. 
    
    나는 기존에 배너 이미지들의 스타일이 존재했기 때문에 해당 부분은 최대한 살리고, img의 link값만 변경해주기 위해서, SwiperSilde안에 Banner 컴포넌트(내가 만들었던 배너 이미지)를 넣어주었다.  
    Banner이미지는 img태그만 존재하기 때문에, 결과적으로는 SwiperSlide 컴포넌트 안에는 img태그가 들어있는 것이다.
      
  - **Swiper를 적용하면서 문제점**

    1. css 파일을 가져올 때, 참고했던 문서들과는 불러오는 것이 달라서.. 에러가 엄청 떠서 약간 애를 먹었다..
    그래서 일단, navigation, pagination은 삭제하고 swiper의 css만 불러오니 잘 되길래, 공식 문서처럼 navigation과 pagination의 css를 불러왔더니 잘 적용되었다.

    2. img태그(Banner 컴포넌트)를 넣는 위치를 찾느라 시간이 좀 걸렸다.
    처음에는 그냥 SwiperSlide 컴포넌트 안에 {…banner1}를 하는 바람에 당연히 에러가 발생했고.. 태그 사이에 넣었더니 또 당연히 에러가 발생했었다.
    그러다가 Banner 컴포넌트를 다시 확인하고.. 생각해보니 Banner 컴포넌트를 모두 불러와서 banner1 (props)를 넣어줘야 내가 원하는 이미지의 링크가 잘 들어간 img 태그가 만들어지는 것을 깨닫고..! SwiperSlide 태그 사이에 Banner 컴포넌트를 넣었더니 잘 돌아갔다..!

- 참고자료
  > [Swiper React Components](https://swiperjs.com/react#styles)
  >
  > [[React] 리액트에서 Swiper.js 를 사용해보자. Swiper 슬라이드 사용예제, Navigation의 arrow 버튼 커스텀하기](https://joyful-development.tistory.com/35)
  >
  > [[#. React] React에서 swiper 사용하기, 배너 슬라이드 사용하기](https://developer0809.tistory.com/94)

## **useState를 사용하여, 버튼 클릭시 페이지 변환**

---

useSate를 사용해서 변환하려고 하고 있으나 문제 해결 못함…뭔가 로직을 잘 못짜고 있는 느낌..?!

Header 컴포넌트에서 마켓컬리 버튼을 클릭하면 마켓컬리 페이지(Market 컴포넌트)로, 뷰티컬리 버튼을 클릭하면 뷰티컬리 페이지(Beauty 컴포넌트)만 보이게 하고 싶은데..  
 Market과 Beauty를 출력하는 것은 App.js에 있고, 버튼은 Header 컴포넌트에 있으니까..? 두개를 어떻게 이어야 할 지 잘 모르겠음…!!!

### 🚨해결!🚨

이벤트 핸들러를 안 해줘서 적용이 안됐다..! ㅜ..  
내가 생각한 방식으로 useState 사용해서 페이지 전환이 되는 것은 맞았다!

  <img width="384" alt="스크린샷 2023-04-23 17 24 48" src="https://user-images.githubusercontent.com/77609591/234813782-3a87ddac-7ec0-46c2-be00-42699b9677e3.png">
  
  App.js에 다음과 같이 코드 작성한다.  
  Header 컴포넌트에 props를 넘겨준다. onClick 하면 onChange가 실행되고 해당 값에 맞춰 page(setPage)가 변환된다. page의 값이 market이면 Market 컴포넌트 보여주기, beauty라면 Beauty 컴포넌트를 보여준다.
  
  그리고 color에 들어오는 값이 market이라면 activeColor라는 이름의 클래스를 추가해준다. 값이 beauty일때도 마찬갖css에서 activeColor 클래스 추가시, 글자 색상이 보라색으로 작성했다.

  <img width="440" alt="스크린샷 2023-04-23 17 25 19" src="https://user-images.githubusercontent.com/77609591/234813790-54be0f84-4f2c-4fa3-a0b5-b69ebf6ef81b.png">

  <img width="592" alt="스크린샷 2023-04-23 17 25 23" src="https://user-images.githubusercontent.com/77609591/234813786-345b1fdc-efcb-46b1-bb6a-7e1379a1f29e.png">

## **종합소감**

---

이전에 만들었던 걸 React로 다시 하느라 애를 먹은 부분이 많았다...  
swiper라던가.. 페이지 변환이라던가...  
그래도 내가 짰던 코드들을 컴포넌트로 분리하는 작업을 하면서 어떻게 컴포넌트를 나누면 좋을지 생각해보고, 직접 활용하면서 이런게 컴포넌트구나!를 깨달았다. 역시.. 따라하는게 아니라 직접 고심해보고 해야.. 머리에 잘 남는 것 같다.  
그리고 state랑 props는 이론은 잘 알겠는데 실 적용을 못하겠었다. 그래서 특별 과외선생님(?)에게 물어봐서 실제 내 코드에 적용해보면서 조금은 이해가 되었다. 앞으로 react 쓸일은 많으니까 계속 만들어보면서 익혀야 겠다.
