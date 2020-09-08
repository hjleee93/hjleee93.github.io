---
title: "JSP : 전송 방식 get/post HTTP Methods"
excerpt: "get/post 방식 구분하기"
categories:
  - jsp
tags:
  - JSP
  - get
  - backend
---

<style>
@font-face { font-family: 'IBMPlexSansKR-Regular';
   src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_20-07@1.0/IBMPlexSansKR-Regular.woff') format('woff'); font-weight: normal; font-style: normal; }
body, a{
font-family: 'IBMPlexSansKR-Regular';
}
</style>

<h3>전송방식 구분하기</h3>

<ol>

<li>get방식</li>
  <div><small>주소창을 타고 넘어가기 때문에 서버로 보내는 데이터를 사용자가 그대로 볼 수 있다. 흔히, 주소창에서 ? 다음에 오는 parameter들이 get방식이라고 볼수 있다.<br>
       - 정보에 <b>취약</b><br>
       - 255자 이하의 적은 양의 데이터를 전송</small></div>
<li>post방식</li>
<div><small>html header를 타고 넘어간다.<br>
       - 보안에 <b>강함</b><br>
       - 255자 이상의 대용량 양의 데이터를 전송</small></div>
</ol><br>

```jsp

/* form태그로 요청 */
<form action="CallServlet" method="get">
//method --> get / post 방식을 결정 할 수 있음
//기본값은 get
//CallServlet --> 요청할 서블릿
  <input type="submit" value="전송">
  //submit을 클릭해야 서블릿이 요청된다
</form>

/* a 태그로 요청 */
//a태그로 링크를 걸어준 경우에서 get 방식으로 인식한다.

<a href="CallServlet">get방식</a>

```
