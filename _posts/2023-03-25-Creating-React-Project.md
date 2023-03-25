---
layout: post
categories:
  - TIL
title: "React: CRA, husky, lint-staged"
tags:
  - TIL
  - React
---

## **Create React App**

---

**CRA**

react, react DOM 라이브러리와는 별개로 이것들을 편하게 사용할 수 있도록 프로젝트를 생성해주는 역할을 한다.  
단순히 프로젝트를 생성하는 것이 아니라 개발하면서 필요한 여러가지 기능들을 제공한다.

- react 프로젝트 만들기

  ```bash
  npx create-react-app 파일명
  ```

  해당 명령어를 통해 react 프로젝트를 만드는 데 필요한 여러 패키지들이 다운된다.

- react-scripts

  package.json 파일을 확인하면 react-scripts를 확인할 수 있다.  
  해당 라이브러리는 CRA 라이브러리이다. 해당 프로젝트를 개발환경에 띄운다던가 배포를 위한 빌드작업을 하는 등의 역할을 한다.

## **husky**

---

git hook을 쉽게 사용할 수 있도록 해준다.  
hook은 git으로 인해 어떤 액션이 발생할때 무언갈 추가로 해주고 싶은 것을 처리해주는 도구이다.

husky는 꼭 git을 설치하고 난 후, 설치해야 한다. (아니면 오류가 발생할 수도..)

```bash
# husky 설치
npm i husky -D

# husky에서 git hook을 활성화 시키기 (git hook 설치)
npx husky install
```

package.json 파일에 해당 코드 추가

```json
"scripts": {
	"prepare": "husky install",
	...
}
```

```bash
# hook 추가하기
npx husky add ./husky/이름 "npm test"
```

- 해당 기능은 언제 사용할까?

  커밋이 되기 직전에 코드를 점검할 때 사용한다.  
  husky는 커밋전에 확인해서 정리가 안되면 커밋을 시키지 않아 강제로 뭔가를 처리할 수 있도록 하기 때문에 점검할 때 유용하게 사용할 수 있다.  
  따라서 커밋이 되었다는 것은 점검을 마쳤다는 것으로 볼 수 있다.

## **lint-staged**

---

> [husky, lint-staged를 사용하자( sub : ESLint 자동화하기 )](https://velog.io/@do_dadu/husky-lint-staged를-사용하자-sub-ESLint-자동화하기)

lint-staged는 stage 상태의 git 파일에 대해 lint와 우리가 설정해둔 명령어를 실행해주는 라이브러리다. 즉, eslint, prettier, husky를 자동화 해주는 거랄까?!

여기서 stage 상태란, 파일들이 git add로 커밋 대상이 된 상태를 말한다.

```bash
# hook 추가하기 (husky 설치 후)
npx husky add ./husky/이름 "lint-staged"

# lint-staged 설치
npm i lint-staged -D
```

package.json 파일에 lint-staged 설정을 추가할 수 있다.  
해당 확장자를 가진 것 혹은 특정 파일이 커밋될 때, 무언갈 하겠다는 의미이다.

```json
"lint-staged": {
	"**/*.js": [
		"eslint --ifx",
		"git add"
	]
}
```

해당 코드는 어느 파일에서든 js 확장자를 가진 파일이 커밋된다면 eslint를 통해 fix하고 git에 add를 하겠다는 의미이다.

추가로 prettier도 사용한다면 prettier 패키지 설치 후 `prettier —write` 명령어를 추가 작성해주면 된다.
