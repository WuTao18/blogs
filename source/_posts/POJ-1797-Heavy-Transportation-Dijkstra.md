---
title: POJ 1797 Heavy Transportation (Dijkstra)
date: 2019-10-12 23:22:15
tags:
- ACM刷题
- Dijkstra
- 最短路
- 图论
categories: 竞赛
mathjax: true
---

题目链接：[POJ 1797](http://poj.org/problem?id=1797)

## Description
> Background
> 
> Hugo Heavy is happy. After the breakdown of the Cargolifter project he can now expand business. But he needs a clever man who tells him whether there really is a way from the place his customer has build his giant steel crane to the place where it is needed on which all streets can carry the weight.
> 
> Fortunately he already has a plan of the city with all streets and bridges and all the allowed weights.Unfortunately he has no idea how to find the the maximum weight capacity in order to tell his customer how heavy the crane may become. But you surely know.
> 
> Problem
> 
> You are given the plan of the city, described by the streets (with weight limits) between the crossings, which are numbered from 1 to n. Your task is to find the maximum weight that can be transported from crossing 1 (Hugo's place) to crossing n (the customer's place). You may assume that there is at least one path. All streets can be travelled in both directions.

<!--more-->

## Input
> The first line contains the number of scenarios (city plans). For each city the number n of street crossings (1 <= n <= 1000) and number m of streets are given on the first line. The following m lines contain triples of integers specifying start and end crossing of the street and the maximum allowed weight, which is positive and not larger than 1000000. There will be at most one street between each pair of crossings.
 
## Output
> The first line contains the number of scenarios (city plans). For each city the number n of street crossings (1 <= n <= 1000) and number m of streets are given on the first line. The following m lines contain triples of integers specifying start and end crossing of the street and the maximum allowed weight, which is positive and not larger than 1000000. There will be at most one street between each pair of crossings.
 

## Sample Input

```markdown
1
3 3
1 2 3
1 3 4
2 3 5
```

## Sample Output

```markdown
Scenario #1:
4

```

## Source

[TUD Programming Contest 2004, Darmstadt, Germany](http://poj.org/searchproblem?field=source&key=TUD+Programming+Contest+2004)

## Solution

### 题意

有 N 个城市，M 条道路，Hugo Heavy 要从城市 1 到城市 N 运输货物，每条道路都有它的最大载重量，求从城市 1 到城市 N 运送最多的重量是多少。

### 思路

**Dijkstra**

与 [POJ 2253 Frogger](https://wutao18.github.io/2019/10/11/POJ-2253-Frogger/) 类似，修改一下 $Dijkstra$ 的松弛方程：$if\ d[v] < min(d[u], w[u][v])\ then\ d[v] = min(d[u], w[u][v])$。注意 $d$ 数组初始化成无穷大。

这题有点坑，输出两个换行。

此题还可以用最大生成树解决。[戳这里](https://wutao18.github.io/2019/10/12/POJ-1797-Heavy-Transportation-%E6%9C%80%E5%A4%A7%E7%94%9F%E6%88%90%E6%A0%91/)

## Code

```cpp
#include <iostream>
#include <cstdio>
#include <queue>
#include <map>
#include <cmath>
#include <algorithm>
#include <cstring>
using namespace std;
const int N = 1010, M = 1e6 + 10;
const int inf = 0x3f3f3f3f;
typedef pair<int, int> P;
int n, m;

struct Edge {
    int to, w;
    Edge(int to, int w): to(to), w(w) {}
};
vector<Edge> G[N];
int d[N], v[N];

void init() {
    for(int i = 0; i < N; ++i) {
        G[i].clear();
    }
}

void add(int x, int y, int z) {
    G[x].push_back(Edge(y, z));
}

void dijkstra(int s) {
    // priority_queue<P,vector<P>,greater<P> > q;
    priority_queue<P> q;
    memset(d, 0, sizeof(d));
    memset(v, 0, sizeof(v));
    d[s] = inf;
    q.push(P(inf, s));
    while(q.size()) {
        P p = q.top(); q.pop();
        int x = p.second;
        if(v[x]) continue;
        v[x] = 1;
        for(int i = 0; i < G[x].size(); ++i) {
            Edge e = G[x][i];
            if (d[e.to] < min(d[x], e.w)) {
                d[e.to] = min(d[x], e.w);
                q.push(P(d[e.to],e.to));
            }
        }
    }
}

int main() {
    int T;
    scanf("%d", &T);
    int kase = 0;
    while(T--) {
        init();
        scanf("%d%d", &n, &m);
        for(int i = 0; i < m; ++i) {
            int x, y, z;
            scanf("%d%d%d", &x, & y, &z);
            add(x, y, z);
            add(y, x, z);
        }
        dijkstra(1);
        if(kase) printf("\n");
        printf("Scenario #%d:\n", ++kase);
        printf("%d\n", d[n]);
    }
    return 0;
}
```
