---
layout: post
title: 백준 9663번 N-Queen
categories: [algorithm]
excerpt: ""
comments: true
share: true
tags: [algorithm, baekjoon, backtracking, java]
date: 2018-12-14
---

## 문제 설명
https://www.acmicpc.net/problem/9663

![No Image](/assets/20181214/nQueen.png)

N * N 크기의 체스판에 퀸이 서로 공격할 수 없도록 놓을 수 있는 경우의 수를 구하는 문제입니다.(퀸은 같은 행, 열, 대각선으로 이동가능)
특정 노드의 유망성을 점검하여 유망하지 않다면 부모의 노드로 돌아가서 다음 노드 탐색하는 백트래킹 알고리즘을 이용하여 문제입니다.

<br>

## 문제 풀이
1. visited 배열을 만들어 열마다 놓아질 퀸의 위치를 저장한다.
2. check 함수를 통해 가능한 경우만 퀸을 놓아 갯수를 구한다.

전체 코드
~~~java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    static int count=0;
    static int[] visited;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        visited = new int[N+1];

        for(int i=1; i<N+1; i++) {
            visited[1] = i;
            nQueen(2);
        }
        System.out.println(count);
    }
    public static void nQueen(int P) {
        if(P == visited.length) {
            count++;
            return;
        }
        for(int q=1; q<visited.length; q++) {
            if(!check(P,q)) continue;
            visited[P] = q;
            nQueen(P+1);
        }
    }

    public static boolean check(int x, int y) {
        for(int p=1; p<x; p++) {
            if(visited[p] == y) // 같은 행
                return false;
            if(Math.abs(x-p) == Math.abs(y-visited[p])) //대각선
                return false;
        }
        return true;
    }
}
~~~

> 5.5초로 채점에 통과하였지만 속도를 좀 더 빠르게 할 수 있는 구조에 대해서 좀 더 생각해 봐야할 것 같습니다.
> 코드에 대한 조언 환영합니다.
