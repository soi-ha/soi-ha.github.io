---
layout: post
categories:
  - TIL
  - Project
title: "영화 검색 사이트: BT: Breakpoint, 모든 컴포넌트에서 전역 스타일 가져오기 "
tags:
  - TIL
  - Vue.js
  - project
  - Bootstrap
  - SCSS
---

## __BT:  Breakpoint (반응형)__
---

> [BootStrap: Breakpoints](https://getbootstrap.com/docs/5.3/layout/breakpoints/)

부트스트랩 내부에서 mixin을 통해서 만들어져 있는 기본적인 재활용 가능한 코드(media-breakpoint-up 등..)를 제공한다. 이 코드를 include 규칙을 통해서 가지고 와서, 인수로 뷰포트의 크기가 sm,md,lg 등 인지를 명시해서 내용을 활용할 수 있다.

```scss
@include media-breakpoint-down(sm) {
  .nav {
    display: none;
  }
}
```

## __모든 컴포넌트에서 전역 스타일 가져오기__
---

style 태그에서 매번 import를 통해 scss의 main.scss 파일을 가져왔다.  
하지만, 매번 작성하지 않아도 컴포넌트 내부에서 사용할 수 있는 구조를 만들어보고자 한다.

> [GitHub - webpack-contrib/sass-loader: Compiles Sass to CSS](https://github.com/webpack-contrib/sass-loader)

sass-loader의 additionalData를 사용하여 모든 컴포넌트에서 전역 스타일을 가져온다.  
webpack.config.js 파일에서 rules option 부분에 sass-loader를 사용하는 곳에서 문자 데이터 형태를 객체 데이터 형태로 바꿔준다.  
loader는 sass-loader, options는 additionalData를 통해서 필요로 하는 데이터를 추가해준다.

```scss
module.exports = {
	module: {
		rules: [
			{
				test: /\.s[ac]ss$/i,
				use: [
					"style-loader",
					"css-loader",
					{
						loader: "sass-loader",
						options: {
							additionalData: "$env: " + process.env.NODE_ENV + ";",
						},
					},
				],
			},
		],
	},
};
```

additionalData의 명시된 코드가 우리가 사용하는 모든 scss파일의 가장 앞 부분에 삽입이 된다. 이로인해, 우리가 따로 import를 추가하여 scss폴더의 main.scss 파일을 불러오지 않아도 자동으로 가져와진다.

- 기존
  
  ```jsx
  {
    test: /\.s?css$/,
    use: [
      // 순서 중요!
      'vue-style-loader',
      'style-loader',
      'css-loader',
      'postcss-loader',
      'sass-loader'
    ]
  }
  ```
    
- **변경**
  
  ```jsx
  {
    test: /\.s?css$/,
    use: [
      // 순서 중요!
      'vue-style-loader',
      'style-loader',
      'css-loader',
      'postcss-loader',
      {
        loader: 'sass-loader',
        options: {
          additionalData: '@import "~/scss/main";'
        }
      }
    ]
  },
  ```
  
  additionalData부분에 우리가 매번 명시하는 코드를 작성해주면 된다. 
  이때, 주의할 점은 **세미콜론 빼먹지 말기**!이다.
  
  해당 코드를 추가하면, 기존에 파일에 작성했던 코드들은 모두 제거해줘야 한다.