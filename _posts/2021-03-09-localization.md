---
title: "Vue.js 로컬라이징"
excerpt: "Vue.js로 로컬라이징(다국어) 처리하기"
categories:
  - frontend
  - Vue.js
  - typescript
tags:
  - 로컬라이징
  - vue.js
  - HyeonLog
---
## Localization 진행하기(다국어 처리) 

학원, 학교를 다니며 프로젝트를 하면서도 항상 글로벌화에 대한 궁금증이 막연하게 있었는데 찾아보지는 않는 삶을 살아왔습니다.

캐나다 다시 가게되면 그때 해야지 해야지하고 취업까지 하게됐지만요...

여하튼 회사에서 이번 기회에 할 수 있는 기회를 얻게 되었습니다. 많이 어렵진 않을까 내심 긴장하기도 했는데 코딩하는 부분은 생각보다 어렵지 않았고 기획과 손품이 많이 드는 작업이라고 생각이 들었습니다. 



**사용언어**

- vue-cli (class component 기반)
- typescript



사용언어를 적어두긴 했기만 기본적으로 작동하는 방식은 유사합니다. 상관하지 않으셔도 괜찮습니다

> i18n 플러그인 바로가기[https://kazupon.github.io/vue-i18n/]

```
npm install vue-i18n

또는

vue add i18n //vue 3.0 이상인 경우
```


<img src="/assets/img/i18n-1.jpg">
위와 같은 파일이 생기는데 js,ts 차이는 무시해도 됩니다.

<img src="/assets/img/i18n-2.jpg">
파일 구조는 위와 같습니다.



1. 기본 언어를 유저의 브라우저 언어를 기반으로 할 건지 버튼을 만들어 해당 페이지가 전환하게 할 것인지 정한 후 locale 값을 정해줍니다.

   저는 버튼으로 바뀔 수 있게 만들 예정입니다.

   

2. i18n.ts(혹은 i18n.js)에 기존의 코드가 있다면 지워주시고 하단 코드를 입력해주세요.

   에러는 뒤에서 수정하도록 하겠습니다.

   ```javascript
   import Vue from 'vue'
   import VueI18n from 'vue-i18n'
   import { languages } from '../locales/index'
   
   const messages = Object.assign(languages)
   
   Vue.use(VueI18n)
   
   
   export default new VueI18n({
     locale: 'ko', 
     fallbackLocale:  'ko',
     messages
   })
   
   ```

   브라우저 기반의 로컬라이징을 제공하고 싶으신 경우 하단의 코드로 대체해서 작성해주시면됩니다. 

   ```javascript
   locale:navigator.language.split('-')[0],
   ```

   

2. 언어는 한국어와 영어 두가지를 제공할 예정이므로 ko.json / en.json 파일을 만들어주세요

   해당 프로젝트의 텍스트 파일은 앞으로 이 json파일에 작성해주시면됩니다.

   (json파일로 하셔도 좋고 js 파일로 작성 후 export 하셔도 무관합니다.)

   일반적인 json형식으로 작성하시면 되고, 네이밍은 해당 프로젝트의 기획자 혹은 자신의 기준에 따라 정해주시면됩니다

   ```json
   {
     "message": "hello !!"
   }
   ```

   

3. locales 폴더 안에 index.ts(혹은 index.js) 파일을 만드신 후 하단의 코드를 입력해주세요

   ```javascript
   import en from './en.json'
   import ko from './ko.json'
   
   
   export const languages = {
        en: en,
        ko: ko,
   }
   ```

4. i18n.ts(혹은 i18n.js)파일로 다시 돌아 가시며 아까 작성한 코드의 에러가 사라진 것을 확인 하실 수 있습니다. 

5. json파일이 import가 되지 않는 경우

   tsconfig.json혹은 jsconfig.json에 하단 코드를 추가해주세요

   ```json
   "compilerOptions": {
   	...
   	"resolveJsonModule": true,
   	...
   }
   ```

6. 버튼으로 해당 프로젝트를 바꾸면서 에러를 수정해주시면 로컬라이징 처리 완료!
<br><br>


기본적으로 로컬라이징은 프로젝트를 어느정도 진행하고 배포하기 직전에 하는 것이 가장 좋은 것 같습니다. 혹은 기본적인 틀만 잡아두고 본격적인 로컬라이징은 마지막에 진행하는 방식을 추천합니다.

한 페이지만 추가되어도 그 안에 있는 텍스트를 다 영어로 바꾸고 만약에 수정해야하는 텍스트가 있다면 그 부분 역시도 영어도 수정해야되고 굉장히 귀찮습니다..





