---
title: "React 시작하기 - 포트폴리오 사이트만들어서 배포하기 3"
excerpt: "Netlify사용하여 배포하기"
categories:
  - frontend
tags:
  - React
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

<h3>Netlify로 리액트 배포하기</h3>

우선 전반적인 사이트 구성은 완료가 되어 배포를 먼저 해보려고합니다. <br>
항상 프로젝트 완료시 배포에서 오류가 많이 나기 때문에...배포를 어느정도 성공 한 후에 리액트 라우트 개발을 시작할 예정입니다.

Netlify는웹 호스팅을 지원하는 클라우딩 플랫폼으로 무료 호스팅이 가능합니다! 저도 이 무료 호스팅을 사용할 예정입니다. 멋진 녀석<br>

저는 프론트단으로만 이루어진 단순 포트폴리오 사이트이기 때문에 node서버나 몽고디비같은 데이터 연결은 없기 때문에 Netlify를 사용하였고 서버가 추가되는 경우에는 Heroku를 사용하셔야되는 걸로 알고 있습니다!

우선 넷리파이 사이트에 들어가셔서 회원 가입 해주세요

<a href="https://www.netlify.com/">NETLIFY 사이트 바로가기</a>

가입 하셧으면 new site from git 버튼을 눌러주세요!<br>
저는 이미 포트폴리오 배포가 완료되서 배포된 사이트가 보입니다

<img src="/assets/img/netlify-1.png">

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

여러분들도 성공적인 포트폴리오 사이트 배포하시길 바라겠습니다:)
