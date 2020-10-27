---
title: Codeforces 993E Nikita and Order Statistics (FFT)
date: 2019-09-22 15:45:43
tags:
- ACM刷题
- FFT
- 构造
- 多项式
- 数学
categories: 竞赛
mathjax: true
---

[Codeforces Round #488 by NEAR (Div. 1)](https://codeforces.com/contest/993)

题目链接：

[Nikita and Order Statistics](https://codeforces.com/problemset/problem/993/E)

[CF993E Nikita and Order Statistics](https://www.luogu.org/problem/CF993E)

> Nikita likes tasks on order statistics, for example, he can easily find the $k$-th number in increasing order on a segment of an array. But now Nikita wonders how many segments of an array there are such that a given number $x$ is the $k$-th number in increasing order on this segment. In other words, you should find the number of segments of a given array such that there are exactly $k$ numbers of this segment which are less than $x$.
> 
> Nikita wants to get answer for this question for each $k$ from $0$ to $n$, where $n$ is the size of the array.

<!--more-->

## Input

> The first line contains two integers $n$ and $x (1 \le n \le 2 \cdot 10^5, -10^9 \le x \le 10^9)$.
> 
> The second line contains $n$ integers $a_1,a_2,…,a_n (-10^9 \le a_i \le 10^9)$ — the given array.

## Output

> Print $n+1$ integers, where the $i$-th number is the answer for Nikita's question for $k=i−1$.

## Examples

> **input**
> 
> >     5 3
> >     1 2 3 4 5
> 
> **output**
> 
> >     6 5 4 0 0 0

> **input**
> 
> >     2 6
> >     -5 9
> 
> **output**
> 
> >     1 2 0

> **input**
> 
> >     6 99
> >     -1 -1 -1 -1 -1 -1
> 
> **output**
> 
> >     0 6 5 4 3 2 1
> 


## Solution

### 题意

给定长度为 $n$ 的数组，对于 $k \in [0, n]$，求有多少个区间满足区间内恰好有 $k$ 个数比 $x$ 小。

### 题解

**FFT**

首先，对于数组中的每一个数，如果比 $x$ 小，那么把这个数置为 $1$，否则置为 $0$。然后求该数组的前缀和 $s$。

设 $f[i]$ 表示前缀和等于 $i$ 的个数。那么 $ans[k] = \sum_{i=k}^nf[i]f[i-k] = \sum_{i=k}^nf[i]f[n-(n+k-i)] = \sum_{i+j=n+k}f[i]f[n-j]$。

令 $g[i] = f[n - i]$，则 $ans[k] = \sum_{i+j=n+k}f[i]g[j]$。

问题就转化为求 $f$ 与 $g$ 的卷积。

注意 $k = 0$ 的情况要特判。此时会有 $n + 1$ 次自己和自己算到一起，也就是空串的情况。所以要对 $ans[0]$ 减去 $n + 1$。减完后还要除以 $2$，因为 $k \neq 0$ 时，由于 $s$ 单调不减，因此 $s[i] \ge s[i - k]$。此时一定保证区间的左端点在前，右端点在后。而 $k = 0$ 时，不能保证左端点一定在前，右端点一定在后，所以要除以 $2$。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const double PI = acos(-1);
const int maxn = 1e6 + 10;
typedef complex<double> Complex;

ll n, x;
ll s[maxn], cnt[maxn];
Complex f[maxn], g[maxn];
ll ans[maxn];
int bit = 2, rev[maxn];

void get_rev(){
    memset(rev, 0, sizeof(rev));
    while(bit <= n * 2) bit <<= 1;
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
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n >> x;
    ++cnt[0];
    for(int i = 1; i <= n; ++i) {
        int a;
        cin >> a;
        s[i] = s[i - 1] + (a < x);
        ++cnt[s[i]];
    }
    for(int i = 0; i <= n; ++i) {
        f[i] = cnt[i];
        g[n - i] = cnt[i];
    }
    get_rev();
    FFT(f, 1), FFT(g, 1);
    for(int i = 0; i < bit; ++i) {
        f[i] = f[i] * g[i];
    }
    FFT(f, -1);
    for(int i = 0; i <= n; ++i) {
        ans[i] = (ll)(f[n + i].real() / bit + 0.5);
    }
    cout << (ans[0] - n - 1) / 2 << " ";
    for(int i = 1; i <= n; ++i) {
        cout << ans[i] << " ";
    }
    cout << endl;
    return 0;
}
```