---
title:  "백준 알고리즘 - 14681번"
excerpt: "if 단계별 풀이 c++"
categories: 
  - algorithm
tags:
  - c++
  - 백준알고리즘
---

<style>
@font-face { font-family: 'IBMPlexSansKR-Regular';
   src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_20-07@1.0/IBMPlexSansKR-Regular.woff') format('woff'); font-weight: normal; font-style: normal; }
body, a{
font-family: 'IBMPlexSansKR-Regular';
}
</style>

<h3>백준 알고리즘 - 14681</h3>

<p>학원을 다니는 동안 주로 java나 프론트단 언어를 사용해서 c++을 더 잊어버리기 전에 단계별 알고리즘 c++로 뿌시기 시작! </p>

<a href="https://www.acmicpc.net/problem/14681">14681번</a>

```c++

#include <iostream>

using namespace std; //간단한 코딩이므로 namespace std 사용

int main()
{
    int num1 = 0;
    int num2 = 0;
    
    cin >> num1 >> num2; 

    if(num1 > 0 && num2 >0){ //1사분면인경우
        cout << 1;
    }else if(num1 <0 && num2 >0){//2사분면
        cout <<2;
    }else if(num1 <0 && num2 <0){//3사분면
        cout <<3;
    }else if(num1 >0 && num2 <0){//4사분면
        cout <<4;
    }

    return 0;
}

```

<p>이 문제는 왜 안풀고 예전에 넘어갔는 지 모르겠다. while문까지 완료된 상태이고 1차원 배열부터 차근차근 도장깨기 시작</p>
