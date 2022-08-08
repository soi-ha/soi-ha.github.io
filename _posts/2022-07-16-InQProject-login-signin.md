---
layout: post
categories:
  - Project
  - TIL
title: "InQ Project Manager: 로그인과 회원가입 페이지 제작"
tags:
  - InQ
  - project
  - InQ Project Manager
  - TIL
  - HTML
  - CSS
---

## __로그인 페이지__
---
해당 프로젝트는 로그인을 해야만 이용이 가능하게 만들었다.  
그래서 해당 프로젝트에 들어가면 가장 먼저 보이는 것은 로그인 페이지이다.  
로그인 페이지는 여타 그렇듯 아이디, 비밀번호 입력창과 로그인, 회원가입 버튼을 넣었다. 아이디와 비밀번호 찾기는 구현하지 못하여서 형태만 넣어 두었다.  

<img width="1512" alt="로그인" src="https://user-images.githubusercontent.com/77609591/179473138-34594167-3995-471d-bcc8-22c93d1dfcee.png">

<br>

### __로그인 HTML__

  ```html
  <!-- Login -->
    <section class="login">
      <div class="inner">

        <div class="text-title">
          <span class="title">로그인</span>
        </div>
        <form action="">
          <div class="text-body">
            <div class="row">
              <label for="loginId" class="sub-title">아이디</label>
              <input type="text" id="loginId" name="loginId" required
                    minlength="4" maxlength="15">
            </div>
            <div class="row">
              <label for="loginPw" class="sub-title">비밀번호</label>
              <input type="password" id="loginPw" name="loginPw" minlength="8" required>
            </div>
            <div class="find-member">
              <a href="javascript:void(0)" class="find-account">아이디 찾기</a>
              <a href="javascript:void(0)" class="find-account">비밀번호 찾기</a>
            </div>
            <div class="btn-group">
              <div class="btn-row">
                <a href="#">
                  <input class="bton btn--reverse" type="button" value="로그인하기">
                </a>
              </div>
              <div class="btn-row">
                <!-- 회원가입 페이지 링크 연결하기 -->
                <a href="#">
                  <button class="bton btn--blue" onClick="location.href='../member/signup.html'" type="button">
                    회원 가입하기</button>
                </a>
              </div>
            </div>
          </div>
        </form>

      </div>
    </section>
  ```  
  - section을 만들어 login 클래스명을 부여하였다.  
  클래스 inner은 스타일을 위해 정의해둔 것으로 추후 common파일 글에 설명하도록 하겠다.  
  - __로그인__ 페이지라는 것을 알려주기 위한 타이틀
  - 입력받은 데이터를 전송해야 하기 때문에 form 태그를 사용한다.  
  입력받을 모든 데이터들은 로그인 버튼을 클릭과 동시에 데이터베이스로 전송된다.
  - text-body: 입력될 데이터들을 묶는 박스를 만들어 스타일을 정의하기 쉽게 한다. (다른 페이지에서도 비슷한 용도로 사용 될 예정)  
  - row (열): 아이디, 비밀번호 입력창의 스타일은 동일하기 때문에 중복성을 최소화 하고자 동일한 클래스명을 부여하였다.  
  - 아이디: 입력받는 타입은 text, 백엔드로 넘길 id와 name. 최소 4글자 입력, 최대 15글자 입력 가능. required 속성을 통해 필수적으로 입력을 하도록 했다.  
  - 비밀번호: 입력받는 타입은 password. 최소 8글자, 최대 15글자 입력하도록 하였다. required 속성을 부여하여 필수 입력을 하도록 했다.
  - label 과 input 태그  
    - label  
    사용자 인터페이스 항목을 설명한다.  
    input 요소와 연결하면 label 클릭시 input에 초점을 맞추거나 활성화 시킬 수 있다. 또, 폼 입력에서 label을 읽어 보조 기술 사용자가 입력해야 하는 텍스트가 무엇인지 쉽게 이해할 수 있도록 한다. _(출처: MDN 웹문서)_
    - input  
    사용자의 데이터를 받을 수 있는 대화형 컨트롤을 생성한다. 다양한 유형으로 데이터를 입력받을 수 있다.
  - find-member 클래스: 형태만 존재하게 하였다.  
  - button  
  btn-group으로 회원가입과 로그인 버튼을 묶었다.  
  btn-row로 각 버튼들의 스타일을 부여하도록 했다.  
    - 로그인 버튼  
    로그인 버튼을 클릭하면 입력한 데이터들을 백엔드로 넘기면서 로그인이 완료되어야 하기에 input 태그로 값을 보냈다.  
    - 회원가입 버튼  
    회원가입 버튼 클릭시 회원가입 페이지로 이동하게 만들었다.  
    button 태그를 사용했으며, onClick 속성을 통해 html에서 페이지를 옮길 수 있도록 했다.  
      - __onClick="이동하고자 하는 페이지의 로컬 주소"__
        ```html
          <button class="bton btn--blue" onClick="location.href='../member/signup.html'" type="button">회원 가입하기</button>
        ```

