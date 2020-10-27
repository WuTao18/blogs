---
title: SPOJ VFMUL - Very Fast Multiplication (FFT)
date: 2019-09-18 21:15:29
tags:
- ACM刷题
- FFT
- 多项式
- 数学
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[VFMUL - Very Fast Multiplication](https://www.spoj.com/problems/VFMUL/en/)

## Description

> Multiply the given numbers.

<!--more-->

## Input
> n [the number of multiplications <= 101]
> 
> l1 l2 [numbers to multiply (at most 300000 decimal digits each)]
> 
> Text grouped in [ ] does not appear in the input file.

## Output
> The results of multiplications.

## Example
```markdown
Input:
5
4 2
123 43
324 342
0 12
9999 12345

Output:
8
5289
110808
0
123437655
```

**Warning: large Input/Output data, be careful with certain languages**

## Solution

### 题意

求两数的乘积

### 思路

**FFT**

FFT 求高精度乘法的模板题。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

const double PI = acos(-1);
typedef complex<double> Complex;
const int maxn = 3e6 + 10;

Complex a[maxn], b[maxn];
int m, n;
int bit = 2, rev[maxn];
int ans[maxn];

void get_rev(){
    memset(rev, 0, sizeof(rev));
    while(bit <= n + m) bit <<= 1;
    for(int i = 0; i < bit; ++i) {
        rev[i] = (rev[i >> 1] >> 1) | (bit >> 1) * (i & 1);
    }
}

void FFT(Complex *a, int op) {
    for(int i = 0; i < bit; ++i) {
        if(i < rev[i]) swap(a[i], a[rev[i]]);
    }
    for(int mid = 1; mid < bit; mid <<= 1) {
        Complex wn = Complex(cos(PI / mid), op * sin(PI / mid));
        for(int j = 0; j < bit; j += mid<<1) {
            Complex w(1, 0);
            for(int k = 0; k < mid; ++k, w = w * wn) {
                Complex x = a[j + k], y = w * a[j + k + mid];
                a[j + k] = x + y, a[j + k + mid] = x - y;
            }
        }
    }
}

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        memset(a, 0, sizeof(a));
        memset(b, 0, sizeof(b));
        string s1, s2;
        cin >> s1 >> s2;
        n = s1.size(), m = s2.size();
        for(int i = 0; i < n; ++i) {
            a[i] = s1[n - i - 1] - '0';
        }
        for(int i = 0; i < m; ++i) {
            b[i] = s2[m - i - 1] - '0';
        }
        get_rev();
        FFT(a, 1);
        FFT(b, 1);
        for(int i = 0; i <= bit; ++i) {
            a[i] *= b[i];
        }
        FFT(a, -1);
        for(int i = 0; i < n + m; ++i) {
            ans[i] = (int)(a[i].real() / bit + 0.5);
        }
        for(int i = 1; i < n + m; ++i) {
            ans[i] = ans[i] + ans[i - 1] / 10;
            ans[i - 1] = ans[i - 1] % 10;
        }
        int s = n + m - 1;
        for(; s >= 0; --s) {
            if(ans[s]) break;
        }
        if(s < 0) printf("0\n");
        else {
            for(int i = s; i >= 0; --i) {
                printf("%d", ans[i]);
            }
            printf("\n");
        }
    }
    return 0;
}
```