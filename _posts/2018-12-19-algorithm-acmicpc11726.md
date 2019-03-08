---
layout: post
title: 백준 11726번 2*n 타일2
categories: [algorithm]
excerpt: ""
comments: true
share: true
tags: [algorithm, baekjoon, dynamic programming, java]
date: 2018-12-19
---

## 문제 설명
https://www.acmicpc.net/problem/11726

![No Image](/assets/20181219/2nTiling.png)

2*n의 직사각형 크기를 1*2, 2*1 타일로 채우는 경우의 수를 구하는 문제입니다.
이때, 값은 10007로 나눈 나머지를 출력합니다.

<br>

## 문제 풀이
1. 구하려는 값을 d[n]으로 하고 d[1]의 경우부터 구합니다.
2. d[1]은 2*1 블록인 경우이므로 1입니다.
3. d[2]는 2*1 블록 2개인 경우, 1*2 블록 2개인 경우 2가지 경우우 이므로 2 입니다.
4. d[3]부터 d[n] = d[n-1] + d[n-2] 점화식을 대입할 수 있습니다.
(d[n-1] + 2*1 블록 , d[n-2] + 1*2 블록 2개 인 경우의 합)
> 처음 제출할때 d[1] = 1, d[2] = 2 로 값을 넣어 줬는데 런타임 에러가 발생했습니다. n의 값이 1부터 가능하므로 n의 값이 2보다 같거나 클 경우에만 d[2]를 참조하도록 주의해야합니다. 


전체 코드
~~~java

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int n = Integer.parseInt(br.readLine());
        //1*2, 2*1타일로 채우는 방법의 수
        int[] d = new int[n+1];

        for(int i=0; i<=n; i++) {
            if(i<=2)
                d[i] =i;
            else
                d[i] = (d[i-1] + d[i-2]) % 10007;
        }

        System.out.println(d[n]);
    }
}

~~~
