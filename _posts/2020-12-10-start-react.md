---
title: "React 시작하기 - 포트폴리오 사이트만들어서 배포하기 1"
excerpt: "리액트 프로젝트 만들기"
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

<h3>리액트 프로젝트 시작하기 - 1</h3>

아제 어느정도 파이널 프로젝프가 마무리 됏고 교육원 수료일도 얼마남지 않았기 때문에 미리미리 포트폴리오를 좀 정리해둬야 할 것 같다. 프로젝트 수가 많은 것도 아니고 고급 프로젝트가 있는것도 아니라 왜 해야되나 싶을 수도 있다. 하지만 이력서 만으로 기술을 설명하기에 부족한 부분도 좀 있고 보는 사람의 편의를 위해 만드는 것이므로 예전에 배웠던 리액트를 다시한 번 복습겸 시작해 보려고 한다!

강의는 fastcampus의 <a href="https://www.fastcampus.co.kr/dev_online_react">프론트엔드 강의</a>를 들으며 진행하고있습니다.

<p><b>시작하기 전에 node.js를 미리 설정해주세요. 이 포스팅은 vs코드 기반으로 작성되었습니다. </b> </p>

<h2>리액트 새 프로젝트 만들기</h2>

1. 프로젝트에서 사용할 새폴더 만들기 : 경로는 마음대로 지정해주세요

2. vs코드 새 윈도우 열기 - 드래그 앤 드랍으로 새폴더를 새 윈도우에 넣어주세요

<img src="/assets/img/react-1.JPG">

3. ctrl + ~ 눌러서 terminal을 열어주세요
   bash로 설정해주시고
   (bash가 없다면 git을 깔아주세요! <a href="https://git-scm.com/downloads">다운로드 바로가기</a>)

4. npx create-react-app '프로젝트명'을 터미널에 입력해주세요
   (저는 portfolio로 만들었습니다.)

<img src="/assets/img/react-2.JPG">

5. 이러한 화면이 터미널에 나타나면 리액트 프로젝트 만들기 끝!
   <img src="/assets/img/react-3.JPG">
   <img src="/assets/img/react-4.JPG">

6. 만들어진 리액트 프로젝트를 확인하기 위해서 터미널에 npm start라고 입력해주세요
   <img src="/assets/img/react-5.JPG">

서버가 실행되고 자동으로 윈도우창이 열리면서 리액트 화면이 나타나면 성공적으로 잘 설치 끝!!

이번달안으로 포트폴리오 완성하자!!!!
