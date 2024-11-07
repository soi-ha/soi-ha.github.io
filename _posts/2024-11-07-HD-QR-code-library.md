---
layout: post
published: true
categories:
  - TIL
  - Project
title: '행동대장: 각 행사 url로 QR코드 생성하기 (`qrcode.react`, `react-qr-code`, `qr.js` 비교하기)'
tags:
  - Wooteco
  - 행동대장
---

## 행동대장 서비스에 QR코드를 도입한 이유

행동대장 서비스는 sns 친구가 아니어도 정산을 할 수 있도록 하는 것이 목적이다. 그러나 기존에는 최초의 목적에 부합하지 않았다. 카톡이 아니더라도 최소 하나의 SNS의 친구가 되어있어야 했다. 따라서, 각 행사의 고유 QR코드를 생성하여 SNS가 없어도 사용할 수 있고 대면에서 바로 행사 페이지에 접속할 수 있도록 했다.

## 어떻게 QR코드를 생성할까?

QR코드를 만드는 방법은 다양하게 있을 것이다. 직접 구현하거나, 라이브러리를 활용하거나.

1. 직접 만들기  
   가능한 일이지만 시간 제약이 존재하며 리소스가 넘쳐나는 세상에서는 너무 비효율적인 일이라고 생각한다.  
    QR코드를 생성하기 위한 알고리즘을 직접 구현하는 것은 매우 복잡하고 수학적인 연산이 필요할 것이다. 따라서, 해당 방법은 바로 목록에서 지워버렸다.

2. 라이브러리 활용하기  
   우리는 시간과 인적 자원을 아끼기 위해 손쉽게 QR코드를 생성해주는 라이브러리를 활용할 것이다.  
   다음 이미지는 NPM에 등록된 QR코드를 생성해주는 라이브러리를 다운로드 순으로 정렬한 것이다.

   <img width="932" alt="NPM QR코드 생성 패키지 다운로드 순위로 정렬" src="https://github.com/user-attachments/assets/58cf5fc9-6445-4927-9bfc-7c614f0e86f4">

   가장 많이 사용된 3가지는 `qrcode.react`, `qr.js`, `react-qr-code`이다. 이 3가지 라이브러리 중 어떤 것을 사용할지 비교를 통해 선택해보자.

## `qrcode.react`, `react-qr-code`, `qr.js` 비교하기

### qrcode.react

- 성능

  React 환경에 최적화되어 있어 QR 코드 생성 속도가 빠르고 효율적이다. 이 라이브러리는 React의 렌더링 방식에 맞게 설계되었기 때문에 대규모 애플리케이션에서도 안정적인 성능을 제공한다. 이는 React의 Virtual DOM을 효과적으로 활용하여 불필요한 재렌더링을 최소화하기 때문이다.

- 커스터마이징

  색상, 크기, 배경색 등 기본적인 커스터마이징이 가능하지만, 로고 삽입이나 복잡한 디자인 커스터마이징은 지원하지 않는다.

- 사용성

  React 개발자라면 쉽게 사용할 수 있으며, 설치 및 사용법이 직관적이다. 별도의 복잡한 설정 없이도 간단히 QR 코드를 생성할 수 있다.

- 문서화

  문서화가 잘 되어 있어 이해하기 쉽고, 예제 코드가 풍부하여 다양한 사용 사례를 참고할 수 있다. 지속적인 업데이트로 최신 기능을 반영한다.

- 안정성

  널리 사용되는 라이브러리로 안정적이며, 다양한 프로젝트에서 검증되었다. 활발한 커뮤니티 지원으로 문제 발생 시 빠른 해결이 가능하다.

