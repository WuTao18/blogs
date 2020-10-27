---
title: POJ 3264 Balanced Lineup (ST算法)
date: 2019-09-04 19:03:09
tags:
- ACM刷题
- ST算法
- 倍增
- RMQ
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[POJ 3264](http://poj.org/problem?id=3264)

## Problem Description
> For the daily milking, Farmer John's $N$ cows $(1 \le N \le 50,000)$ always line up in the same order. One day Farmer John decides to organize a game of Ultimate Frisbee with some of the cows. To keep things simple, he will take a contiguous range of cows from the milking lineup to play the game. However, for all the cows to have fun they should not differ too much in height.
> 
> Farmer John has made a list of $Q (1 \le Q \le 200,000)$ potential groups of cows and their $heights (1 \le height \le 1,000,000)$. For each group, he wants your help to determine the difference in height between the shortest and the tallest cow in the group.

<!--more-->

## Input
> Line $1$: Two space-separated integers, $N$ and $Q$. 
> 
> Lines $2..N+1$: Line $i+1$ contains a single integer that is the height of cow $i $
> 
> Lines $N+2..N+Q+1$: Two integers $A$ and $B (1 ≤ A ≤ B ≤ N)$, representing the range of cows from $A$ to $B$ inclusive.
 
## Output
> Lines $1.. Q$: Each line contains a single integer that is a response to a reply and indicates the difference in height between the tallest and shortest cow in the range.
 

## Sample Input

```markdown
6 3
1
7
3
4
2
5
1 5
4 6
2 2
```

## Sample Output

```markdown
6
3
0
```

## Source

[USACO 2007 January Silver](http://poj.org/searchproblem?field=source&key=USACO+2007+January+Silver)

## Solution

### 题意

给定 $n$ 个数，$q$ 个询问。每个询问包含 $l$ 和 $r$，求区间 $[l, r]$ 内的最大值减最小值的差值。

### 题解

**ST算法**

ST 算法模板题。

## Code

```cpp
#include <cstdio>
#include <iostream>
#include <string>
#include <cstring>
#include <cmath>
#include <algorithm>
using namespace std;
const int maxn = 1e5 + 10;

int a[maxn];
int f[maxn][50], g[maxn][50];
int n, q;

void st_prework() {
    for(int i = 1; i <= n; ++i) {
        f[i][0] = a[i];
        g[i][0] = a[i];
    }
    int t = log(n) / log(2) + 1;
    for(int j = 1; j < t; ++j) {
        for(int i = 1; i <= n - (1 << j) + 1; ++i) {
            f[i][j] = max(f[i][j - 1], f[i + (1 << (j - 1))][j - 1]);
            g[i][j] = min(g[i][j - 1], g[i + (1 << (j - 1))][j - 1]);
        }
    }
}

int st_query(int l, int r) {
    int k = log(r - l + 1) / log(2);
    return max(f[l][k], f[r - (1 << k) + 1][k]) - min(g[l][k], g[r - (1 << k) + 1][k]);
}

int main() {
    scanf("%d%d", &n, &q);
    for(int i = 1; i <= n; ++i) {
        scanf("%d", &a[i]);
    }
    st_prework();
    for(int i = 0; i < q; ++i) {
        int l, r;
        scanf("%d%d", &l, &r);
        printf("%d\n", st_query(l, r));
    }
    return 0;
}
```
