---
title: "Vue.js vuetify Data Table - 1"
excerpt: "XML 파싱해서 Data table에 데이터 넣기"
categories:
  - frontend
tags:
  - Vue.js
  - frontend
---

<style>
@font-face { font-family: 'IBMPlexSansKR-Regular';
   src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_20-07@1.0/IBMPlexSansKR-Regular.woff') format('woff'); font-weight: normal; font-style: normal; }
body, a, h3, h4,h1{
font-family: 'IBMPlexSansKR-Regular';
}
td{
	border: 1px solid;
}
</style>

<h3>Vue.js vuetify Data Table - 1</h3>

<p>시작하기 전에 vue-cli, vuetify, vuex, axios를 미리 설정해주세요 </p>

<a href="https://vuetifyjs.com/en/getting-started/installation/#vue-cli-install">vuetify 사이트 바로가기 </a>

vuetify는 bootstrap과 비슷한 ui 프레임워크로써 vue.js에서 아주 유용하게 사용 가능하다!

또한 vue에서는 bootstrap-vue 지원하고 있으므로 bootstrap과 vuetify를 같이 쓴다면 ui 구성하는 부분이 아주 손쉽게 해결된다!

<h3><u>XML 데이터 가져오기</u></h3>

사실 XML 데이터를 가져오는 것은 restful 서버만 잘 이해하고 있다면 그다지 어렵진 않은 것 같다.

현재 진행중인 프로젝트가 워크넷 api를 사용하는데 데이터들이 XML로 되어있어 우선 JSON 파일로 변환하는 작업이 필요했다.

vue cli는 node.js 기반으로 되어있기 때문에 node.js 모듈을 다 쓸 수 있다. 정말 최고다^^ node로 처음에 웹을 배워서 그런지 node 모듈을 쓸 수 있다는게 얼마나 편한지 ㅠㅠ

XML -> JSON 모듈도 당근있다<br>
<a href="https://www.npmjs.com/package/xml-js">xml-js Reference</a><br>
npm i xml-js 설치 후

```js
//최상단에 xml-js를 require 해준다
var convert = require("xml-js");
```

뷰 컴포넌트에 바로 methods로 axios를 호출해도되지만
다른 페이지에서 같은 axios를 사용하게 될 경우도 있을 것 같아 vuex를 사용하여 jobStore 파일로 분리하였다.

```js
const jobStore = {
  //store 파일을 모듈화했으므로 namespace 처리한다.
  namespaced: true,
  state: {
    //컴포넌트 간 사용할 데이터(변수)라고 생각하면 쉬움
    jobs: [],
  },
  //actions에 axios를 함수화한다.
  actions: {
    loadXml({ commit }) {
      //최신 채용 정보 xml
      axios
        .get(
          "http://openapi.work.go.kr/opi/opi/opia/wantedApi.do?authKey=[인증키값]&callTp=L&returnType=XML&startPage=1&display=20&occupation=214200|214201|214202|214302|022|023|024|025|056"
        )
        .then((response) => {
          //get 호출한 데이터 가져오기
          let data = response.data;
          //xml to json
          //compact버전으로 convert 하겟다.
          let json = convert.xml2json(data, { compact: true });
          //jobs에 파싱된 데이터 저장
          this.jobs = JSON.parse(json);
          //데이터가 수정됐으므로 mutation에 넘겨준다 --> commit
          commit("SET_POST", this.jobs);
        });
    },
  },

  mutations: {
    //state에 저장된 데이터를 변경해준다
    //mutations 말그대로 변이!
    SET_POST(state, jobs) {
      state.jobs = jobs;
    },
  },
};

//모듈화 했으므로 export를 해줘야함
export default jobStore;
```

store에 저장한 값은 vue 컴포넌트에서 사용하기 위해서 따로 불러오는 과정이 필요합니다.

```js
//store호출
import { createNamespacedHelpers } from "vuex";
const { mapState } = createNamespacedHelpers("jobStore");

...
...

export default {
mounted(){
    //action에 있는 loadXml 호출용
    this.$store.dispatch('jobStore/loadXml')
  },
  computed:{
    ...mapState([
      //매핑값
      'jobs'
    ])
  }
}
```

이렇게 불러오면 data()에 선언하는 변수와 똑같이 이제 사용이 가능합니다! <br><br>

vuex에 대한 설명이나 모듈화에 대한 설명은 진행중인 프로젝트가 끝난 후 따로 포스팅 하도록 하겠습니다. vue관련 내용들은 reference가 잘 되어있기 때문에 참조하셔도 정말 좋습니다!
