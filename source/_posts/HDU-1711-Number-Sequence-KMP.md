---
title: HDU 1711 Number Sequence (KMP)
date: 2019-10-29 21:54:55
tags:
- ACM刷题
- KMP
- 字符串
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1711](http://acm.hdu.edu.cn/showproblem.php?pid=1711)

## Problem Description

> Given two sequences of numbers : a[1], a[2], ...... , a[N], and b[1], b[2], ...... , b[M] (1 <= M <= 10000, 1 <= N <= 1000000). Your task is to find a number K which make a[K] = b[1], a[K + 1] = b[2], ...... , a[K + M - 1] = b[M]. If there are more than one K exist, output the smallest one.


## Input

> The first line of input is a number T which indicate the number of cases. Each case contains three lines. The first line is two numbers N and M (1 <= M <= 10000, 1 <= N <= 1000000). The second line contains N integers which indicate a[1], a[2], ...... , a[N]. The third line contains M integers which indicate b[1], b[2], ...... , b[M]. All integers are in the range of [-1000000, 1000000].

## Output

> For each test case, you should output one line which only contain K described above. If no such K exists, output -1 instead.


## Sample Input

```markdown
2
13 5
1 2 1 2 3 1 2 3 1 3 2 1 2
1 2 3 1 3
13 5
1 2 1 2 3 1 2 3 1 3 2 1 2
1 2 3 2 1
```

## Sample Output

```markdown
6
-1
```

## Source

> [HDU 2007-Spring Programming Contest](http://acm.hdu.edu.cn/search.php?field=problem&key=HDU+2007-Spring+Programming+Contest&source=1&searchmode=source)

## Solution

### 题意

给定两个大小分别为 $n$ 和 $m$ 的数组，求第二个数组在第一个数组中出现的位置。

### 思路

KMP 模板题。把字符串改成数组就行了。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e6 + 10, maxm = 1e4 + 10;

int a[maxn], b[maxm], pi[maxm];

// 前缀函数
void prefix_function(int n) {
    for(int i = 1; i < n; ++i) {
        int j = pi[i - 1];
        while(j > 0 && b[i] != b[j]) j = pi[j - 1];
        if(b[i] == b[j]) ++j;
        pi[i] = j;
    }
}

int kmp(int n, int m) {
    prefix_function(m);
    int i = 0, j = 0;
    while(i < n && j < m) {
        if(a[i] == b[j]) {
            ++i;
            ++j;
        } else {
            if(j == 0) ++i;
            else j = pi[j - 1];
        }
    }
    if(j == m) {
        return i - j + 1;
    }
    return -1;
}

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        int n, m;
        scanf("%d%d", &n, &m);
        for(int i = 0; i < n; ++i) {
            scanf("%d", &a[i]);
        }
        for(int i = 0; i < m; ++i) {
            scanf("%d", &b[i]);
        }
        int ans = kmp(n, m);
        printf("%d\n", ans);
    }
    return 0;
}
```
