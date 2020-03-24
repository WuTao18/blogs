---
title: HDU 5443 The Water Problem (ST算法)
date: 2019-09-04 19:01:38
tags:
- ACM刷题
- ST算法
- 倍增
- RMQ
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[HDU 5443](http://acm.hdu.edu.cn/showproblem.php?pid=5443)

## Problem Description
> In Land waterless, water is a very limited resource. People always fight for the biggest source of water. Given a sequence of water sources with $a_1,a_2,a_3,...,a_n$ representing the size of the water source. Given a set of queries each containing $2$ integers $l$ and $r$, please find out the biggest water source between $a_l$ and $a_r$.
 

## Input
> First you are given an integer $T(T\le 10)$ indicating the number of test cases. For each test case, there is a number $n(0\le n\le 1000)$ on a line representing the number of water sources. $n$ integers follow, respectively $a_1,a_2,a_3,...,a_n$, and each integer is in ${1,...,10^6}$. On the next line, there is a number $q(0\le q\le 1000)$ representing the number of queries. After that, there will be $q$ lines with two integers $l$ and $r(1\le l\le r\le n)$ indicating the range of which you should find out the biggest water source.
 

## Output
> For each query, output an integer representing the size of the biggest water source.
 

## Sample Input

```markdown
3
1
100
1
1 1
5
1 2 3 4 5
5
1 2
1 3
2 4
3 4
3 5
3
1 999999 1
4
1 1
1 2
2 3
3 3
```

## Sample Output

```markdown
100
2
3
4
4
5
1
999999
999999
1
```

## Source

[2015 ACM/ICPC Asia Regional Changchun Online](http://acm.hdu.edu.cn/search.php?field=problem&key=2015+ACM%2FICPC+Asia+Regional+Changchun+Online&source=1&searchmode=source)

## Solution

### 题意

给定 $n$ 个数，$q$ 个询问，每个询问包含 $l$ 和 $r$，求区间 $[l, r]$ 内的最大值。

### 思路

**ST算法**

$RMQ$ 问题。ST 算法模板题。预处理时间 $O(nlogn)$，查询时间 $O(1)$。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1010;

int a[maxn];
int f[maxn][11];
int n;

void st_prework() {
    for(int i = 1; i <= n; ++i) f[i][0] = a[i];
    int t = log(n) / log(2) + 1;
    for(int j = 1; j < t; ++j) {
        for(int i = 1; i <= n - (1 << j) + 1; ++i) {
            f[i][j] = max(f[i][j - 1], f[i + (1 << (j - 1))][j - 1]);
        }
    }
}

int st_query(int l, int r) {
    int k = log(r - l + 1) / log(2);
    return max(f[l][k], f[r - (1 << k) + 1][k]);
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    while(T--) {
        cin >> n;
        for(int i = 1; i <= n; ++i) {
            cin >> a[i];
        }
        st_prework();
        int q;
        cin >> q;
        for(int i = 0; i < q; ++i) {
            int l, r;
            cin >> l >> r;
            cout << st_query(l, r) << endl;
        }
    }
    return 0;
}
```
