---
title: PTA 1154 Vertex Coloring
date: 2019-11-20 22:38:37
tags:
- ACM刷题
- 枚举
- STL
- 图论
categories: 竞赛
mathjax: true
---

题目链接：[1154 Vertex Coloring](https://pintia.cn/problem-sets/994805342720868352/problems/1071785301894295552)

> A **proper vertex coloring** is a labeling of the graph's vertices with colors such that no two vertices sharing the same edge have the same color. A coloring using at most $k$ colors is called a (proper) $k$-**coloring**.
> 
> Now you are supposed to tell if a given coloring is a proper $k$-coloring.

<!--more-->

## Input Specification:
> Each input file contains one test case. For each case, the first line gives two positive integers $N$ and $M$ (both no more than $10​^4$), being the total numbers of vertices and edges, respectively. Then $M$ lines follow, each describes an edge by giving the indices (from $0$ to $N−1$) of the two ends of the edge.
> 
> After the graph, a positive integer $K (≤ 100)$ is given, which is the number of colorings you are supposed to check. Then $K$ lines follow, each contains $N$ colors which are represented by non-negative integers in the range of **int**. The $i$-th color is the color of the $i$-th vertex.

## Output Specification:
> For each coloring, print in a line `k-coloring` if it is a proper `k`-coloring for some positive `k`, or `No` if not.

## Sample Input 1:
```markdown
10 11
8 7
6 8
4 5
8 4
8 1
1 2
1 4
9 8
9 1
1 0
2 4
4
0 1 0 1 4 1 0 1 3 0
0 1 0 1 4 1 0 1 0 0
8 1 0 1 4 1 0 5 3 0
1 2 3 4 5 6 7 8 8 9
```

## Sample Output:
```markdown
4-coloring
No
6-coloring
No
```

## 题意

给定一个图，以及每个顶点的颜色，问是否所有边连接的两个顶点颜色不同。

## 思路

枚举每一条边即可。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e4 + 10;

pair<int, int> edges[maxn];
int colors[maxn];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n, m;
    cin >> n >> m;
    for(int i = 0; i < m; ++i) {
        int x, y;
        cin >> x >> y;
        edges[i] = {x, y};
    }
    int k;
    cin >> k;
    while(k--) {
        map<int, int> mp;
        for(int i = 0; i < n; ++i) {
            cin >> colors[i];
            mp[colors[i]]++;
        }
        int flag = 1;
        for(int i = 0; i < m; ++i) {
            int x = edges[i].first, y = edges[i].second;
            if(colors[x] == colors[y]) {
                flag = 0;
                break;
            }
        }
        if(flag) {
            cout << mp.size() << "-coloring" << endl;
        } else {
            cout << "No" << endl;
        }
    }
    return 0;
}
```