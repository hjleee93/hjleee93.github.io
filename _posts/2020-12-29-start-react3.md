---
title: "React 시작하기 - 포트폴리오 사이트만들어서 배포하기 3"
excerpt: "Heroku사용하여 배포하기"
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

<h3>Heroku로 리액트 배포하기</h3>

우선 전반적인 사이트 구성은 완료가 되어 배포를 먼저 해보려고합니다. <br>
항상 프로젝트 완료시 배포에서 오류가 많이 나기 때문에...배포를 어느정도 성공 한 후에 리액트 라우트 개발을 시작할 예정입니다.

Heroku는 웹 호스팅을 지원하는 클라우딩 플랫폼으로 5개까지 무료 호스팅이 가능합니다! 저도 이 무료 호스팅을 사용할 예정입니다. 멋진 녀석<br>

저는 프론트단으로만 이루어진 단순 포트폴리오 사이트이기 때문에 node서버나 몽고디비같은 데이터 연결은 없기 때문에 빌드 없이 그냥 올려도 될 것으로 생각됩니다.

우선 히로쿠 사이트에 들어가셔서 회원 가입 후 히로쿠 CLI를 다운해주세요

<a href="https://www.heroku.com/">Heroku</a>

히로쿠는 git을 기반으로하여 업로드를 진행하기 때문에 우선 깃헙에 원하는 프로젝트를 커밋해주세요. 저는 기존 포트폴리오 깃헙 레파지토리를 이용하였습니다.

vscode로 터미널을 여신 후에 heroku login을 입력하시면 로그인을 원하면 아무 키를 입력하라는 안내가 나옵니다. <br>아무 키를 입력하시면 히로쿠 로그인 페이지가 뜨니
로그인을 완료해주세요

<img src="/assets/img/react-11.JPEG">

로그인을 완료하시면 아래와 같은 화면이 뜹니다. 닫으셔도 좋습니다.

<img src="/assets/img/react-12.JPEG">

이제 히로쿠 프로젝트를 만들어보겠습니다. heroku create (프로젝트명) <br>
프로젝트명은 다른 사용자와 겹치지 않게 잘 만들어주셔야 합니다.<br>
저는 그냥...heroku create으로 자동생성되는 프로젝트 이름을 사용하였습니다.
제법 멋진 프로젝트 명입니다.

<img src="/assets/img/react-13.JPEG">

마지막으로 git push heroku master만 하면 끝!<br> 뒤에 -f는 강제성로 푸시하겟다는 뜻입니다. lock 파일 있어서 그렇더라구요 지우고 다시 푸시하니까 잘 올라갔습니다!<br>
올라가는데 은근 시간이 걸려서 블로그를 같이 작성중입니다<br>
시간 소중

<img src="/assets/img/react-14.JPEG">

Build succeeded! 떳다면 거의 성공입니다!

<img src="/assets/img/react-15.JPEG">

성공이 아니네욥^^; 학교에서 과제할때 자주보던 녀석인데...ㅎ

<img src="/assets/img/react-16.JPEG">
