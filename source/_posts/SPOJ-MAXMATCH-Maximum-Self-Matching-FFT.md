---
title: SPOJ MAXMATCH - Maximum Self-Matching (FFT)
date: 2019-09-24 23:29:39
tags:
- ACM刷题
- FFT
- 字符串
- 多项式
- 数学
categories: 竞赛
mathjax: true
---

题目链接：[MAXMATCH - Maximum Self-Matching](https://www.spoj.com/problems/MAXMATCH/en/)

## Description

> You're given a string **s** consisting of letters 'a', 'b' and 'c'.
> 
> The matching function $m_s( i )$ is defined as the number of matching characters of **s** and its i-shift. In other words, $m_s( i )$ is the number of characters that are matched when you align the 0-th character of **s** with the i-th character of its copy.
> 
> You are asked to compute the maximum of $m_s( i )$ for all i ( 1 <= i <= |**s**| ). To make it a bit harder, you should also output all the optimal i's in increasing order.

## Input
> The first and only line of input contains the string **s**. $2 \le |s| \le 10^5$.

## Output
> The first line of output contains the maximal $m_s( i )$ over all i.
> 
> The second line of output contains all the i's for which $m_s( i )$ reaches maximum.

## Example

```markdown
Input:
caccacaa

Output:
4
3

Explanation:

caccacaa
   caccacaa


 The bold characters indicate the ones that match when shift = 3.
```


## Solution

### 题意

给定一个字符串 $s$ (下标从 $0$ 开始，只包含 'a', 'b', 'c')，让 $s$ 与 $s$ 匹配，下标从 $1$ 移动到 $|s|$，每次匹配时的相同的字符个数记为 $m_s( i )$，求 $m_s( i )$ 的最大值，以及最大值所匹配的所有位置。

比如 ababa

```
ababa
 ababa
  ababa
   ababa
    ababa
```

$m_s( i )$ 分别为 $0, 3, 0, 1$，最大值为 $3$。

### 思路

**FFT**

字符串匹配问题。

设模式串为 $p$，目标串为 $t$，$f[k]$ 为模式串从目标串第 $k$ 位开始匹配的结果。

对 $a$，$b$，$c$ 分开求。

首先判断 $a$ 的情况，将字符串转化为 01 串，比如 ababa 转为 10101。

那么

$$f[k] = \sum_{i=0}^{|s|-k-1} p[i] \cdot t[k + i]$$

$$f[k] = \sum_{i=k}^{|s|-1} p[i - k] \cdot t[i]$$

将模式串倒置，有

$$f[k] = \sum_{i=k}^{|s|-1} p[|s| - 1 - i + k] \cdot t[i]$$

令 $j = |s| - 1 - i + k$，有

$$f[k] = \sum_{i+j=|s|-1+k} p[j] \cdot t[i]$$

用 $FFT$ 求一下卷积即可。

对于 $b$ 和 $c$ 的求法相同。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const double PI = acos(-1);
const double eps = 1e-8;
typedef complex<double> Complex;
const int maxn = 2e6 + 10;

Complex p[maxn], t[maxn];
Complex a[maxn], b[maxn], c[maxn];
int ans[maxn];
string str;
int n;
int bit = 2, rev[maxn];

void get_rev(){
    memset(rev, 0, sizeof(rev));
    while(bit <= n + n) bit <<= 1;
    for(int i = 0; i < bit; ++i) {
        rev[i] = (rev[i >> 1] >> 1) | (bit >> 1) * (i & 1);
    }
}

void FFT(Complex *arr, int op) {
    for(int i = 0; i < bit; ++i) {
        if(i < rev[i]) swap(arr[i], arr[rev[i]]);
    }
    for(int mid = 1; mid < bit; mid <<= 1) {
        Complex wn = Complex(cos(PI / mid), op * sin(PI / mid));
        for(int j = 0; j < bit; j += mid<<1) {
            Complex w(1, 0);
            for(int k = 0; k < mid; ++k, w = w * wn) {
                Complex x = arr[j + k], y = w * arr[j + k + mid];
                arr[j + k] = x + y, arr[j + k + mid] = x - y;
            }
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> str;
    n = str.size();
    for(int i = 0; i < n; ++i) {
        p[n - i - 1] = str[i] == 'a' ? 1 : 0;
        t[i] = p[n - i - 1];
    }
    get_rev();
    FFT(p, 1); FFT(t, 1);
    for(int i = 0; i < bit; ++i) {
        a[i] = p[i] * t[i];
    }
    FFT(a, -1);

    for(int i = 0; i < n; ++i) {
        p[n - i - 1] = str[i] == 'b' ? 1 : 0;
        t[i] = p[n - i - 1];
    }
    for(int i = n; i < bit; ++i) {
        p[i] = 0;
        t[i] = 0;
    }
    FFT(p, 1); FFT(t, 1);
    for(int i = 0; i < bit; ++i) {
        b[i] = p[i] * t[i];
    }
    FFT(b, -1);

    for(int i = 0; i < n; ++i) {
        p[n - i - 1] = str[i] == 'c' ? 1 : 0;
        t[i] = p[n - i - 1];
    }
    for(int i = n; i < bit; ++i) {
        p[i] = 0;
        t[i] = 0;
    }
    FFT(p, 1); FFT(t, 1);
    for(int i = 0; i < bit; ++i) {
        c[i] = p[i] * t[i];
    }
    FFT(c, -1);

    int maxa = 0;
    for(int i = 1; i < n; ++i) {
        ans[i] = (int)(a[n - 1 + i].real() / bit + 0.5) + (int)(b[n - 1 + i].real() / bit + 0.5) + (int)(c[n - 1 + i].real() / bit + 0.5); 
        maxa = max(maxa, ans[i]);
    }
    vector<int> pos;
    for(int i = 1; i < n; ++i) {
        if(ans[i] == maxa) {
            pos.push_back(i);
        }
    }
    cout << maxa << endl;
    for(int i = 0; i < pos.size(); ++i) {
        cout << pos[i] << " ";
    }
    cout << endl;
    
    return 0;
}
```