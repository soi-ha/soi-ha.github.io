---
layout: post
published: true
categories:
  - TIL
title: '우테코: 프리코스 2주차 미션 복기'
tags:
  - TIL
  - WoowaCourse
  - JS
---

모든 프리코스가 끝나고 이제야 작성하는 약간 늦은 2주차 프리코스 복기이다.  
다 끝나고 진행해서 까먹은 개념도 존재할 것이고, 해당 복기로 인해 당시의 내 코드를 돌아볼 수 있기에 조금 늦게 작성하지만 안 적는 것 보단 낫다고 생각해서 지금이라도 작성해본다.  
해당 글은 당시에 내가 작성했던 소감문을 바탕으로 작성했다.

[2주차: 자동차 경주 Repository](https://github.com/soi-ha/javascript-racingcar-6/tree/soi-ha)
<img width="779" alt="2주차 프리코스 예제 테스트 성공 캡쳐" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/bd010c95-24a4-4577-932e-dd0577d65058">

2주차 미션은 1주차 미션의 코드리뷰를 통해 새롭게 알게된 개념들을 적극 반영하여 구현하였다. 확실히 이전보다 코드를 읽는데 아주 약간은..! 나아진 듯 싶다.

---

## 리뷰받은 내용 반영

### 고차함수 사용하기

1주차 코드리뷰에서 고차함수를 사용해보는 것도 좋다는 피드백을 받았다. 그전에는 for이나 while을 통해 단순히 코드를 작성했었다. 코드리뷰를 통해 고차함수가 있다는 것을 알았고 for을 사용하는 것 보다 코드가 더 간단해질 수 있다는 것을 알게되었다. 그래서 이번 2주차에는 고차함수인 filter, map, forEach을 사용하여 기능을 구현했다.

- **filter를 사용하여 값 걸러내기**

  자동차의 이름을 입력받을 때, 5글자 이상이거나 영어나 한글이 아니면 레이싱을 진행할 수 없도록 했다. 이때, 해당 조건에 맞는 값들만 걸러내기 위해서 filter을 사용했다.

  <img width="588" alt="filter를 사용한 코드" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/00e50676-897f-437e-9fbe-5af314739ce1">

- **map을 사용하여 입력받은 자동차 이름의 수만큼 배열에 값(0) 생성하기**

  우승자를 찾기 위해 입력받은 자동차의 이름의 수만큼 0의 값이 존재하는 배열이 필요했다. 이때 map을 사용하여 입력받은 자동차의 값을 x로 하여 0의 값을 반환하도록 해서, 자동차의 수 만큼 0의 값을 갖고 있는 배열을 생성했다.

  <img width="353" alt="map을 사용한 코드" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/67e0f94b-0379-4d40-be38-b2b11dda6194">

- **forEach를 사용하여 배열 값 변경하기**

  forEach을 사용하여 입력받은 자동차의 이름들을 `이름` 에서 `이름 : `의 형태로 배열의 값을 변경했다.

  <img width="263" alt="forEach를 사용한 코드1" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/069d87fb-0716-4597-94bf-6d2627019a74">

  첫번째 인수(car)를 통해 각 자동차의 이름을 받고 두번째 인수(idx)를 통해 해당 값의 위치를 활용했다.

  또한 우승자의 이름을 담은 배열을 생성할 때 forEach를 사용했다. 자세한 설명은 뒤에서!

  <img width="562" alt="forEach를 사용한 코드2" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/ef081db8-c632-4065-897d-2c247bffefd6">

### Set을 사용하여 중복 제거

1주차 코드리뷰 중 Set을 사용하여 중복을 제거하는 것을 추천하는 내용이 있었다. 그래서 사용하게 될 기회가 있어서 Set을 활용해보았다.  
자동차 이름을 입력받을 때, 중복된 이름은 입력받지 않도록 처리해야 했다. 이때 자동차의 이름을 담은 배열의 길이와 Set을 사용하여 중복을 제거한 자동차 이름을 담은 배열의 길이를 비교하여 중복된 값이 존재하는지 확인했다.

<img width="412" alt="Set을 사용한 코드" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/b5edd26a-429c-4e6f-8b32-4564d4869ddc">

이 과정을 통해 추가로 알게된 것은 Set의 길이를 구할때는 length가 아니라 size라는 것을 알게되었다. length로 했는데 값이 안나와서.. 조콤 당황했다가 바로 찾아보고 size로 해야 한다는 것을 처음 알게 되었다.

### 매직넘버

코드리뷰를 통해 매직넘버의 개념을 처음 알게 되었다. 1주차에서는 boolean의 값을 1과 0 혹은 true로 단순히 명시하여 사용했었다.  
매직넘버의 개념을 알게된 2주차에는 `const FORWARD = true;`와 같이 매직넘버를 적극 활용하여 기능을 구현했다. 사실 1주차에서 const를 사용하면 모두 upper 스네이크 케이스를 사용해야 하는 줄 알았다. 1주차 코드리뷰를 통해 그렇지 않다는 것을 알게 되었다.

<img width="389" alt="매직넘버 사용한 코드" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/6bc74e12-9e7f-4bd6-8f03-e5d163167fba">

매직넘버(const)일때, 해당 값은 절대 변경되지 않을 것이기에 upper 스네이크 케이스를 사용하고 단순 const를 사용할 때에는 lower 카멜케이스를 사용하면 된다는 것을 확실하게 알 수 있었다.

---

## 2주차에 새롭게 적용한 개념

### 정규표현식

자동차 이름을 입력받을 때, 올바르게 입력했는지 확인하기 위해 정규표현식을 사용하였다.  
이전에도 정규표현식은 알았지만 작성하는 것이 어렵다고 생각하여 사용하는 것을 회피했다. 하지만, 생각보다 그리 복잡한 것도 아니었고.. ~~우리에겐 챗GPT가 있기 때문에 이 친구한테 부탁하면 바로 알려준ㄷ.. ^^~~

<img width="580" alt="정규표현식을 사용한 코드" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/1d171186-3c14-49a8-ae07-793ed73bd1bc">

### indexOf('')

우승자의 이름만을 출력하기 위해서 득점 현황만 담고있는 배열(forwardCount)에서 가장 높은 득점의 위치(MAX_COUNT)를 찾는다. 해당 위치를 가지고 이름을 담고있는 배열(carList)에서 **이름**만을 출력하도록(이름 배열에 전진 실행결과도 같이 존재함) indexOf를 활용했다.

<img width="562" alt="indexOf를 사용한 코드" src="https://github.com/soi-ha/soi-ha.github.io/assets/77609591/f2a0feed-67ca-4dea-95f9-75e426ed1a49">

`indexOf(' ')`를 사용하여 첫 공백의 위치를 확인했다. carList에는 값이 `soha : --------`형태로 존재하는데, 첫 공백의 위치를 찾으면 **0번째부터 해당 위치의-1** 까지의 값만 출력하면 이름. 즉, `soha`만을 출력할 수 있다.

### jest로 테스트코드 작성

이번 미션을 하면서 테스트 코드를 작성해보는 것은 처음이었다. 개념은 알고 있었지만 직접 기능을 구현한 코드에서 테스트 코드까지 작성하는 것은 처음이기에 좀 막막했다.  
그래서 미션을 제출하는 순간에도 이렇게 테스트코드를 작성하는 것이 맞을 지 확신이 들지는 않았다. 실제로, 4주차까지 미션을 완료하고 나니.. 음 이렇게 테스트코드를 작성하는 것은 아니었다는 것을 알게되었다. 그래도 3주차부터 제대로된 테스트코드를 작성하는 방법을 알게되었다.

## 2주차 미션을 마치며

2주차는 1주차에 많은 역경을 견뎌서 그런지.. 1주차에 비해서 개인적인 스케줄이 많았음에도 좀 더 빨리 기능을 구현할 수 있었다. 2주차 미션을 진행하면서 개발 속도가 빨라진 것을 보고 일주일동안 정말 많은 성장을 이뤘다는 것을 몸소 느낄 수 있었다.  
1주차때 애먹게 만들었던 행동도 하지 않기 위해 신경쓰고, 코드리뷰를 통해 받았던 피드백들을 적극 활용해보려고 노력했다. 이를 통해 이전보다 조금 더 코드의 가독성이 높아진 것 같다. 실제로 이때 들인 습관 덕분에 3,4주차 미션에도 잘 활용할 수 있었다.

> 학습 참고자료  
> [자바스크립트 세트(Set) 완벽 가이드](https://www.daleseo.com/js-set/)  
> [Set() 함수를 이용한 중복제거 / Set 함수 객체 길이 구하기](https://muna76.tistory.com/241)  
> [idexOf: 문자열의 특정 문자 개수 세는 방법](https://deeplify.dev/front-end/js/count-characters-in-string)  
> [코드의 매직 넘버 (Magic Number) 란 무엇일까?](https://jake-seo-dev.tistory.com/155)
