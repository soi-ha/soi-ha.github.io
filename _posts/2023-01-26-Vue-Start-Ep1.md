---
layout: post
categories:
  - TIL
title: "Vue.js: Vue 시작하기 Ep.1"
tags:
  - TIL
  - Vue.js
  - JS
---

## __Vue CLI, Vetur__
---

- Vue CLI 설치
  
  [Vue CLI Link](https://cli.vuejs.org/)
  
  경로 위치 상관없이 해당 명령어를 실행하면 된다. (-g는 전역설치이기 때문에 경로 위치 상관없음)
  
  ```bash
  npm install -g @vue/cli
  # 또는
  yarn global add @vue/cli
  ```
    
- Vue CLI로 폴더 생성
  
  Vue CLI 설치로 아래와 같은 명령어 사용 가능
  
  ```bash
  # 간단하게 새로운 프로젝트 생성하기
  ## vue create 폴더명
  vue create hello-world
  ```
  
  폴더 생성 후, 다음과 같은 옵션 선택이 뜬다. (화살표로 이동이 가능하며 선택이 가능하다.)
  
  현재는 옵션 선택에 vue2,3버전 중 어떤 걸 사용할 것인지 묻는다.  
  (원하는 버전으로 선택하면 된다.) 
  
  ![https://cli.vuejs.org/cli-new-project.png](https://cli.vuejs.org/cli-new-project.png)
    
- package.json
  
  <img width="289" alt="스크린샷 2023-01-26 14 54 51" src="https://user-images.githubusercontent.com/77609591/214785809-6c349beb-e2cd-49d4-95cc-d0454939c478.png">
  
  - serve는 dev와 동일한 것이다.
    
    개발서버를 열고 싶다면 다음과 같은 명령어 사용
    
    ```bash
    npm run serve
    ```
      
  - 제품화 과정에서는 `npm run build`
  - **lint**
    
    vue.js 코드를 어떤 규칙에 맞게 작동이 잘 되었는지 확인하기 위해서 해당 명령어를 사용한다.
    
    lint를 통해서 코드를 작성하는 규칙을 정할 수 있다.  
    하단에서 **eslintConfig** 에서 기본적인 구성옵션을 확인할 수 있으며, **rules** 부분에 원하는 규칙을 추가할 수 있다.
      
  - vue-cli의 경우에는 내부적으로 webpack을 사용하고 있다.
    
    webpack에 대한 기본적인 구조는 프로젝트 내에서 숨겨져 있다.  
    대신, 숨겨진 부분을 vue-cli-service를 통해서 제공한다.
      
  - browserslist
    
    프로젝트가 지원할 브라우저의 기본적인 종류를 명시한다.  
    vue.js 설치시 자동으로 작성되어져 있으며, babel, postCSS등을 기본적으로 사용할 수 있다.
        
- Vue CLI 장점과 단점
  
  __장점__
  
  기본적인 부분부터 디테일한 부분까지 vue-cli에서 흡수를 했기 때문에 입문자 입장에서는 구성옵션을 최대한 신경쓰지 않고 문법에 집중해서 프로젝트를 만들 수 있어서 편리하다. 
  
  __단점__
  
  vue-cli에서 대부분 관리하기 때문에 세부적인 부분들을 정리하면서 나만의 프로젝트를 관리하기 어렵다.
    
- public과 src 파일 **(Vetur)**
  - html 파일
    
    `<div id=”app”></div>` 에 vue.js를 연결해서 사용한다.
    
  - main.js
    
    ```jsx
    import { createApp } from 'vue'
    import App from './App.vue'
    
    createApp(App).mount('#app')
    ```
    
    createApp을 vue에서 가져와서(import) 사용을 하고 있다.  
    4번째 줄 createApp 앞에는 Vue.이 생략되어져 있으며, 이것을 대신해서 vue 패키지에서 해당하는 내용을 가져와서 직접적으로 실행하는 구조로 되어져 있다.
    
    mount 메소드를 통해서 html에서 app 아이디를 가진 요소를 선택해서 vue.js 프로젝트를 바로 연결한다.
    
    App이라는 이름으로 어떤 내용을 가져와서 연결을 하는데, 해당 내용은 주변에 존재하는 App.vue 파일에서 온다.
      
  - App.vue (확장자 **Vetur**)
      
    npm 프로젝트로 시작하게 되면, vue라는 확장자를 가진 파일을 만들어서 vue 프로젝트를 시작할 수 있다.
    
    App.vue 파일을 열어보면 하이라이팅이 되어있지 않아 내용을 확인하기 어려운데, vscode에서 확장자를 설치하여 하이라이팅을 만들어준다.   
    vscode에서 **Vetur**라는 확장자를 설치해준다.
    
    - template
      
      기본적인 html 내용을 작성한다.
        
    - script
      
      자바스크립트 내용을 작성한다.
        
    - style
      
      css 내용을 작성한다.

## __Vue3 Webpack Template__
---

이전 webpack 연습에서 사용했던 폴더를 그대로 사용할 것이다. 

- src 폴더 생성
    
  main.js 파일 생성 (기존에 존재했던 js 폴더는 삭제)  
  App.vue 파일 생성
  
  App.vue와 main.js을 연결해서 사용한다.  
  해당 내용이 정상적으로 작동할 수 있도록 패키지를 설치한다.
    
- vue 패키지 설치 (일반 의존성 모드)
  
  vue 패키지는 기본적인 문법을 해석하기 위한 패키지이다.
  
  ```bash
  npm i vue@next
  ```
  
  @next 없이 설치하면 2 버전이 설치된다. 해당 키워드를 입력해야 최신 버전(3 버전)이 설치된다.
  
  vue.js는 개발 할 때만 사용하는 것이 아닌 실제 브라우저에서 동작하는 패키지이기 때문에 개발의존성 모드 (-D)로 설치하면 안된다.
    
- vue 관리를 위한 패키지 (개발 의존성 모드)
  
  프로젝트 내부에서 vue 파일을 관리하기 위해서 해당 패키지들을 설치한다.
  
  ```bash
  npm i -D vue-loader@next vue-style-loader @vue/compiler-sfc
  ```
  
  - vue-loader
    
    뒤에 @next를 붙여서 설치한다. 최신 버전(버전 3)으로 설치하기 위함이다.
      
  - vue-style-loader
    
    App.vue와 같이 vue 파일 내부에서 style 태그로 css 내용을 작성할 수 있다. 이런 vue 파일 내부에 있는 style 태그를 해석해서 동작할 수 있도록 만들어준다.
      
  - @vue/compiler-sfc
    
    vue 파일을 변환해서 브라우저에서 동작할 수 있는 형태로 바꿔준다.
        
- webpack.config.js
  
  ```jsx
  rules: [
        {
          test: /\.vue$/,
          use: 'vue-loader'
        },
        {
          test: /\.s?css$/,
          use: [
            'vue-style-loader',
            'style-loader',
            'css-loader',
            'postcss-loader',
            'sass-loader'
          ]
        }
  ```
  
  vue 확장자를 찾아 번역하기 위한 코드를 추가한다.   
  scss 부분에 vue-style-loader가 마지막으로 읽히기 위해서 가장 상단에 해당 패키지 명을 작성한다.
  
  ```jsx
  const { VueLoaderPlugin } = require('vue-loader')
  ```
  
  상단에 해당 내용을 불러온다. 객체구조분해를 통해서 내용을 가져온다는 것을 주의하도록 하자.
  
  ```jsx
  plugins: [
    ...
    ,
    new VueLoaderPlugin()
  ]
  ```
  
  plugins 부분에 불러왔던 내용을 new 키워드를 통해 생성자 함수로 실행한다.
    
- App.vue
  
  정상적으로 실행하는지 확인하기 위한 간단한 코드 작성
  
  ```
  <template>
    <h1>{{ message }}</h1>
  </template>
  
  <script>
  export default {
    data() {
      return {
        message: "Hello Vue!!"
      }
    }
  }
  </script>
  ```
    
- main.js
  
  ```jsx
  import Vue from 'vue'
  import App from './App.vue'
  
  Vue.createApp(App).mount('#app')
  ```
  
  Vue 라는 이름으로 vue 패키지를 불러온다.  
  App 이라는 이름으로 주변에서 App.vue 파일을 불러온다.
  
  mount의 인수로 ‘#app’을 넣는다. html에서 아이디 값이 app인 요소를 찾아 Vue.js 프로젝트를 연결한다.
  
  createApp의 인수로는 App을 넣는다.   
  만약 cdn으로 vue를 불러와서 사용했다면 해당 인수로는 우리가 App.vue에서 작성했던 data 부분을 작성했을 것이다. 하지만, 해당 방법에서는 App.vue 파일을 만들어 해당 파일에 data 내용을 작성했기 때문에 불러왔던 이름을 넣어주기만 하면 된다.
  
  즉, main.js에서 App.vue 파일이 Vue 프로젝트의 시작이 될 수 있도록 설정을 하는 것이다.
  
  - Vue CLI를 사용해서 작성한다면!
    
    ```jsx
    import { createApp } from 'vue'
    import App from './App.vue'
    
    createApp(App).mount('#app')
    ```
    
    Vue. 객체를 작성하지 않아도 되며, import로 가져오기를 할때 객체구조분해를 통해서 바로 createApp을 가져올 수 있다. 
      

<br>

---

<br>

### __vue 확장자를 생략하기__

<br>

- webpack.config.js
  
  ```jsx
  module.exports = {
    resolve: {
      extensions: ['.js', '.vue']
    },
  }
  ```
  
  resolve와 extensions 추가로 js와 vue 확장자를 생략해도 문제없게 만들어주었다.

<br>

---

<br>

### __새로운 component 만들기__

<br>

App.vue파일에 연결할 component를 만든다.

- src 폴더 내부에 **components 폴더** 생성
- 해당 폴더 내부에 HelloWorld.vue 파일 생성
  
  파일명은 첫번째 글자 대문자, 두번째 단어의 첫 글자도 대문자인 방식 (파스칼 표기법 PascalCase)으로 작성한다.
    
- 이미지 출력하기 (**경로 별칭**)
  - src 폴더 내부에 **assets 폴더**를 생성하고 사용하길 원하는 이미지를 해당 폴더에 넣는다.
  - HelloWorld.vue 파일에 다음과 같이 경로를 입력한다.
    
    ```jsx
    <template>
      <img src="~assets/logo.png" alt="">
    </template>
    ```
    
    해당 경로에서 사진을 가져와서 vue 파일 내부에서 분석하여 화면에 출력할 수 있게 만들어줘야 한다.
      
  - 패키지 설치하기
    
    ```bash
    npm i -D file-loader
    ```
      
  - webpack.config.js
    
    rules 부분에 이미지 파일 확장자들을 찾아서 file-loader 패키지를 실행하도록 한다.
    
    ```jsx
    module: {
        rules: [
          ...
          {
            test: /\.(png|jpe?g|gif|webp)$/,
            use: 'file-loader'
          }
        ]
      }
    ```
    
    reslove 부분에 **경로 별칭(Alias)**을 추가한다.  
    기존에는 상대경로를 통해서 파일을 불러왔다. 경로 별칭(~)을 사용하면 해당하는 경로별칭이 지칭하는 실제 경로로 바로 점프해서 해당하는 파일을 찾을 수 있게 한다.  
    말 그대로, 경로를 별칭으로 만들어서 사용하는 것이다.
    
    틸드(물결)부분에는 path의 resolve를 통해서 현재 webpack.config.js가 있는 경로(__dirname 의미)에서 src 폴더를 지칭해준다.
    
    assets 부분에서는 __dirname으로 src폴더 안에 있는 assets 라는 실제 이미지가 존재하는 폴더를 지칭해준다.
    
    ```jsx
    resolve: {
        // 경로 별칭
        alias: {
          '~': path.resolve(__dirname, 'src'),
          'assets': path.resolve(__dirname, 'src/assets')
        }
      }
    ```
    
    우리가 만든 경로 별칭을 통해서 틸드 혹은 틸드의 assets에 손쉽게 접근을 해서 실제 이미지나 파일들을 가지고 올 수 있다.
    
  - App.vue
    
    잘 작동하는지 확인하기 위해서 다음과 같이 코드를 작성한다.
    
    HelloWorld는 경로별칭 사용이 가능하다. 해당 파일은 components 폴더 안에 존재하는데 이 폴더는 src 폴더안에 있기에 틸드(src로 지정) 경로별칭을 사용할 수 있다. 
    
    ```jsx
    <template>
      <h1>{{ message }}</h1>
      <HelloWorld />
    </template>
    
    <script>
    import HelloWorld from '~/components/HelloWorld'
    
    export default {
      components: {
        HelloWorld
      },
      data() {
        return {
          message: "Hello Vue!!"
        }
      }
    }
    </script>
    ```
    
    export default에 components 속성을 객체 데이터로 만들어서 HelloWorld를 연결해준다.
    
    template에 HelloWorld를 빈 태그처럼 작성해주면 된다.