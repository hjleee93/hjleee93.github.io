---
title: "Vue.js vuetify Data Table - 3"
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

<h3>Vue.js vuetify Data Table - 2</h3>

지난 포스팅에서 데이터를 성공적으로 가져왓다면 이제 Data Table에 해당하는 데이터를 넣어 줄 차례이다. Data table은 페이징, 검색, 분류기능이 다 제공되어 편리하게 프론트를 구성할 수 있다. 개인적으로 페이징 처리는 예비 개발자로써 중요하다고 생각이 들기 때문에 이 프로젝트가 끝난 후 개인 프로젝트에서 다시 적용해 볼 예정이다.

<h3><u>Data Table 구성하기</u></h3>

프론트 틀은 직접 사이트에서 원하는 틀로 선택 후 작성 해주시면 됩니다.
<a href="https://v15.vuetifyjs.com/en/components/data-tables/"> Data Table Reference</a>

Data Talbe을 사용할 컴퐅넌트에서 mapState로 jobs로 불러왔기 때문에 바로 작성이 가능합니다.

전체 코드

```js
//테이블 코드
<div class="overflow">
    <!-- 테이블 -->
    <v-card>
      <v-card-title class="search-bar">
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
       </v-card-title>

        <v-data-table
          class="row-pointer mt-4"
          //테이블의 헤더
          :headers="headers"
          //테이블 body의 데이터가 될 mapState 값을 넣어줍니다.
          :items="tableList"
          :search="search" >

        <-- template 파트는 필수 항목은 아니지만 테이블의 구성을 수정하고 싶을 때 사용하면 됩니다 -->
        <template v-slot:item="props">

          <tr class="job-info" @click="moveDtlPage(props.item.jobNo)">
            <td >{{props.item.company}}</td>
            <td><p id="job-title">{{props.item.title}}</p><p>

              <table>
                <tr >
              <td class="title-dtl"><span>{{props.item.career}}</span></td>
              <td class="title-dtl"><span>{{props.item.holidayTpNm}}</span></td>
              <td class="title-dtl"><span>{{props.item.region}}</span></td>
              </tr>
              </table>
              </p></td>
            <td>{{props.item.ability}}</td>
            <td>{{props.item.Condition}}</td>
            <td v-if="props.item.deadline.includes('채용시까지')">
             채용시까지</td>
            <td v-else>
              <!-- d-day 7일이하  -->
              <b-btn class="d-day-btn argent-btn mr-2"
              v-if="($moment($moment(20+props.item.deadline).format('YYYY-MM-DD')).diff($moment(new Date()), 'days') + 1 ) <= 7">D-
              {{$moment($moment(20+props.item.deadline).format('YYYY-MM-DD')).diff($moment(new Date()), 'days') + 1 }}
              </b-btn>
              <!-- d-day 20일이하  -->
              <b-btn class="d-day-btn warn-btn mr-2"
              v-else-if="($moment($moment(20+props.item.deadline).format('YYYY-MM-DD')).diff($moment(new Date()), 'days') + 1 ) > 7 &&
              ($moment($moment(20+props.item.deadline).format('YYYY-MM-DD')).diff($moment(new Date()), 'days') + 1 ) <=20 ">D-
              {{$moment($moment(20+props.item.deadline).format('YYYY-MM-DD')).diff($moment(new Date()), 'days') + 1 }}
              </b-btn>

              <b-btn class="d-day-btn ok-btn mr-2" v-else>D-
              {{$moment($moment(20+props.item.deadline).format('YYYY-MM-DD')).diff($moment(new Date()), 'days') + 1 }}
              </b-btn>
              {{ props.item.deadline}}
            </td>
          </tr>
        </template>
        </v-data-table>
    </v-card>
      </div>


```

```js
import { createNamespacedHelpers } from "vuex";
const { mapState } = createNamespacedHelpers("jobStore");

export default {
  data: () => ({
    search: "",

    //테이블의 헤더에 해당하는 내용을 작성해줍니다. class css 수정용으로 작성한 내용이므로 신경쓰지 않으셔도 됩니다.
    headers: [
      { text: "기업명", value: "company", class: "custom-header" },
      { text: "제목", value: "title", class: "custom-header" },
      { text: "지원자격", value: "ability", class: "custom-header" },
      { text: "근무조건", value: "Condition", class: "custom-header" },
      { text: "마감일", value: "deadline", class: "custom-header" },
    ],
  }),
  //store이름을 다르게 구성하엿으니 신경쓰지 않으셔도됩니다!
  mounted() {
    this.$store.dispatch("jobStore/loadJobTable");
  },

  methods: {
    //상세페이지로 이동하는 메소드
    moveDtlPage: function (e) {
      this.$router.push({ name: "jobInfoDtl", params: { wantedNo: e } });
    },
  },
  computed: {
    ...mapState([
      //매핑값
      "tableList",
      "jobInfo",
    ]),
  },
};
```

데이터만 잘 가지고 온다면 테이블에 데이터를 넣어주는 과정은 그렇게 어렵지 않은 것 같다.
물론 테이블의 데이터를 수정하고 싶을 때 v-slot:item="props" 을 사용하는 것을 같은 팀원이 알려주었는데 사실 정확하게 어떻게 돌아가는 건지 아직 파악을 하지 못했다.

프로젝트에 필요한 부분 부분만 공부하다보니 공부에 공백이 생기는 것 같아서 아쉽지만, 처음부터 프로그래밍의 모든것을 공부하기란 쉽지않아서..
프로젝트가 끝난후에 처음부터 개념 정리용으로 다시 공부를 시작해봐야겟다.
