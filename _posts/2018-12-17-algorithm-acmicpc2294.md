---
layout: post
title: 백준 2294번 동전 2
categories: [algorithm]
excerpt: ""
comments: true
share: true
tags: [algorithm, baekjoon, dynamic programming, java]
date: 2018-12-17
---

## 동적 프로그래밍(Dynamic Programming)
특정 범위까지의 값을 구하기 위해 다른 범위의 값을 이용하여 구하는 기법 입니다. 가능한 모든 방법을 충분히 빠른 속도로 구할 수 있는 경우 유용하지만 빠른 경로를 찾는 방법과 비교하였을 때, 시간이 많이 걸린다는 단점이 있습니다.


## 문제 설명
https://www.acmicpc.net/problem/2294

![No Image](/assets/20181217/coin2.png)

N가지의 동전을 사용하여 K금액을 만드는 경우 동전의 최소 갯수를 구하는 문제입니다.

<br>

## 문제 풀이
1. 구하려는 값을 d[K]로 하고 배열 d에 100001을 넣어 최소값을 구할 수 있도록 합니다.
2. d[1] 부터 d[k]까지 값을 구합니다.(동전의 가치가 작은 경우부터 동전으로 구할 수 있는 금액을 구함)
3. j 금액의 최소 동전 수를 구할 때 기존 d[j]의 값과 d[j에서 동전의 금액을 뺸 값] + 1의 값을 비교하여 최소인 경우 할당한다.(동전의 금액 = 갯수 1)
  >Math.min(d[j],d[j-coin[i]]+1);
4. 값을 구한 경우에만 출력하고 못찾은 경우 -1을 출력한다.

전체 코드
~~~java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());
        int[] coin = new int[N];
        for(int i=0; i<N; i++) {
            coin[i] = Integer.parseInt(br.readLine());
        }

        System.out.println(getMinCoinNum(coin, K));

    }

    private static int getMinCoinNum(int[] coin, int price) {
        int result = -1;
        int[] d = new int[price + 1];
        //초기 값 세팅(값을 못찾는 경우, 최소값 비교)
        for(int z=1; z<d.length; z++) {
            d[z] = 100001;
        }
        //최소값을 비교해 값 구하기
        for(int i=0; i<coin.length; i++) {
            for(int j=coin[i]; j<=price; j++) {
                d[j] = Math.min(d[j],d[j-coin[i]]+1);
            }
        }

        //값을 못 구한 경우
        if(d[price] != 100001)
            result = d[price];
        return result;
    }
}
~~~
