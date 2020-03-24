---
title: POJ 3268 Silver Cow Party (Dijkstra)
date: 2019-10-13 20:18:22
tags:
- ACM刷题
- Dijkstra
- 最短路
- 图论
categories: 竞赛
mathjax: true
---

题目链接：[POJ 3268](http://poj.org/problem?id=3268)

## Description
> One cow from each of $N$ farms $(1 ≤ N ≤ 1000)$ conveniently numbered $1..N$ is going to attend the big cow party to be held at farm #$X (1 ≤ X ≤ N)$. A total of $M (1 ≤ M ≤ 100,000)$ unidirectional (one-way roads connects pairs of farms; road $i$ requires $T_i (1 ≤ T_i ≤ 100)$ units of time to traverse.
> 
> Each cow must walk to the party and, when the party is over, return to her farm. Each cow is lazy and thus picks an optimal route with the shortest time. A cow's return route might be different from her original route to the party since roads are one-way.
> 
> Of all the cows, what is the longest amount of time a cow must spend walking to the party and back?


## Input
> Line $1$: Three space-separated integers, respectively: $N$, $M$, and $X$
> 
> Lines $2.. M+1$: Line $i+1$ describes road $i$ with three space-separated integers: $A_i$, $B_i$, and $T_i$. The described road runs from farm $A_i$ to farm $B_i$, requiring $T_i$ time units to traverse.
 
## Output
> Line 1: One integer: the maximum of time any one cow must walk.

## Sample Input

```markdown
4 8 2
1 2 4
1 3 2
1 4 7
2 1 1
2 3 5
3 1 2
3 4 4
4 2 3
```

## Sample Output

```markdown
10
```

## Hint

> Cow 4 proceeds directly to the party (3 units) and returns via farms 1 and 3 (7 units), for a total of 10 time units.

## Source

[USACO 2007 February Silver](http://poj.org/searchproblem?field=source&key=USACO+2007+February+Silver)

## Solution

### 题意

给定 $n$ 个点和 $m$ 条边的有向图，每个点有一头牛，牛 $a$ 到牛 $b$ 需花费的时间为 $t$，现在所有的牛要到牛 $x$ 所在的点去，求所有牛去牛 $x$ 处再返回的最短时间中的最大值。

### 思路

**Dijkstra**

对原图和反向建边的图各跑一次 $Dijkstra$，两次的 $d$ 数组的和就是每头牛的往返的最短时间，然后找出其中的最大值即可。

## Code

```cpp
#include <iostream>
#include <cstdio>
#include <queue>
#include <map>
#include <cmath>
#include <algorithm>
#include <cstring>
#include <functional>
using namespace std;
const int N = 1010, M = 1e5 + 10;
const int inf = 0x3f3f3f3f;
typedef pair<int, int> P;
int n, m, x;
int s[M], e[M], w[M];

struct Edge {
    int to, w;
    Edge(int to, int w): to(to), w(w) {}
};
vector<Edge> G[N];
int d[N], vis[N];
int cnt[N];

void init() {
    for(int i = 0; i < N; ++i) {
        G[i].clear();
    }
}

void add(int x, int y, int z) {
    G[x].push_back(Edge(y, z));
}

void dijkstra(int s) {
    priority_queue<P,vector<P>,greater<P> > q;
    memset(d, 0x3f, sizeof(d));
    memset(vis, 0, sizeof(vis));
    d[s] = 0;
    q.push(P(0, s));
    while(q.size()) {
        P p = q.top(); q.pop();
        int x = p.second;
        if(vis[x]) continue;
        vis[x] = 1;
        for(int i = 0; i < G[x].size(); ++i) {
            Edge e = G[x][i];
            if (d[e.to] > d[x] + e.w) {
                d[e.to] = d[x] + e.w;
                q.push(P(d[e.to], e.to));
            }
        }
    }
}

int main() {
    init();
    scanf("%d%d%d", &n, &m, &x);
    for(int i = 1; i <= m; ++i) {
        scanf("%d%d%d", &s[i], &e[i], &w[i]);
        add(s[i], e[i], w[i]);
    }
    dijkstra(x);
    for(int i = 1; i <= n; ++i) {
        cnt[i] = d[i];
    }
    init();
    for(int i = 1; i <= m; ++i) {
        add(e[i], s[i], w[i]);
    }
    dijkstra(x);
    int ans = 0;
    for(int i = 1; i <= n; ++i) {
        ans = max(ans, cnt[i] + d[i]);
    }
    printf("%d\n", ans);
    return 0;
}
```
