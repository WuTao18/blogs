---
title: POJ 3070 Fibonacci  (矩阵快速幂)
date: 2019-08-27 18:56:30
tags:
- ACM刷题
- 模板题
- 矩阵
- 快速幂
categories: 竞赛
mathjax: true
---

题目链接：[POJ 3070](http://poj.org/problem?id=3070)

## Problem Description
> In the Fibonacci integer sequence, $F_0 = 0$, $F_1 = 1$, and $F_n = F_{n − 1} + F_{n − 2}$ for $n \ge 2$. For example, the first ten terms of the Fibonacci sequence are:
> 
> $$0,\ 1,\ 1,\ 2,\ 3,\ 5,\ 8,\ 13,\ 21,\ 34,\ ...$$
> 
> An alternative formula for the Fibonacci sequence is
> 
> ![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/08/27/1566896523034-1566896523038.png).
> 
> Given an integer $n$, your goal is to compute the last $4$ digits of $F_n$.

## Input
> The input test file will contain multiple test cases. Each test case consists of a single line containing $n$ (where $0 \le n \le 1,000,000,000$). The end-of-file is denoted by a single line containing the number $−1$.
 
## Output
> For each test case, print the last four digits of $F_n$. If the last four digits of $F_n$ are all zeros, print $‘0’$; otherwise, omit any leading zeros (i.e., print $F_n$ mod $10000$).
 

## Sample Input

```markdown
0
9
999999999
1000000000
-1
```

## Sample Output

```markdown
0
34
626
6875
```

## Solution

### 题意

给定正整数 $n$，求斐波那契数列第 $n$ 项。

### 题解

**矩阵快速幂**

矩阵快速幂模板题。

$$
 \left[
 \begin{matrix}
   F_{n + 1} & F_n \\
   F_n & F_{n - 1}
  \end{matrix}
  \right] =
  \left[
 \begin{matrix}
   1 & 1 \\
   1 & 0
  \end{matrix}
  \right] ^ n
$$

## Code

```cpp
#include <iostream>
#include <cstdio>
#include <cmath>
#include <cstring>
using namespace std;
typedef long long ll;
const int maxn = 1e2 + 5;
const ll mod = 1e4;

struct Matrix {
    int n, m;
    ll a[maxn][maxn];
    Matrix(int n = 0, int m = 0) : n(n), m(m) {}
    void input() {
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= m; ++j) {
                scanf("%lld", &a[i][j]);
            }
        }
    }
    void output() {
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= m; ++j) {
                printf("%lld", a[i][j]);
                printf("%s", j == m? "\n": " ");
            }
        }
    }
    void init() {
        memset(a, 0, sizeof(a));
    }
    void unit() {
        init();
        for(int i = 1; i <= n; ++i) {
            a[i][i] = 1;
        }
    }
    Matrix operator *(const Matrix b) {
        Matrix c(n, b.m);
        c.init();
        for(int i = 1; i <= c.n; ++i) {
            for(int k = 1; k <= m; ++k) {
                for(int j = 1; j <= c.m; ++j) {
                    c.a[i][j] = (c.a[i][j] + a[i][k] * b.a[k][j]) % mod;
                }
            }
        }
        return c;
    }
    Matrix qmod(ll b) {
        Matrix ans = Matrix(n, n);
        ans.unit();
        if(!b) return ans;
        while(b) {
            if(b & 1) ans = ans * (*this);
            *this = (*this) * (*this);
            b >>= 1;
        }
        return ans;
    }
};

int main() {
    int n;
    while(~scanf("%d", &n)) {
        if(n == -1) break;
        ll ans = 0;
        if(n) {
            Matrix m(2, 2);
            m.a[1][1] = 1;
            m.a[1][2] = 1;
            m.a[2][1] = 1;
            m.a[2][2] = 0;
            m = m.qmod(n - 1);
            ans = m.a[1][1] % mod;
        } 
        printf("%lld\n", ans);
    }
    return 0;
}
```

