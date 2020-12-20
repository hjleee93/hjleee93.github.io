---
title: "JavaScript - Async/Await"
excerpt: "ES6문법 Async/Await로 비동기 처리하기"
categories:
  - JavaScript
  - frontend
tags:
  - JavaScript
  - frontend
  - 비동기
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

<h3>Async/Await로 비동기 처리 하기 </h3><br>

<p>
Async/Await는 Es6에서 업데이트된 비동기 패턴을 처리 하는 문법입니다. Promise 처리방식의 단점을 보완하고 조금 더 읽기 편하게 업데이트 된 형식입니다.
</p><br><br>

<h3>Async/Await 사용법</h3>

```js
async function 함수명() {
  // !await는 단독으로 사용이 불가합니다. async 함수안에서 작동합니다.
  await 비동기_처리_메서드_명();
}
```

<h3>Async/Await 에러핸들링</h3>
기존 Promise구문에서는 resolve, reject 방식을 사용하여 에러를 처리 하였지만 async/await 구문에서는 기존에사용했던 try~catch로 에러핸들링이 가능합니다.

```js
function resolveAfter2Seconds() {
  //기존의 Promise방식 비동기처리
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("resolved");
    }, 2000);
  });
}
async function asyncCall() {
  //Async/Await 방식 비동기처리
  console.log("calling");
  try {
    const result = await resolveAfter2Seconds();
    //2초뒤에 실행
    console.log(result);
  } catch (err) {
    console.log(err);
  }
  //비동기 메소드가 실행된 후 출력
  console.log("ends");
}
asyncCall();
```

[ 결과 ]<br>

- asyncCall()을 호출했을 때 "calling"이 가장 먼저 출력되며
- await resolveAfter2Seconds(); 부분이 호출되면서 Promise 함수가 실행됩니다.
- 2초 뒤에 resolve 부분의 console이 출력됩니다.
- reject에러를 던지는 경우 catch(err)부분이 실행됩니다.
  <br><br>

<a href="https://jsfiddle.net/hjleee/2gh1tL85/8/">JSFiddle에서 실행해보기 </a><br>

함수 앞부분에 async와 비동기 처리를 할 메소드 앞에 await를 작성해 주면 되기 때문의 코드의 가독성이 훨씬 향상됏음을 알 수 있습니다.

<hr>

<b>참고 문서</b>

- <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function">MDN</a>
- <a href="https://joshua1988.github.io/web-development/javascript/js-async-await/#%EA%B7%B8%EB%9E%98%EC%84%9C-%EC%9D%BD%EA%B8%B0-%EC%A2%8B%EC%9D%80-%EC%BD%94%EB%93%9C%EC%99%80-async--await%EA%B0%80-%EB%AC%B4%EC%8A%A8-%EC%83%81%EA%B4%80%EC%9D%B4%EC%A3%A0">CAPTAIN PANGYO</a>
- <a href="https://www.fastcampus.co.kr/"> 패스트 캠퍼스 </a>프론트엔드 강의
