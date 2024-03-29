---
layout: post
published: true
categories:
  - TIL
title: '우테코: 프리코스 3주차 미션 복기'
tags:
  - TIL
  - WoowaCourse
  - JS
---

3주차 프리코스는 1,2주차 동안 활용해야겠다 생각했던 것들을 모두 활용할 수 있었던 미션이었다. 해당 미션부터는 클래스 활용과 테스트코드 작성이 정말 필수적이었기 때문이다.

그래서 기능 구현을 하기 전, 하루를 오로지 공부만 해야 했다. 이전에는 클래스를 사용해본적이 없기 때문이다. 학습하면서 어렵다고 느껴지고 이해가 완벽히 되는 느낌은 아니었지만, 미션 기능구현을 하면서 학습했던 개념들을 가지고 직접 코드를 작성하니 이해가 훨씬 잘 되었다.

프리코스를 마친 현재에도 해당 내용들이 잊혀지지 않고 진짜 '나의 지식'이 되었다는 느낌이 든다.

[3주차: 로또 Repository](https://github.com/soi-ha/javascript-lotto-6/tree/soi-ha)
<img width="779" alt="3주차 프리코스 미션 완료 캡쳐" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/9382bf4f-5494-45f7-b158-805470c7c13c">

---

## 새롭게 학습하고 적용한 점

### reduce

2주차에 고차함수를 많이 활용하여 기능을 구현했지만 reduce는 사용하지 않았다. 이번 미션에는 마침 reduce를 사용하기 딱 좋을 기능이 있어서 활용해보았다.  
reduce는 다른 고차함수랑 다르게 반환값이 배열이 아니라 값 하나이다. 그래서 수익률을 계산할때(ReturnOnInvestment.js) 해당하는 값 하나만 얻으면 됐기에 reduce를 활용하여 당첨금액을 구했다.

<img width="352" alt="reduce를 활용한 코드" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/9feb90cb-c1f6-4b7f-99ec-83e91fcf3c30">

이전에는 reduce는 사용하기 어렵고 이해하기 힘들다!라는 느낌이 너무 강해서 사용하기 꺼려졌었다. 하지만 이번에 고차함수를 마스터해보고자 남은 하나인 reduce도 3주차에 사용하게 되었는데, 막상 사용해보니 전혀 어려운 개념도 아니고 활용하기 까다롭지도 않았다. 오히려 필요한 값만 딱 return하니 굉장히 편한 함수였다..!  
그간 내가 편견을 가지고 있었다는 것을 알게 되어서, 앞으로도 이런 편견을 가지고 있던 것들을 하나씩 부숴봐야겠다 생각이 들었다.

### Class

1,2주차 프리코스를 하면서 기본적으로 주는 App.js파일이 class App으로 시작하는데, class를 활용해야 겠다~ 생각만 하고 개념만 찾아봤지 직접 사용하지는 못했었다.  
하지만 이번 3주차는 정말 class를 사용해야 했기 때문에 [JAVASCRIPT.INFO](https://ko.javascript.info/classes)를 정독하면서 학습했다. 해당 문서를 읽을때 완벽히 이해가 된다거나 어떻게 활용을 해야겠다 라는 느낌이 오지 않았었지만, 일단 개념을 머리에 넣어두고 기능 구현을 시작했다.

기능을 구현하면서 작성되어있는 Lotto.js 파일에서 클래스를 어떻게 활용하고 있는지, LottoTest.js 파일에서 요구하는 작동 범위가 뭔지를 먼저 파악했다. 해당 파일에서 사용한 class 사용 방식과 class를 적극 활용하신 다른 분의 2주차 코드를 참고하면서 코드를 작성하니 어느새 class를 나름 사용할 수 있는 수준이 되었다.

**아직은 조금 부족하지만 클래스를 처음으로 활용한 코드!**
<img width="565" alt="클래스를 활용한 코드" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/fbdbd01a-661a-406b-b0b1-587235580cac">

처음에는 prefix, 클래스 필드가 어떤 건지 전혀 몰랐고, 1주차 코드리뷰에서 validate를 활용하여 예외 처리를 하는게 어떻냐는 의견을 받았을 때 validate를 뭘 어떻게 쓰라는 건지, 유효성 검사라는 서칭 내용만 뜨고 js에서 활용하는건 어떻게 하는지 전혀 몰랐었다.  
이런 상태였지만 이번 3주차 프리코스를 마치니 이제는 prefix가 뭘 말하고 클래스 필드는 뭔지, validate를 어떻게 사용해야 할지를 알게되었다!

### 도메인 로직

`도메인 로직에 단위 테스트를 구현해야 한다`라는 프로그래밍 요구사항이 존재했는데, 단위 테스트는 들어봤어도 도메인 로직은 이번에 처음 들었다. 그래서 도메인 로직이 뭔지 검색했다.

내가 이해한 **도메인 로직이란!** 프로그램이 돌아가는데 중요한 역할을 수행하는, 이 프로그램에서 A라는 기능이 빠지면 제대로 프로그램이 돌아가지 않는다!라면 A는 도메인 로직이다 라고 이해했다. 또한 도메인 로직은 비지니스 로직과 동일한 용어라는 것도 알게되었다. ~~(제대로 이해한거 맞겠지?!)~~

도메인 로직이 뭔지 알게되니 도메인 로직에 단위 테스트를 구현하라는 요구사항은 프로그램이 돌아가는데 중요한 역할을 하는 코드가 잘 실행되는지 테스트하라!는 말이라는 걸 알게되었다.

### jest 테스트코드 작성

위의 도메인 로직 개념을 학습한 후, 테스트 코드를 작성하려고 하니 어떤 파일을 테스트 해야할지 단번에 알 수 있었다. ~~(이것이 바로 학습의 힘. 아는 만큼 보인다.)~~  
2주차 미션을 진행할 때는 전혀 감을 잡을 수 없어 얼렁뚱땅 테스트 코드를 작성했었던 것에 비해 엄청난 발전이었다.

2주차까지는 테스트 코드를 어떻게 작성해야 할지 머리는 이해했지만 가슴은 이해하지 못해 잘 작성하지 못했다. 이런 상태였지만 3주차는 테스트코드를 작성하는 것도 매우 중요한 사항이었기 때문에 어떻게든 가슴도 테스트코드 작성 방식을 이해하고자 LottoTest.js 파일과 ApplicationTest.js 파일의 코드를 정말 하나하나 뜯어가며 이해하려 했다.

**생각보다 쉬웠던 테스트코드 작성**
<img width="723" alt="테스트 코드" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/afbaa7bd-4803-4d7d-9330-f7d94a3c031d">

이로인해 예외 테스트와 원하는 결과가 잘 나오는지 기능 테스트 코드를 작성할 수 있었다. 이번에 테스트 코드를 직접 작성하면서 느낀것은 생각보다 테스트 코드 작성하는 것은 별거 없었다.. 그냥 내가 무서워하고 있었구나를 깨달았다.

**테스트코드 실행 결과**

<img width="273" alt="테스트코드 실행 결과" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/521c0dfd-f591-48d0-bdc6-3d4c6284207f">

---

## 실수했던 점

### 예외 반환 문구랑 기능테스트 문구에서 테스트 실패

예외 반환시 `[ERROR]`가 잘 포함되어 출력되는데 테스트 실패가 되어서 무슨 문제일까 엄청 찾았다. 문제는.. 제공하는 `Console` 사용안하고 그냥 `console.log`로 error를 출력하고 있어서 테스트 실패가 됐던것이었다. ~~(진짜 바보냐..ㅠ)~~

기능테스트 문구(ApplicationTest.js)에서도 테스트 실패가 발생했었는데, PrintValue 클래스에 작성한 출력 문구에 띄어쓰기를 하나 더 해서.. 테스트 실패가 됐던 것이었다.^^;

위와 같은 실수를 통해 작은 실수가 1시간 그 이상을 허비하게 되니 코드를 작성할 때 신중하게 작성해야겠다는 것을 뼈저리게.. 배울 수 있었다. (진짜 신경써서 한줄 한줄 작성하자.)

---

### 마치며

3주차는 1,2주차에 가볍게 넘어갔던 개념들을 적극 활용해야 했었다. class와 jest 테스트코드 작성은 이번 3주에 프로그래밍 요구사항으로 명확히 주지 않았더라면 사용하는 방법을 아직도 잘 몰랐을 것이고 어렵다는 편견을 가지고 있었을 것이다.  
하지만 3주차 미션을 통해 편견을 부술 수 있었고 이제는 온전히 나의 지식이 되었다. (진짜 뿌듯하다...!!!) 앞으로도 학습에 편견을 가지지 말고 도전하려고 노력할 것이다. 지금 당장 떠오르는 건 무서워서(?) 미뤄뒀던 Redux 사용하기이다. 이번 프로젝트 리팩토링때 사용해보고자 한다!

> 학습 참고자료  
> [javascript: Array.reduce() 사용 방법 정리](https://miiingo.tistory.com/365)  
> [자바스크립트 고차 함수(Higher-Order Function) 이해하기](https://velog.io/@jakeseo_me/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%9D%BC%EB%A9%B4-%EC%95%8C%EC%95%84%EC%95%BC-%ED%95%A0-33%EA%B0%80%EC%A7%80-%EA%B0%9C%EB%85%90-22-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B3%A0%EC%B0%A8-%ED%95%A8%EC%88%98Higher-Order-Function-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0#arrayprototypereduce)  
> [class(클래스)에 대해 알아보자](https://velog.io/@hamham/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-class%ED%81%B4%EB%9E%98%EC%8A%A4%EC%97%90-%EB%8C%80%ED%95%B4-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90#-2-constructor-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%B6%94%EA%B0%80)  
> [JAVASCRIPT.INFO / CLASS](https://ko.javascript.info/classes)  
> [자바스크립트 클래스 필드와 이벤트 핸들러](https://developer-alle.tistory.com/373)  
> [프로토타입과 클래스](https://learnjs.vlpt.us/basics/10-prototype-class.html)  
> [도메인 로직이 뭐지 🧐](https://velog.io/@chaerim1001/%EB%8F%84%EB%A9%94%EC%9D%B8-%EB%A1%9C%EC%A7%81%EC%97%90-%EB%8C%80%ED%95%9C-%EC%A0%95%EB%A6%AC)  
> [jest로 테스트 작성하기](https://velog.io/@hamham/jest%EB%A1%9C-%ED%85%8C%EC%8A%A4%ED%8A%B8-%ED%95%98%EB%8A%94-%EB%B2%95)  
> [TDD 시작하기 (Unit Test, Jest)](https://libertegrace.tistory.com/entry/TDD-TDD-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Unit-Test-Jest)  
> [테스트 코드로 JS 의 기능 및 로직 점검하기](https://velog.io/@skyu_dev/Jest-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%BD%94%EB%93%9C%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%97%AC-JS%EC%9D%98-%EA%B8%B0%EB%8A%A5-%EC%A0%90%EA%B2%80%ED%95%98%EA%B8%B0)
