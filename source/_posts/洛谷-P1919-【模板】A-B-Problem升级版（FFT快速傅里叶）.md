---
title: 洛谷 P1919 【模板】A*B Problem升级版（FFT快速傅里叶）
date: 2019-09-16 22:00:52
tags:
- ACM刷题
- FFT
- 多项式
- 数学
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[P1919 【模板】A*B Problem升级版（FFT快速傅里叶）](https://www.luogu.org/problem/P1919)

## 题意

给定两个 $n$ 位 $10$ 进制整数 $x$ 和 $y$，求 $x*y$。

<!--more-->

## 思路

**FFT**

$FFT$ 的模板题，好像也可以直接用高精度乘法做。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const double PI = acos(-1);
const int maxn = 4e5 + 10;
typedef complex<double> Complex;

Complex a[maxn], b[maxn], c[maxn];
int ans[maxn];

void FFT(int n, Complex *a, int op) {
    if(n == 1) return;
    Complex a1[n>>1], a2[n>>1];
    for(int i = 0; i < n / 2; ++i) {
        a1[i] = a[2 * i];
        a2[i] = a[2 * i + 1];
    }
    FFT(n>>1, a1, op);
    FFT(n>>1, a2, op);
    Complex wn=Complex(cos(2 * PI / n), sin(2 * PI * op / n));
    Complex w=Complex(1, 0);
    for(int i = 0; i < (n>>1); ++i, w = w * wn) {
        a[i] = a1[i] + w * a2[i];
        a[i + (n>>1)] = a1[i] - w * a2[i];
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin >> n;
    string s1, s2;
    cin >> s1 >> s2;
    for(int i = 0; i < n; ++i) {
        a[i] = (double)(s1[n - i - 1] - '0');
        b[i] = (double)(s2[n - i - 1] - '0');
    }

    int m = 2 * (n - 1);
    n = 1;
    while(n <= m) n = n << 1;
    FFT(n, a, 1);
    FFT(n, b, 1);

    for(int i = 0; i <= n; ++i) {
        c[i] = a[i] * b[i];
    }
    FFT(n, c, -1);

    for(int i = 0; i <= m; ++i) {
        ans[i] = (int)(c[i].real() / n + 0.5);
    }

    for(int i = 1; i <= m; ++i) {
        ans[i] = ans[i] + ans[i - 1] / 10;
        ans[i - 1] = ans[i - 1] % 10;
    }

    int s = m;
    for(; s >= 0; --s) {
        if(ans[s]) break;
    }
    for(int i = s; i >= 0; --i) {
        cout << ans[i];
    }
    cout << endl;
    return 0;
}
```