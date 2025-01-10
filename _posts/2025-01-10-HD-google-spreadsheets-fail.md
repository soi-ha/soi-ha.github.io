---
layout: post
published: true
categories:
  - TIL
  - Project
title: '행동대장: Google 스프레드시트 사용 실패 경험 정리'
tags:
  - Wooteco
  - 행동대장
---

**🚨 해당 [issue](https://github.com/woowacourse-teams/2024-haeng-dong/pull/913)는 구현에 실패했으며, 해당 PR은 실패 사유에 대해 공유하는 글입니다.**

## 구현 목적

### 회원 탈퇴 기능을 왜 구현해야 하죠?

행동대장 서비스는 카카오로 회원가입 및 로그인이 도입되면서, 회원 탈퇴에 대한 기능이 필수적으로 필요하게 되었습니다(개인정보보호법). 따라서 회원 탈퇴 기능을 구현하게 되었습니다. 그리고 [이전 이슈](https://github.com/woowacourse-teams/2024-haeng-dong/pull/858)에서 언급한 것처럼 회원가입을 진행한 유저가 탈퇴를 하는 사유에 대해 트래킹을 해야 한다고 생각했습니다. 탈퇴 사유를 수집함으로 우리 서비스의 문제를 보완해 나갈 수 있기 때문입니다.

### 구글 스프레드시트를 데이터베이스로 사용한 이유

탈퇴 사유를 백엔드의 api를 통해 저장할 수 있지만, 저희에게는 현재 서버 비용에 한계가 존재합니다. 서비스를 통해 금전적인 이득을 취하는 상황이 아니기 때문이죠. 그렇기에 회원 탈퇴 이용에 큰 영향을 끼치지 않는 선에서 서비 비용을 줄여야 한다고 생각했습니다. 이렇게 생각하여 찾은 방법이 구글의 스프레드시트 api를 활용하는 것이었습니다.

구글 스프레드시트를 데이터베이스로 활용하여 회원 사용자의 탈퇴 사유를 저장합니다. 구글 스프레드시트는 잘 만들어진 서비스로 우리가 추후 탈퇴 사유에 대한 데이터를 분석하고 싶을 때 쉽게 확인할 수 있을 것입니다.

## 구현 방법

해당 파트는 이런 프로세스로 구글 스프레드시트를 활용할 수 있음을 정리한 것입니다. 아래 설명 할 방법처럼 구현을 하려고 했으나 제 판단으로는 프론트엔드만으로는 구현이 불가능하였다는 점..

### 구글 spreadsheet api 사용을 위한 단계

1. 구글 클라우드 프로젝트 생성 및 서비스 계정 생성
2. 스프레드 시트에 접근할 수 있도록 구글 계정 인증
3. http 요청

### 1. 구글 클라우드 프로젝트 생성

- 구글 클라우드에서 프로젝트를 생성합니다.
- 해당 프로젝트의 API에 Google Sheets를 추가합니다.
- 서비스 계정을 생성합니다.
- 계정 생성으로 얻게된 credential 파일을 클라이언트 폴더에 추가합니다.
  해당 파일에는 구글 스프레드시트에 접근하고 편집 가능할 수 있도록 하는 다양한 인증 id, key 등이 존재합니다.
- 구글 스프레드시트에서 연결하고자 하는 시트를 생성합니다.
- 해당 시트에 편집자로 서비스 계정 생성을 통해 얻은 이메일을 등록하고 편집자 권한을 부여합니다.
- 해당 시트의 url에서 sheet id를 가져와 클라이언트 파일에서 사용할 수 있도록 저장해둡니다.

### 2. 스프레드 시트에 접근 | 두가지 방법 (근데 에러를 곁들인..)

스프레드 시트에 접근하기 위해서 다음 범위(scope) 중 하나가 필요합니다.

- `https://www.googleapis.com/auth/drive`
- `https://www.googleapis.com/auth/drive.file`
- `https://www.googleapis.com/auth/spreadsheets`

우리는 위에 언급한 범위를 승인 받기 위해서 아까 위에서 생성된 서비스 계정의 이메일로 접근해야 합니다. 이때 우리는 이 승인을 각각 다른 라이브러리를 활용하여 진행할 수 있습니다.

**[googleapis](https://www.npmjs.com/package/googleapis?activeTab=readme) 라이브러리 활용**

- googleapis 라이브러리에서 google import
- google.auth를 사용하여 인증

**[google-auth-library](https://www.npmjs.com/package/google-auth-library) 활용**

- google-auth-library에서 JWT import
- new JWT를 생성하여 인증
- 단, 해당 라이브러리를 사용하여 구글 스프레드시트에 인증을 받을 때는 [google-spreadsheet](https://www.npmjs.com/package/google-spreadsheet?activeTab=readme) 라이브러리를 활용합니다.
  google-spreadsheet는 외부에서 제작한 라이브러리이기 때문에 변화에 즉각 대응하기 어렵습니다. 하지만 비교적 구현하기 쉽습니다.

**🚨두 방법 모두 공통적인 문제 발생**

두가지 방법을 각각 시도하면서 공통적인 문제가 발생했습니다. 무려 에러의 갯수는 총 35개…

<img width="611" alt="webpack_v5_error_1" src="https://github.com/user-attachments/assets/f330e54d-8d03-42e4-87d0-bb2976e7c266" />

에러의 유형은 다음과 같습니다.

- webpack 버전 5 사용으로 인해 폴리필 에러  
  `webpack < 5 used to include polyfills for node.js core modules by default`
- child_process를 찾을 수 없는 에러  
  `Module not found: Error: Can't resolve 'child_process’`
- net, tls를 찾을 수 없는 에러
  `Module not found: Error: Can't resolve 'net’`

**[폴리필 에러는 해결이 가능했습니다만](https://soi-ha.github.io/2025/01/10/2025-01-10-HD-child-process-not-find-module.html)**… 남은 두 에러는 해결이 불가능했습니다..
위의 두 에러는 **Node.js 환경에서 동작해야 할 코드가 브라우저 환경에서 실행**하려고 할 때 발생하는 것이기 때문이었죠..  
[(이 에러를 발견한 상황을 보고 싶다면 여기를 클릭..)](https://soi-ha.github.io/til/project/2025/01/10/HD-polyfills-error.html)

네.. 저는 클라이언트에서 서버 코드를 실행하고 있었던 것이었스빈다!!ㅜ

**👉결론👈**

구글 스프레드시트 api를 사용하고 싶으면 백엔드가 필요하다… 입니다…

### 3. http 요청

해당 코드가 실행되면 403에러가 발생하는 것을 보아하니 제대로 된 코드는 맞는 것 같습니다. 아까워서… 이렇게라도 남겨봐요…

잘 작동하는지 테스트하기 위해서 값들은 냅다 넣었습니다. ~~흐린눈.. 부탁드립니다.~~  
(참고로 아래 코드에서 사용한 requestPostWithResponse는 행동대장 클라이언트 코드에서 제작한 api 요청을 위한 메서드입니다.)

```tsx
export const requestPostGoogleSpreadSheets = async () => {
	await requestPostWithResponse({
		baseUrl: 'https://sheets.googleapis.com/v4/spreadsheets',
		endpoint: `/${process.env.GOOGLE_SPREADSHEET_ID}/values/Sheet1!A1:E1:append`,
		queryParams: {
			valueInputOption: 'RAW',
		},
		headers: {
			'Access-Control-Allow-Credentials': 'true',
		},
		body: {
			range: 'Sheet1!A1:E1',
			majorDimension: 'ROWS',
			values: [
				['Door', '$15', '2', '3/15/2016'],
				['Engine', '$100', '1', '3/20/2016'],
			],
		},
	});
};
```

## 참고

- [기본 쓰기 : Google Sheets : Google for Developers](https://developers.google.com/sheets/api/samples/writing?hl=ko#append_values)