- [qrcode.react NPM 문서](https://www.npmjs.com/package/qrcode.react)

### react-qr-code

- 성능

  성능이 뛰어나 React 애플리케이션에서 빠르게 QR 코드를 생성할 수 있다. 특히 동적 데이터와 연동하여 실시간으로 QR 코드를 생성할 때 유리하다. 이는 이 라이브러리가 React의 상태 관리와 연동이 잘 되어 있어, 상태 변화에 따라 QR 코드가 즉시 업데이트되기 때문이다.

- 커스터마이징

  기본적인 커스터마이징 옵션을 제공하며, 색상, 크기, 스타일 등을 쉽게 변경할 수 있다. 다만, 고급 커스터마이징 옵션은 제한적이다.

- 사용성

  React 개발 환경에서 쉽게 사용할 수 있도록 설계되어 있으며, 직관적인 사용법을 제공한다. 사용법이 간단해 초보자도 쉽게 사용할 수 있다.

- 문서화

  잘 정리된 문서화로 쉽게 시작할 수 있으며, 예제 코드와 함께 제공되어 실습하며 배울 수 있다. 지속적인 업데이트를 통해 최신 기능을 지원한다.

- 유연성

  다양한 QR 코드 생성 요구사항을 충족시킬 수 있는 유연성을 제공하며, 다양한 크기와 스타일의 QR 코드를 생성할 수 있다.

- [react-qr-code NPM 문서](https://www.npmjs.com/package/react-qr-code)

### qr.js

- 성능

  경량 라이브러리로 QR 코드 생성 속도가 매우 빠르다. 성능이 중요한 경우에 적합하며, 서버 사이드 렌더링(SSR)에도 유리하다. 이는 이 라이브러리가 독립적인 JavaScript로 동작하여 Node.js 환경에서도 쉽게 통합될 수 있기 때문이다.

- 커스터마이징

  기본적인 QR 코드 생성 기능만 제공하며, 고급 커스터마이징 기능은 부족하다. 색상, 크기 등 제한적인 커스터마이징만 가능하다.

- 사용성

  경량 라이브러리로 간단한 API를 제공하여 사용이 쉽다. 설치 및 사용법이 매우 직관적이며, 작은 프로젝트에 적합하다.

- 문서화

  간단한 문서화가 되어 있어 기본적인 사용법을 이해하기 쉽다. 다만, 업데이트가 자주 이루어지지 않으며, 최신 기능 지원이 제한적이다.

- 크기
  라이브러리 크기가 작아 애플리케이션의 전체 크기에 부담을 주지 않으며, 빠른 로딩 속도를 유지할 수 있다.
- [qr.js NPM 문서](https://www.npmjs.com/package/qr.js)

## 어떤 라이브러리를 선택?

라이브러리를 선택하기에 앞서, 행동대장 서비스에서 QR코드를 생성할 때 필요한 조건들을 나열해보자.

1. QR 코드의 색상 변경이 가능할 것
2. QR 코드의 사이즈 변경이 가능할 것
3. 하나의 행사. 즉, 하나의 url에 하나의 QR코드가 생성될 것

위 조건 말고는 다른 것은 크게 중요하지 않았다. 하나의 행사에 url이 동적으로 계속 변경되는 것이 아니었기 때문이다. 위 조건에 맞춰 3가지 라이브러리 중 하나를 선택해보자.  
일단, qr.js 제외. 업데이트가 12년 전이고.. 아직도 버전이 0.0.0이기 때문이다. 12년전보다 웹 생태계는 많이 바뀌었다고 들었기에 오래된 라이브러리 사용은 좋지 않다고 판단했다.
그리고 남은 두 라이브러리.. react-qr-code와 qrcode.react는 모든 측면에서 우수하고 비슷한 스펙을 가지고 있다. 그래서 뭘 선택해야 하나 고민이 많이 들었는데.. 둘 중 하나를 제외하자면 react-qr-code였다. url의 상태가 지속적으로 변하는 서비스가 아니였기 때문에 동적 데이터에 장점을 둔 해당 라이브러리의 사용은 굳이였다.

따라서, QR코드 생성도 빠르면서 간단한 커스터마이징이 가능하고 지속적으로 배포가 이뤄지고 있는 qrcode.react를 선택했다. 그리고 무엇보다 qrcdoe.react는 가장 많이 사용되고 있기 때문에 자료를 찾기가 수월했다. 문서화도 매우 친절하게 잘 되어있고! ~~← 이게 가장 크다.~~

## qrcode.react를 사용하여 행동대장 서비스에 QR코드 기능 추가하기

```tsx
import { QRCodeSVG } from 'qrcode.react';
import getEventPageUrlByEnvironment from '@utils/getEventPageUrlByEnvironment';
import getEventIdByUrl from '@utils/getEventIdByUrl';

// ...

const QRCodePage = () => {
	const eventId = getEventIdByUrl();

	return (
		// ...
		<div css={QRCodeStyle()}>
			<QRCodeSVG value={getEventPageUrlByEnvironment(eventId, 'home')} size={240} fgColor={`${theme.colors.black}`} />
		</div>
		// ...
	);
};

export default QRCodePage;
```

- qrcode.react의 QRCodeSVG 컴포넌트를 사용한다.
- QRCodeSVG 컴포넌트의 value로 QR 코드를 생성할 url를 넣어준다. 그러면 QR코드가 생성된다.(초간단!) 해당 QR코드는 동일한 url이라면 변하지 않는다.

### 데스크탑 QR 코드로 초대하기

현재 데스크탑에서 header의 정산 초대하기 버튼을 클릭하면 내용 및 url 복사만 가능하다. 데스크탑에서도 QR code 기능을 추가하는 것이 좋을 것이라고 판단했다.
따라서, 데스크탑에 정산 초대하기 버튼을 클릭하면 DropDown을 통해 `링크 복사하기`와 `QR코드로 초대하기`를 선택할 수 있도록 했습니다.

```tsx
import { useNavigate } from 'react-router-dom';
import { Dropdown, DropdownButton } from '@components/Design';
import getEventIdByUrl from '@utils/getEventIdByUrl';

// ...

const DesktopShareEventButton = ({ onCopy }: DesktopShareEventButtonProps) => {
	// ...

	const navigate = useNavigate();
	const eventId = getEventIdByUrl();

	const navigateQRPage = () => {
		navigate(`/event/${eventId}/qrcode`);
	};

	return (
		<div style={{ marginRight: '1rem' }}>
			<Dropdown base="button" baseButtonText="정산 초대하기">
				<DropdownButton text="링크 복사하기" onClick={copyAndToast} />
				<DropdownButton text="QR코드로 초대하기" onClick={navigateQRPage} />
			</Dropdown>
		</div>
	);
};

export default DesktopShareEventButton;
```

## 구현 결과

- 변경된 행동대장 모바일 버전: 정산 초대하기

  <img width="290" alt="모바일 버전 QR코드 초대하기1" src="https://github.com/user-attachments/assets/6e3e114b-58bf-4c5c-82bc-118588fcafe4">
  <img width="297" alt="모바일 버전 QR코드 초대하기2" src="https://github.com/user-attachments/assets/cf9eba10-4b0e-4bfd-befc-63c8df43040d">

- 변경된 행동대장 데스크탑 버전: 정산 초대하기

  <img width="566" alt="데스크탑 버전 QR코드 초대하기1" src="https://github.com/user-attachments/assets/24d0ffe8-87eb-4dee-852a-99c34a0de9d2">
  <img width="584" alt="데스크탑 버전 QR코드 초대하기2" src="https://github.com/user-attachments/assets/15046f7e-b88b-45b2-826e-d0872640ad5b">

## 참고자료

- [어느 것이 더 나은 QR 코드 생성 라이브러리?](https://npm-compare.com/ko-KR/qr-code-styling,qr.js,qrcode.react,qrious,react-qr-code)
