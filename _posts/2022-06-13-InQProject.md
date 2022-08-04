---
layout: post
categories:
  - Project
title: "InQ Project Manager: 개발 동아리 2022 1학기 프로젝트"
tags:
  - InQ
  - project
  - InQ Project Manager
---
## __프로젝트 소개__
---
해당 프로젝트는 교내 개발 동아리 InQ에서 진행한 프로젝트이다.  
3월 17일에 첫 회의를 시작으로, 4월 30일에 프론트엔드 개발을 시작했다.  
프로젝트 이름은 __InQ Project Manager__ 이며, 동아리 활동을 통해 생기는 프로젝트를 관리해 주는 웹 페이지이다.  
## __초보의 프론트엔드 제작__
---
프로젝트 디자인부터 제작까지 모두 혼자 진행했다.   
웹 구조, 디자인, 프론트엔드 등.. 우리 팀에서 웹 제작이 가능한 사람이 프론트엔드 한명(나), 백엔드 한명이여서 한달동안 둘이서 제작을 완료할 수 있을 까? 걱정이 많았지만 어찌저지 포기할건 포기하고 살릴건 살리면서 제작을 완료하였다.   
일단 나는 미적인 감각은 정말 쥐똥만큼도 없기 때문에... 웹 디자인을 어떻게 할지 정말 걱정과 고민이 많았다. 그런 내가 선택한 최선의 방법은...! 이 프로젝트와 유사한 기존에 존재하던 서비스의 웹 디자인 참고와 내 취향인 웹의 디자인을 참고하여 제작했다.  
해당 사이트에 들어가서 __검사(F12)__ 계속 누르면서 html 구조를 어떻게 짰는지 확인하고, 참고하고, 내 방식대로 수정을 계속 거듭하면서 만들었다.  

<img width="706" alt="사이두 1" src="https://user-images.githubusercontent.com/77609591/173778367-80eff9ce-7d1c-4db1-b891-2f7eaf30f975.png" />
<img width="706" alt="사이두 2" src="https://user-images.githubusercontent.com/77609591/173778387-87db5cbd-54e3-4407-b3d8-fc36fe0853a5.png" />
<img width="706" alt="식스샵 1" src="https://user-images.githubusercontent.com/77609591/173778399-b1c8cb88-ba08-442b-8f06-4d5d1b56dcb7.png" />
<img width="706" alt="식스샵 2" src="https://user-images.githubusercontent.com/77609591/173778402-b4edfce2-c7ab-429e-a643-efb472a9764c.png" />
<img width="706" alt="식스샵 3" src="https://user-images.githubusercontent.com/77609591/173778411-0034a414-69b9-4b8b-898c-7ff5fed24eb7.png" />  

*참고한 사이트: 42DoProject, 식스샵*
### __InQ Project Manager 디자인 컨셉__
프로젝트 디자인 컨셉은 딱! 뭐다!는 없다. 일단 식스샵의 디자인이 어렵지 않으면서 심플하고 눈에 잘 들어오며 요즘의 웹 디자인이라 생각이 들어서 큰 디자인은 식스샵을 굉장히 많이 참고하면서 만들었다.  
확고한 디자인 컨셉은 없지만 시그니처 색은 꼭 있어야 한다고 생각했다. 마침 동아리 로고에 사용한 색들이 눈에 잘 띄면서 색 조합이 잘 어울렸기에 해당 색상을 프로젝트의 시그니처 색으로 사용하였다.

## __프로젝트 기능__
---
프로젝트의 기능을 크게 4가지로 나누면 다음과 같다.
- 로그인
- 회원가입
- 홈
- 프로젝트
### __로그인__
<img width="1512" alt="로그인" src="https://user-images.githubusercontent.com/77609591/173769166-9fe2a512-e4e0-4185-9835-823a84c977b2.png">  

아이디 및 비밀번호 입력후, 로그인 버튼 클릭시 로그인이 된다. 만약, 회원이 없다면 로그인되지 않는다. (로그인을 하지 않으면 홈으로 넘어가지 않음)
### __회원가입__
<img width="1493" alt="회원가입" src="https://user-images.githubusercontent.com/77609591/173769409-38492d1f-3f74-4959-a965-a03da5e9481e.png">

로그인 페이지에서 회원가입 버튼 클릭시, 회원가입 페이지로 넘어간다.  
회원가입 항목은 9개이다. * 가 붙은 항목들은 required를 통해 필수입력으로 했다. 
  - 이름 *
  - 아이디 *
  - 비밀번호 *
  - 비밀번호 확인 *
  - 이메일 *
  - 포지션 *  
  select를 사용하여 본인의 포지션 1가지를 선택하도로 하였다.
  - 보유기술 * 
  <img width="1512" alt="회원가입 - 보유기술 모달" src="https://user-images.githubusercontent.com/77609591/173769414-e69ae8c5-a61d-452f-938b-79b335269038.png">
  modal창을 제작하여 본인에게 해당하는 보유기술 여러가지를 선택할 수 있도록 했다.  
  보유기술 버튼 클릭시, modal창이 나오고 checkbox로 되어있는 기술 선택후 완료버튼을 누르면 modal창이 닫히면서 데이터가 넘어간다. 
  - 깃허브
  - 한 줄 소개  

