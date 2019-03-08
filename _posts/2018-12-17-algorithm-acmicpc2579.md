---
layout: post
title: 백준 2579번 계단오르기
categories: [algorithm]
excerpt: ""
comments: true
share: true
tags: [algorithm, baekjoon, dynamic programming, java]
date: 2018-12-17
---

## 문제 설명
https://www.acmicpc.net/problem/2579

![No Image](/assets/20181217/climbingStairs.png)

계단을 전부 오를 동안 밟은 계단의 점수 합이 최대인 최대값을 구하는 문제입니다.
이때, 규칙이 세가지가 있습니다.
1. 계단은 한 계단씩 혹은 두 계단씩 오를 수 있습니다.
2. 연속된 세개의 계단을 모두 밟을 수 없다.
3. 마지막 도착 계단은 무조건 밟는다.

<br>

## 문제 풀이
1. 구하려는 값을 d[endStep]으로 하고 첫단계부터 구합니다.
2. d[1]은 첫번째 계단을 밟은 경우가 가장 크므로 값을 넣어줍니다.
3. d[2]는 첫번째 계단과 두번째 계단을 둘다 밟은 경우가 크므로 값을 넣어줍니다.
4. 3번째 단계부터 최대 값을 구합니다.
   >세개의 계단을 연속으로 밟을 수 없으므로,
   현재의 3단계 전 최대값에서 전계단을 밟은 경우와 현재의 2계단 전 최대 값에서 전계단을 밟지 않은 경우 중 최대값을 구하여 d[i]값을 얻습니다.


전체 코드
~~~java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int step = Integer.parseInt(br.readLine());
        int[] stepPoint = new int[step+1];
        for(int i=1; i<stepPoint.length; i++) {
            stepPoint[i] = Integer.parseInt(br.readLine());
        }
        System.out.println(getMaxPoint(stepPoint, step));
    }

    private static int getMaxPoint(int[] stepPoint, int endStep) {
        int[] d = new int[stepPoint.length];
        d[1] = stepPoint[1];
        d[2] = stepPoint[1]+stepPoint[2];
        for(int i=3; i<d.length; i++) {
            d[i] = Math.max(d[i-3]+stepPoint[i-1], d[i-2]) + stepPoint[i];
        }

        return d[endStep];
    }
}

~~~