### __로그인 CSS__
  ```css
  /* Login */
    .login {
      margin-top: 300px;
      margin-bottom: 500px;
    }
    .login .inner {
      color: var(--inq-blue);
    }
    .login .text-title {
      margin-bottom: 80px;
    }
    .login .text-title .title {
      font-size: var(--large-font);
      font-weight: 800;
    }
    .login .text-body {
      margin: 0 auto;
      position: relative;
    }
    .login .text-body .row {
      display: flex;
      flex-direction: column;
      margin: 40px 0;
    }
    .login .text-body .row input {
      width: 720px;
      height: 82px;
      padding-left: 30px;
      border: 2px solid var(--inq-blue);
      color: var(--inq-blue);
    }
    .login .text-body .row .sub-title {
      margin-bottom: 18px ;
    }
    .login .text-body .find-member {
      position: absolute;
      width: 720px;
      right: 0;
      left: 0;
      margin: auto;
      padding-left: 150px;
    }
    .login .text-body .find-member a {
      color: var(--inq-blue);
    }
    .login .text-body .find-member .find-account {
      margin-left: 30px;
    }
    .login .text-body .btn-group {
      position: absolute;
      width: 720px;
      right: 0;
      left: 0;
      margin: auto;
      margin-top: 50px;
      padding-left: 165px;
    }
    .login .text-body .btn-group .btn-row {
      margin: 50px 0;
    }
    .login .text-body .btn-group button,
    .login .text-body .btn-group input {
      width: 390px;
      height: 82px;
    }
  ```
  - 버튼 hover시 색상이 변경되는데, 이에 대한 스타일은 모두 common 스타일 파일에 공통적으로 정의를 해두었기에 해당 login 스타일에는 없다.  
  - 로그인 페이지 css는 사이즈 조정이라던가 크기 조정이 대부분이다.
  - 입력창 글자 색과 테두리 색 변경하기  
  해당하는 클래스에 `color:blue; border: blue;`를 적용하면 글자색과 테두리 색을 원하는 색상으로 변경이 가능하다! -> 이번에 처음 알게 되었다.

<br>

## __회원가입 페이지__
---
회원가입 페이지를 만들 때, 로그인 페이지에서 사용한 구조와 스타일을 그대로 가져와서 사용하면 됐었기에 어려운 점은 크게 없었다.  
하지만, 내가 전혀. 단 한번도 해본 적 없었던 __모달__ 창을 만드느라 이거 하나에 굉장히 많은 시간을 썼다. 한... 이틀 정도...?ㅎ  
처음 모달창 만들 던날, 아무리 잘 입력해도 작동을 하지 않아서.. 때려치고 다른 것을 했다. 다른 날 다른 참고 자료를 보고 계속 도전한 끝에 겨우 성공해서...!!! 해당 프로젝트에서 모달창을 매우 잘 사용할 수 있었다.  
언제나 깨닫는 것은 한 번 잘 만들어 두면 두고두고 나중에 쉽게 써먹을 수 있다는 것이다. 

<img width="1493" alt="회원가입" src="https://user-images.githubusercontent.com/77609591/179473151-6dd5d8fb-a54b-4148-ac4e-e6db007ffc2f.png">

_회원가입 페이지1_

