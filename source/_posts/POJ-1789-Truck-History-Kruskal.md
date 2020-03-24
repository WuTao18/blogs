---
title: POJ 1789 Truck History (Kruskal)
date: 2019-10-15 22:29:35
tags:
- ACM刷题
- Kruskal
- 最小生成树
- 图论
categories: 竞赛
mathjax: true
---

题目链接：[POJ 1789](http://poj.org/problem?id=1789)

## Description
> Advanced Cargo Movement, Ltd. uses trucks of different types. Some trucks are used for vegetable delivery, other for furniture, or for bricks. The company has its own code describing each type of a truck. The code is simply a string of exactly seven lowercase letters (each letter on each position has a very special meaning but that is unimportant for this task). At the beginning of company's history, just a single truck type was used but later other types were derived from it, then from the new types another types were derived, and so on.
> 
> Today, ACM is rich enough to pay historians to study its history. One thing historians tried to find out is so called derivation plan -- i.e. how the truck types were derived. They defined the distance of truck types as the number of positions with different letters in truck type codes. They also assumed that each truck type was derived from exactly one other truck type (except for the first truck type which was not derived from any other type). The quality of a derivation plan was then defined as
> 
> $1/Σ_{(t_o,t_d)}d(t_o,t_d)$
> 
> where the sum goes over all pairs of types in the derivation plan such that $t_o$ is the original type and $t_d$ the type derived from it and d($t_o$,$t_d$) is the distance of the types.
Since historians failed, you are to write a program to help them. Given the codes of truck types, your program should find the highest possible quality of a derivation plan.


## Input
> The input consists of several test cases. Each test case begins with a line containing the number of truck types, N, 2 <= N <= 2 000. Each of the following N lines of input contains one truck type code (a string of seven lowercase letters). You may assume that the codes uniquely describe the trucks, i.e., no two of these N lines are the same. The input is terminated with zero at the place of number of truck types.
 
## Output
> For each test case, your program should output the text "The highest possible quality is 1/Q.", where 1/Q is the quality of the best derivation plan.


## Sample Input

```markdown
4
aaaaaaa
baaaaaa
abaaaaa
aabaaaa
0
```

## Sample Output

```markdown
The highest possible quality is 1/3.
```

## Source

[CTU Open 2003](http://poj.org/searchproblem?field=source&key=CTU+Open+2003)

## Solution

### 题意

用一个 $7$ 位的字符串代表一个编号，两个编号之间的距离等于这两个编号之间不同字母的个数。

给定 $n$ 个编号，求连接所有编号的最短距离。

### 思路

**Kruskal**

把每个字符串看成结点，用无向边连接任意两个结点，边权为两个字符串之间的距离，对构成的无向图求最小生成树就是答案。

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
const int N = 2010, M = 4e6 + 10;
const int inf = 0x3f3f3f3f;
int n, m;
int ans;

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
    for(int i = 0; i <= n; ++i) {
        fa[i] = i;
    }
    ans = 0;
}

void kruskal() {
    sort(edge + 1, edge + 1 + m, cmp);
    for(int i = 1; i <= m; ++i) {
        int x = get(edge[i].x);
        int y = get(edge[i].y);
        if(x != y) {
            ans += edge[i].z;
            fa[x] = y;
        }
    }
}

char str[N][10];

int dis(int x, int y) {
    int res = 0;
    for(int i = 0; i < 7; ++i) {
        if(str[x][i] != str[y][i]) {
            ++res;
        }
    }
    return res;
}

int main() {
    while(scanf("%d", &n) && n) {
        for(int i = 1; i <= n; ++i) {
            scanf("%s", str[i]);
        }
        init();
        m = 0;
        for(int i = 1; i <= n; ++i) {
            for(int j = i + 1; j <= n; ++j) {
                edge[++m].x = i;
                edge[m].y = j;
                edge[m].z = dis(i, j);
            }
        }
        kruskal();
        printf("The highest possible quality is 1/%d.\n", ans);
    }
    return 0;
}
```