가입하기 버튼을 클릭하면 회원가입이 완료된다.
### __홈__

홈 페이지에는 크게 2가지로 나뉘어진다.
- 회원 정보  
<img width="1499" alt="홈1" src="https://user-images.githubusercontent.com/77609591/173769386-6e8730d8-28d6-45a6-a260-cfdb21bb72e8.png">
회원가입을 할때 작성한 항목들을 나타낸다.  
깃허브 항목에 있는 버튼을 클릭하면, 회원가입 할 때 입력한 깃허브 주소로 이동한다.  
참여한 프로젝트 박스는 각 프로젝트의 이름을 클릭하면 해당 프로젝트 상세 설명 페이지로 이동한다.
- 프로젝트  
<img width="1494" alt="홈2" src="https://user-images.githubusercontent.com/77609591/173769394-57c0f444-e306-45cd-8216-2e141f2dd8b4.png">
__프로젝트__ 글 클릭시, 프로젝트 페이지로 이동한다.  
현재 모집중인 프로젝트만을 보여준다. 카드 형태로 주요 내용들이 압축되어 있으며, 카드 클릭시 해당 프로젝트 설명 패이지로 이동한다.  
모집중인 프로젝트 카드들을 자동을 슬라이드하게 하였다.
### __프로젝트__
<img width="1496" alt="프로젝트" src="https://user-images.githubusercontent.com/77609591/173769361-13d5cfc8-9dff-4299-ba60-fbf85c5a0fa7.png">  

프로젝트는 크게 2가지로 나뉘어진다.
- 프로젝트 홈 페이지  
모든 프로젝트를 모아둔 __홈__ 페이지이다.  
프로젝스 홈 페이지 상단에 검색란이 있다. 찾고 싶은 프로젝트의 진행 상황을 선택한 후, 프로젝트 명을 검색할 수 있다. 진행상황은 __모집중__,__진행중__,__완료__ 3가지 이다.
- 프로젝트 등록 페이지
<img width="1492" alt="프로젝트 등록" src="https://user-images.githubusercontent.com/77609591/173769371-b6361834-1bc6-4ed8-a5c8-6e1e19ed46e2.png">   
프로젝트 홈 페이지 상단 우측에 __프로젝트 등록__ 버튼이 있다. 해당 버튼을 누르면 새로운 프로젝트를 등록할 수 있다.  
프로젝트 등록을 위한 항목은 총 5개이다.
  - 프로젝트 이름 *
  - 모집완료 기간 *
  - 프로젝트 진행 기간(시작~종료) *
  - 모집중인 역할 *  
  버튼을 클릭하면 modal창이 나오며, 여러가지 포지션들을 중복 선택할 수 있다.
  - 프로젝트 소개  
  하단의 프로젝트 등록 버튼을 클릭하면 프로젝트 등록이 완료된다.
- 상세 프로젝트 설명 페이지
<img width="1486" alt="프로젝트 상세" src="https://user-images.githubusercontent.com/77609591/173769378-0e1f89e5-3ba1-4f89-ac2f-410fdf6b08e0.png">
프로젝트 홈 페이지에서 보고싶은 프로젝트 카드를 클릭하면 해당 프로젝에 대한 상세 설명 페이지로 넘어간다.  
프로젝트 등록 페이지에서 작성한 내용들을 볼 수 있다. 추가로, 현재 모집중인 역할과 참여중인 멤버의 현황을 실시간으로 확인 할 수 있다.  
페이지 하단 좌측에 위치한 참여하기 버튼을 클릭하면 해당 프로젝트에 참여가 된다.  
추가로, 프로젝트를 등록한 회원에게는 참여하기 버튼 하단에 __프로젝트 상태 변경__ 버튼이 추가로 존재한다. 이 버튼은, 오직 해당 프로젝트를 등록한 회원에게만 보이고 변경이 가능하다. 프로젝트 상태 변경 버튼을 클릭하면 modal창이 나오고 radio형식의 모집중, 진행중, 완료를 선택할 수 있다. 상태 선택 후 완료 버튼을 누르면 해당 프로젝트의 상태가 변경된다.   

---
## __마무리__  
프로젝트를 제작하면서 많은 난관을 마주했다. 일단, 나는 프로젝트를 누군가와 같이 해본적이 처음이었으며 각자의 포지션을 나누어서 진행한 것은 더더욱 생소한 경험이었다.  
각자 역할을 나누어 진행하고 추후에 프론트와 백엔드를 연동하면서 생각지도 못한 오류들이 너무 많았다. 내가 노트북에서 서버 열어서 보면 멀쩡한데 팀원 노트북에서는 깨진다던가.. 기능이 먹히지 않는다던가.. 정확한 원인을 찾지 못한 것이 아직도 있지만! 해결한 것도 많기 때문에 스트레스와 뿌듯함을 동시에 얻을 수 있는 경험이었다.  
다음번에 팀으로 프로젝트를 진행한다면 이러한 점들을 더욱 고려하고 바로바로 적용시켜 보는 등을 통해 개선해 나가야 겠다.   

InQ Project Manager에 대한 간단한 소개와 소감을 마치며, 해당 프로젝트의 frontend 부분을 제작하면서 발생했던 오류나, 새롭게 적용해본 기능, 상세한 기능 설명 등은 각 페이지 기능 설명란에 작성하도록 하겠다.