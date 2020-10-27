---
title: POJ 2253 Frogger
date: 2019-10-11 23:11:04
tags:
- ACM刷题
- Dijkstra
- Floyd
- 最短路
- 图论
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2253](http://poj.org/problem?id=2253)

## Description

> Freddy Frog is sitting on a stone in the middle of a lake. Suddenly he notices Fiona Frog who is sitting on another stone. He plans to visit her, but since the water is dirty and full of tourists' sunscreen, he wants to avoid swimming and instead reach her by jumping.
> 
> Unfortunately Fiona's stone is out of his jump range. Therefore Freddy considers to use other stones as intermediate stops and reach her by a sequence of several small jumps.
> 
> To execute a given sequence of jumps, a frog's jump range obviously must be at least as long as the longest jump occuring in the sequence.
> 
> The frog distance (humans also call it minimax distance) between two stones therefore is defined as the minimum necessary jump range over all possible paths between the two stones.
> 
> You are given the coordinates of Freddy's stone, Fiona's stone and all other stones in the lake. Your job is to compute the frog distance between Freddy's and Fiona's stone.

<!--more-->

## Input

> The input will contain one or more test cases. The first line of each test case will contain the number of stones n (2<=n<=200). The next n lines each contain two integers xi,yi (0 <= xi,yi <= 1000) representing the coordinates of stone #i. Stone #1 is Freddy's stone, stone #2 is Fiona's stone, the other n-2 stones are unoccupied. There's a blank line following each test case. Input is terminated by a value of zero (0) for n.
 
## Output

> For each test case, print a line saying "Scenario #x" and a line saying "Frog Distance = y" where x is replaced by the test case number (they are numbered from 1) and y is replaced by the appropriate real number, printed to three decimals. Put a blank line after each test case, even after the last one.

## Sample Input

```markdown
2
0 0
3 4

3
17 4
19 4
18 5

0
```

## Sample Output

```markdown
Scenario #1
Frog Distance = 5.000

Scenario #2
Frog Distance = 1.414
```

## Source

[Ulm Local 1997](http://poj.org/searchproblem?field=source&key=Ulm+Local+1997)

## Solution

### 题意

给定 $n$ 个点的坐标，求从点 $1$ 到点 $2$ 的所有路径的最大边权的最小值。

比如有三条路的边权如下:

2->5->3->4 最大边权为 5

1->8 最大边权为 8

4->4->4->4->4 最大边权为 4

所以答案为 4。

### 思路

**Dijkstra 或 Floyd**

只需修改一下 $Dijkstra$ 的松弛方程：$if\ d[v] > max(d[u], w[u][v])\ then\ d[v] = max(d[u], w[u][v])$ 

```cpp
#include <iostream>
#include <cstdio>
#include <queue>
#include <map>
#include <cmath>
#include <algorithm>
#include <cstring>
using namespace std;
const double inf = 1e18;
const int N = 210;

int n;
double d[N];
bool v[N];
double mp[N][N];

struct Point {
    double x, y;
    void input() {
        scanf("%lf%lf", &x, &y);
    }
    double dis(Point a) {
        return sqrt(pow(x - a.x, 2) + pow(y - a.y, 2));
    }
}p[N];

priority_queue<pair<int, int> >q;

void dijkstra(int s) {
    for(int i = 0; i <= n; ++i) {
        d[i] = inf;
    }
    memset(v, 0, sizeof(v));
    d[s] = 0;
    q.push(make_pair(0, s));
    while(q.size()) {
        int x = q.top().second; q.pop();
        if(v[x]) continue;
        v[x] = 1;
        for(int i  = 1; i <= n; ++i) {
            if(mp[i][x] < inf && d[i] > max(d[x], mp[i][x])) {
                d[i] = max(d[x], mp[i][x]);
                q.push(make_pair(-d[i], i));
            }
        }
    }
}

void init() {
    for(int i = 1; i < N; ++i) {
        for(int j = 1; j < N; ++j) {
            mp[i][j] = inf;
        }
    }
}

int main() {
    int kase = 0;
    while(~scanf("%d", &n) && n) {
        init();
        for(int i = 1; i <= n; ++i) {
            p[i].input();
        }
        for(int i = 1; i <= n; ++i) {
            for(int j = i + 1; j <= n; ++j) {
                double z = p[i].dis(p[j]);
                mp[i][j] = mp[j][i] = z;
            }
        }
        dijkstra(1);
        printf("Scenario #%d\n", ++kase);
        printf("Frog Distance = %.3lf\n\n", d[2]);
    }
    return 0;
}
```

同样，修改一下 $Floyd$ 的状态转移方程：$w[i][j] = min(w[i][j], max(w[i][k], w[k][j]))$

```cpp
#include <iostream>
#include <cstdio>
#include <queue>
#include <map>
#include <cmath>
#include <algorithm>
#include <cstring>
using namespace std;
const double inf = 1e18;
const int N = 210;

int n;
bool v[N];
double mp[N][N];

struct Point {
    double x, y;
    void input() {
        scanf("%lf%lf", &x, &y);
    }
    double dis(Point a) {
        return sqrt(pow(x - a.x, 2) + pow(y - a.y, 2));
    }
}p[N];

void floyd() {
    for(int k = 1; k <= n; ++k) {
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= n; ++j) {
                mp[i][j] = min(mp[i][j], max(mp[i][k], mp[k][j]));
            }
        }
    }
}

void init() {
    for(int i = 1; i < N; ++i) {
        for(int j = 1; j < N; ++j) {
            mp[i][j] = inf;
        }
    }
}

int main() {
    int kase = 0;
    while(~scanf("%d", &n) && n) {
        init();
        for(int i = 1; i <= n; ++i) {
            p[i].input();
        }
        for(int i = 1; i <= n; ++i) {
            for(int j = i + 1; j <= n; ++j) {
                double z = p[i].dis(p[j]);
                mp[i][j] = mp[j][i] = z;
            }
        }
        floyd();
        printf("Scenario #%d\n", ++kase);
        printf("Frog Distance = %.3lf\n\n", mp[1][2]);
    }
    return 0;
}
```