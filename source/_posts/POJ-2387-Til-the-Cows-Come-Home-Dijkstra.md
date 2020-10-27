---
title: POJ 2387 Til the Cows Come Home (Dijkstra)
date: 2019-10-11 23:09:35
tags:
- ACM刷题
- Dijkstra
- 最短路
- 图论
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2387](http://poj.org/problem?id=2387)

## Description
> Bessie is out in the field and wants to get back to the barn to get as much sleep as possible before Farmer John wakes her for the morning milking. Bessie needs her beauty sleep, so she wants to get back as quickly as possible.
> 
> Farmer John's field has N (2 <= N <= 1000) landmarks in it, uniquely numbered 1..N. Landmark 1 is the barn; the apple tree grove in which Bessie stands all day is landmark N. Cows travel in the field using T (1 <= T <= 2000) bidirectional cow-trails of various lengths between the landmarks. Bessie is not confident of her navigation ability, so she always stays on a trail from its start to its end once she starts it.
> 
> Given the trails between the landmarks, determine the minimum distance Bessie must walk to get back to the barn. It is guaranteed that some such route exists.

<!--more-->

## Input
> * Line 1: Two integers: T and N
> 
> * Lines 2..T+1: Each line describes a trail as three space-separated integers. The first two integers are the landmarks between which the trail travels. The third integer is the length of the trail, range 1..100.
 
## Output
> * Line 1: A single integer, the minimum distance that Bessie must travel to get from landmark N to landmark 1.
 

## Sample Input

```markdown
5 5
1 2 20
2 3 30
3 4 20
4 5 20
1 5 100
```

## Sample Output

```markdown
90
```

## Hint

> INPUT DETAILS:
> 
> There are five landmarks.
> 
> OUTPUT DETAILS:
> 
> Bessie can get home by following trails 4, 3, 2, and 1.

## Source

[USACO 2004 November](http://poj.org/searchproblem?field=source&key=USACO+2004+November)

## Solution

### 题意

给定无向图，求点 $1$ 到 $n$ 的最短路。

### 题解

**Dijkstra**

$Dijkstra$ 的模板题，注意有重边。

写了一下堆优化的 $Dijkstra$。

## Code

```cpp
#include <iostream>
#include <queue>
#include <map>
#include <cmath>
#include <algorithm>
#include <cstring>
using namespace std;
const int inf = 0x3f3f3f3f;
const int N = 1010;

int t, n;
int d[N];
bool v[N];

priority_queue<pair<int, int> >q;
int g[N][N];

void dijkstra(int s) {
    memset(d, 0x3f, sizeof(d));
    memset(v, 0, sizeof(v));
    d[s] = 0;
    q.push(make_pair(0, s));
    while(q.size()) {
        int x = q.top().second; q.pop();
        if(v[x]) continue;
        v[x] = 1;
        for(int i = 1; i <= n; ++i) {
            if(g[x][i] != inf) {
                if(d[i] > d[x] + g[x][i]) {
                    d[i] = d[x] + g[x][i];
                    q.push(make_pair(-d[i], i));
                }
            }
        }
    }
}


void init() {
    for(int i = 1; i < N; ++i) {
        for(int j = 1; j < N; ++j) {
            g[i][j] = inf;
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    while(cin >> t >> n) {
        init();
        for(int i = 0; i < t; ++i) {
            int x, y, z;
            cin >> x >> y >> z;
            if(z < g[x][y]) {
                g[x][y] = g[y][x] = z;
            }
        }
        dijkstra(1);
        cout << d[n] << endl;
    }
    return 0;
}
```
