---
title: POJ 1679 The Unique MST (次小生成树)
date: 2019-10-14 20:50:19
tags:
- ACM刷题
- Kruskal
- 最小生成树
- 图论
categories: 竞赛
mathjax: true
---

题目链接：[POJ 1679](http://poj.org/problem?id=1679)

## Description
> Given a connected undirected graph, tell if its minimum spanning tree is unique.
> 
> Definition 1 (Spanning Tree): Consider a connected, undirected graph G = (V, E). A spanning tree of G is a subgraph of G, say T = (V', E'), with the following properties:
> 
> 1. V' = V.
> 2. T is connected and acyclic.
> 
> Definition 2 (Minimum Spanning Tree): Consider an edge-weighted, connected, undirected graph G = (V, E). The minimum spanning tree T = (V, E') of G is the spanning tree that has the smallest total cost. The total cost of T means the sum of the weights on all the edges in E'.


## Input
> The first line contains a single integer t (1 <= t <= 20), the number of test cases. Each case represents a graph. It begins with a line containing two integers n and m (1 <= n <= 100), the number of nodes and edges. Each of the following m lines contains a triple (xi, yi, wi), indicating that xi and yi are connected by an edge with weight = wi. For any two nodes, there is at most one edge connecting them.
 
## Output
> For each input, if the MST is unique, print the total cost of it, or otherwise print the string 'Not Unique!'.


## Sample Input

```markdown
2
3 3
1 2 1
2 3 2
3 1 3
4 4
1 2 2
2 3 2
3 4 2
4 1 2
```

## Sample Output

```markdown
3
Not Unique!
```

## Source

[POJ Monthly--2004.06.27 srbga@POJ](http://poj.org/searchproblem?field=source&key=POJ+Monthly--2004.06.27+srbga%40POJ)

## Solution

### 题意

给定一个连通的无向图 (没有重边)，求最小生成树是否唯一。

### 思路

**Kruskal**

求出次小生成树，如果次小生成树的边权和与最小生成树的边权和相等，则最小生成树不唯一。

次小生成树的求法有很多，这里给出其中一种。

次小生成树与最小生成树仅有一条边不同。先用 Kruskal 求出最小生成树，然后枚举最小生成树的每一条边，求删去该边后的最小生成树，所有情况中的最小的生成树就是次小生成树。

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
const int N = 110, M = 1e5 + 10;
const int inf = 0x3f3f3f3f;
int n, m;

struct Edge {
    int x, y, z;
} edge[M];

int fa[N];

int cmp(Edge a, Edge b) {
    return a.z < b.z;
}

int get(int x) {
    if(x == fa[x]) return x;
    return fa[x] = get(fa[x]);
}

void init() {
    for(int i = 0; i <= N; ++i) {
        fa[i] = i;
    }
}

int mst[M];  // 最小生成树所有边的下标
int mst_size;  // 最小生成树的边权和

int kruskal() {
    init();
    int flag = 0;
    int ans = 0;
    mst_size = 0;
    sort(edge + 1, edge + 1 + m, cmp);
    for(int i = 1; i <= m; ++i) {
        int x = get(edge[i].x);
        int y = get(edge[i].y);
        if(x != y) {
            ans += edge[i].z;
            fa[x] = y;
            mst[++mst_size] = i;
        }
    }
    return ans;
}

// 次小生成树
int sec_mst() {
    int ans = inf;
    for(int i = 1; i <= mst_size; ++i) {
        init();
        int sum = 0;
        int cnt = 0;
        for(int j = 1; j <= m; ++j) {
            if(j != mst[i]) {  // 删去最小生成树中的一条边
                int x = get(edge[j].x);
                int y = get(edge[j].y);
                if(x != y) {
                    sum += edge[j].z;
                    fa[x] = y;
                    ++cnt;
                }
            }
        }
        if(cnt != mst_size) continue;
        ans = min(ans, sum);
    }
    if(ans == inf) return -1;
    return ans;
}

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        scanf("%d%d", &n, &m);
        for(int i = 1; i <= m; ++i) {
            scanf("%d%d%d", &edge[i].x, &edge[i].y, &edge[i].z);
        }
        int tmp1 = kruskal();
        int tmp2 = sec_mst();
        if(tmp1 == tmp2) {
            printf("Not Unique!\n");
        } else {
            printf("%d\n", tmp1);
        }
    }
    return 0;
}
```
