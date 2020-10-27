---
title: HDU 1575 Tr A (矩阵快速幂)
date: 2019-08-27 16:54:42
tags:
- ACM刷题
- 模板题
- 矩阵
- 快速幂
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1575](http://acm.hdu.edu.cn/showproblem.php?pid=1575)

## Problem Description
> A为一个方阵，则Tr A表示A的迹（就是主对角线上各项的和），现要求Tr(A^k)%9973。  
 
<!--more-->

## Input
> 数据的第一行是一个T，表示有T组数据。 
> 
> 每组数据的第一行有n(2 <= n <= 10)和k(2 <= k < 10^9)两个数据。接下来有n行，每行有n个数据，每个数据的范围是[0,9]，表示方阵A的内容。  
 

## Output
> 对应每组数据，输出Tr(A^k)%9973。
 

## Sample Input

```markdown
2
2 2
1 0
0 1
3 99999999
1 2 3
4 5 6
7 8 9
```

## Sample Output

```markdown
2
2686
```

## Solution

### 题意

如题。

### 题解

**矩阵快速幂**

矩阵快速幂模板题。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 10 + 5;
const ll mod = 9973;

struct Matrix {
    int n, m;
    ll a[maxn][maxn];
    Matrix(int n = 0, int m = 0) : n(n), m(m) {}
    void input() {
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= n; ++j) {
                scanf("%lld", &a[i][j]);
            }
        }
    }
    void output() {
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= n; ++j) {
                printf("%lld", a[i][j]);
                printf("%s", j == n? "\n": " ");
            }
        }
    }
    void init() {
        memset(a, 0, sizeof(a));
    }
    void unit() {
        if(n == m) {
            init();
            for(int i = 1; i <= n; ++i) {
                a[i][i] = 1;
            }
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
        if(n == m) {
            Matrix a = *this;
            Matrix ans = Matrix(n, n);
            ans.unit();
            if(!b) return ans;
            while(b) {
                if(b & 1) ans = ans * a;
                a = a * a;
                b >>= 1;
            }
            return ans;
        }
    }
};

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        int n;
        ll k;
        scanf("%d%lld", &n, &k);
        Matrix m(n, n);
        m.input();
        m = m.qmod(k);
        ll ans = 0;
        for(int i = 1; i <= n; ++i) {
            ans = (ans + m.a[i][i]) % mod;
        }
        printf("%lld\n", ans);
    }
    return 0;
}
```