<img width="1493" alt="회원가입2" src="https://user-images.githubusercontent.com/77609591/179473161-7fffb58f-439d-49a5-8d15-98d0d8c560cb.png">

_회원가입 페이지2_

<img width="1512" alt="회원가입 - 보유기술 모달" src="https://user-images.githubusercontent.com/77609591/179473159-b11c09e1-07fc-4e6f-84c1-c80fa48f9d15.png">

_회원가입 페이지 모달창_

### __회원가입 HTML__
  로그인 페이지의 구조와 동일한 부분들은 지워두었다.  
  select 태그 사용하는 것과 modal창 구조만 남겨두었다.  
  모달창을 오로지 내가 짠 코드로만 완성하고 싶었는데 도전했다가 실패하고.. 마감 기간은 찾아오기에 결국 bootstrap을 사용하여 모달창을 구현하였다.
  ```html
  <!-- Sign up -->
  <section class="signup">
    <div class="inner">

      <div class="text-title">
        <span class="title">회원 가입</span>
      </div>
      <form action="" method="get" class="form-example">
        <div class="text-body">
          
          <div class="row">
            <label for="memberPosition" class="sub-title">포지션*</label>
            <select name="memberPosition" id="memberPosition" required >
              <option value="">--- Postion ---</option>
              <option value="fullstack">Full Stack</option>
              <option value="backend">Backend</option>
              <option value="frontend">Frontend</option>
              <option value="ios">IOS</option>
              <option value="android">Android</option>
              <option value="ai">AI</option>
              <option value="null">Null</option>
            </select>
          </div>

          <div class="row">
            <label for="memberSkill" class="sub-title">보유기술*</label>
            <div class="modal-btn">
              <!-- Button trigger modal -->
              <button type="button" class="btn btn-primary bton btn--blue-reverse" data-toggle="modal" data-target="#exampleModalCenter">
                선택하기
              </button>
            </div>

            <!-- Modal -->
            <div class="modal fade" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
              <div class="modal-dialog modal-dialog-centered" role="document">
                <div class="modal-content">
                  <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLongTitle">보유기술</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                      <span aria-hidden="true">&times;</span>
                    </button>
                  </div>
                  <div class="modal-body">

                    <div class="check-box">
                      <input type="checkbox" id="HTML" name="HTML">
                      <label for="HTML">HTML</label>
                    </div>

                    <div class="check-box">
                      <input type="checkbox" id="CSS" name="CSS">
                      <label for="CSS">CSS</label>
                    </div>

                    <div class="check-box">
                      <input type="checkbox" id="JS" name="JS">
                      <label for="JS">JS</label>
                    </div>

                    <div class="check-box">
                      <input type="checkbox" id="Spring" name="Spring">
                      <label for="Spring">Spring</label>
                    </div>

                    <div class="check-box">
                      <input type="checkbox" id="Django" name="Django">
                      <label for="Django">Django</label>
                    </div>

                    <div class="check-box">
                      <input type="checkbox" id="Vue.js" name="Vue.js">
                      <label for="Vue.js">Vue.js</label>
                    </div>

                    <div class="check-box">
                      <input type="checkbox" id="Angular" name="Angular">
                      <label for="Angular">Angular</label>
                    </div>
                    <div class="check-box">
                      <input type="checkbox" id="Swift" name="Swift">
                      <label for="Swift">Swift</label>
                    </div>
                    <div class="check-box">
                      <input type="checkbox" id="Kotlin" name="Kotlin">
                      <label for="Kotlin">Kotlin</label>
                    </div>
                    <div class="check-box">
                      <input type="checkbox" id="jQuery" name="jQuery">
                      <label for="jQuery">jQuery</label>
                    </div>
                    <div class="check-box">
                      <input type="checkbox" id="Express" name="Express">
                      <label for="Express">Express</label>
                    </div>

                  </div>
                  <div class="modal-footer">
                    <button type="button" class="btn btn-primary bton btn--reverse" data-dismiss="modal">완료</button>
                  </div>
                </div>
              </div>
            </div>
          </div>

        </div>
      </form>

    </div>
  </section>
  ```
  - 포지션 (position) 선택(select) 
  회원의 포지션은 하나만 선택하게 했다. 개발에 있어서 필요한 포지션은 한정되어 있기 때문에 한정된 선택지 안에서 하나만 고르게 하기 위해 select 태그를 사용하였다.  
  select 태그에 id와 name을 작성한다. 하나만 선택할 것이기에 name명은 동일하다.   
  정해진 선택지는 option태그를 각각 만들어 생성한다. 이때, value속성의 값은 option 태그마다 다르다. 선택지 중, 단 하나만 선택하는 것이기 때문이다.
  - __Modal 창__  
  각 개인이 가지고 있는 보유기술은 다양하다. 그렇기에 선택할 수 있는 갯수를 제한하지 않으려 checkbox 형태로 선택하는 방법으로 했다.  
  그리고 여기에 더해 따로 '선택하기'버튼을 누르면 창이 하나 뜨면서 그 창에서 checkbox를 선택하게 하고 싶었다.  
  내가 원하고자 하는 이 창을 '모달(modal)'이라고 부른다.  
  이 모달창을 처음에는 순수 내가 만들고 싶었으나, 작동을 하지 않는 통에... bootstrap의 모달창을 가져다 사용하였다.  
    ```html
    <!-- Button trigger modal -->
    <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModalCenter">
      Launch demo modal
    </button>

    <!-- Modal -->
    <div class="modal fade" id="exampleModalCenter" tabindex="-1" role="dialog" aria-labelledby="exampleModalCenterTitle" aria-hidden="true">
      <div class="modal-dialog modal-dialog-centered" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLongTitle">Modal title</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            ...
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary">Save changes</button>
          </div>
        </div>
      </div>
    </div>
    ```
    해당 코드는 [bootstarap](https://getbootstrap.com/docs/4.0/components/modal/)에서 제공하는 샘플코드이다.  
    나는 해당 코드를 바탕으로 나만의 모달창을 제작하였다.  
    모달창 안의 내용에 checkbox를 넣었다. 상단에 닫기 버튼이 이미지로 존재하기에 하단의 닫기 버튼을 없애고 '완료'버튼 하나만을 남겨두었다. 완료버튼 클릭시 모달창이 자동으로 닫히게 하는 등의 설정을 하였다. (`data-dismiss='modal'`추가로 닫히게 하였다.)

### __회원가입 CSS__  
  회원가입 페이지의 CSS는 크게 특징적인 것은 없다.  
  ```css
  /* Modal */
  .signup .text-body .row .modal-btn .btn {
    width: 250px;
    padding: 10px;
    font-size: var(--btn-font);
  }
  .signup .text-body .row .modal-dialog.modal-dialog-centered .modal-content .modal-header h5 {
    padding: 12px;
    font-weight: 700;
    font-size: var(--large-font);
  }
  .signup .text-body .row .modal-dialog.modal-dialog-centered .modal-content .modal-header button span {
    font-size: 45px;
  }
  .signup .text-body .row .modal-dialog.modal-dialog-centered .modal-content .modal-body {
    display: flex;
    flex-wrap: wrap;
  }
  .signup .text-body .row .modal-dialog.modal-dialog-centered .modal-content .modal-body .check-box input {
    width: 40px;
    height: 40px;
    margin: 30px 8px 25px 30px;
  }
  ```
  모달창 css에서 약간 애 먹은 것은 checkbox 사이즈와 정렬이 내가 생각한 것 처럼 잘 안되서 시간이 좀 걸렸다.  
  checkbox와 그 옆의 label의 사이즈랑 위치가 동일 선상에 맞추느라 힘들었다... 각각 사이즈를 다르게 해서 맞추었다. 

<br>

## __마무리__
---
로그인과 회원가입 페이지는 유사해서 금방 만들 수 있었다.  
다만, 회원가입 페이지에서 모달창이 ^^... 처음 적용해보고 만들어보겠다고 낑낑거리다가 시간이 많이 지체되었지만..! 어찌저찌 완성은 했으니 만족한다!  
다음은 가장 큰 난제였던 **메인 홈 화면** 제작기를 작성하도록 하겠다 :)
