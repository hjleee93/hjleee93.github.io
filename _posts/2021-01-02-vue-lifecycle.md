---
title: "Vue.js 개념정리 - 1"
excerpt: "Vue.js 라이프 사이클 정리"
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

<h3>Vue.js 라이프 사이클 정리하기</h3>

파이널 프로젝트를 Vue.js로 진행하면서 프로젝트를 시간안에 끝내야했기 때문에 개념정리를 하면서 프로젝트를 한다는 게 쉽지않았다. <br>프로젝트는 비록 끝났지만 이제라도 Vue.js 개념정리를 해두려고 한다. <br>
처음으로는 나에게 가장 많은 어려움을 주었던 라이프 사이클
Vue.js 공식사이트에서 발췌한 내용을 위주로 정리해두었습니다.

<a href="https://vuejs.org/v2/guide/instance.html">Vue.js 사이트 바로가기</a>

Vue의 라이프 사이클을 검색하면 가장 먼저 만날 수 있는 이미지<br>
정말 직관적으로 이해하기 쉽게 작성된 플로우 차트라고 생각한다.<br>
단지 내가 겉핥기로만 알고있었을 뿐,,,

<img src="/assets/img/lifecycle.png">

우선 크게 Creation, Mounting, Updating, Destruction을 나눠서 정리해보겠습니다.

<li>Creation은 라이프사이클에서 가장 먼저 실행되는 단계입니다. </li>

<!--
. Creation, Mounting, Updating, Destruction
1. Creation은 라이프사이클 중 가장 먼저 실행되는 단계이다. 이 단계의 훅에서는 DOM트리에 해당 컴포넌트가 반영이 안되므로 태그의 id나 class에 접근할 수 없다.
   훅으로는 beforeCreated, created가 있는데 beforeCreated에서는 data나 event에 접근할 수 없다.
1. Mounting은 DOM 삽입 단계로 렌더링 되기 직전의 컴포넌트에 접근할 수 있다. 훅으로는 beforeMount, mounted가 있는데 beforeMount훅은 템플릿과 렌더 함수들이 컴파일이 되고 렌더링이되기 직전 단계에 호출이 된다. 아직 까지 DOM element에 직접적으로 접근할 수가 없다. mounted 훅에서, 컴포넌트가 렌더링이 된 상태일 때 호출된다. DOM에 접근할 수 있지만 주의해야할 점은 자식 컴포넌트에서 마운트된 상태임을 보장할 수 없다는 점이다.
1. Updating은 웹페이지의 내용이나 무언가 바껴서 재렌더링을 해야할 때 실행된다. 훅으로는 beforeUpdate와 updated가 있고 beforeUpdate 훅은 DOM변경이 완료가 되고 패치가 되기 직전에 호출이된다. updated 훅은 재 렌더링이 완료된 이후에 호출이 된다. updated는 패치 이후에 호출되는 훅이라 변화가 끝난 DOM에 접근이 가능하다.
1. Destruction은 컴포넌트가 해체?파괴될 때 실행된다. 훅으로는 beforeDestroy, destroyed 단계로 beforeDestroy는 해체 직전에 호출되며 모든 DOM과 이벤트들이 남아있다. destroyed는 해체가 완전히 된 후에 호출이되는 훅이다.

가입 하셧으면 new site from git 버튼을 눌러주세요!<br>
저는 이미 포트폴리오 배포가 완료되서 배포된 사이트가 보입니다

저는 깃헙을 사용하였기 때문에 깃헙으로 배포 진행하겠습니다

<img src="/assets/img/netlify-2.JPG">

깃헙을 선택하시면 레파지토리를 모든 레파지토리를 등록하지
일부 레파지토리만 등록할지 선택 할 수 있습니다!
저는 포트폴리오 레파지토리만 등록해두었습니다.

<img src="/assets/img/netlify-3.JPG">

해당 레파지토리를 클릭하시면 아래와 같은 화면이 나옵니다!
Deploy site를 클릭하시면 배포 완료!
<img src="/assets/img/netlify-4.JPG">

빌드 진행시간이 소요되므로

<img src="/assets/img/netlify-5.JPG">

Site deploy in progress 이 끝나길 기다려주세요! 그럼 아래와 같은 도메인이 생깁니다.
기본 제공되는 도메인을 사용하셔도 좋고 저는 도메인을 구매하여 등록하였습니다

<img src="/assets/img/netlify-5.JPG">

도메인을 적용한 제 포트폴리오 사이트입니다!

<a href="https://hyeonlog.com/">HyeonLog</a>

여러분들도 성공적인 포트폴리오 사이트 배포하시길 바라겠습니다:) -->
