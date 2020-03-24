---
title: Codeforces 578B "Or" Game (前缀和 + 贪心)
date: 2019-10-01 23:22:59
tags:
- ACM刷题
- 贪心
- 前缀和
- 枚举
- 思维
categories: 竞赛
mathjax: true
---

[Codeforces Round #320 (Div. 1) [Bayan Thanks-Round]](https://codeforces.com/contest/578)

题目链接：[B. "Or" Game](https://codeforces.com/problemset/problem/578/B)

> You are given $n$ numbers $a_1, a_2, ..., a_n$. You can perform at most $k$ operations. For each operation you can multiply one of the numbers by $x$. We want to make $a_1 | a_2 | ... | a_n$ as large as possible, where $|$ denotes the bitwise OR.
> 
> Find the maximum possible value of $a_1 | a_2 | ... | a_n$ after performing at most $k$ operations optimally.

## Input

> The first line contains three integers $n$, $k$ and $x (1 ≤ n ≤ 200 000, 1 ≤ k ≤ 10, 2 ≤ x ≤ 8)$.
> 
> The second line contains $n$ integers $a_1, a_2, ..., a_n (0 ≤ a_i ≤ 10^9)$.

## Output

> Output the maximum value of a bitwise OR of sequence elements after performing operations.

## Examples

> **input**
> 
> >     3 1 2 
> >     1 1 1 
> 
> **output**
> 
> >     3

> **input**
> 
> >     4 2 3
> >     1 2 4 8
> 
> **output**
> 
> >     79


## Note
> For the first sample, any possible choice of doing one operation will result the same three numbers $1, 1, 2$ so the result is $1 | 1 | 2 = 3$.
> 
> For the second sample if we multiply $8$ by $3$ two times we'll get $72$. In this case the numbers will become $1, 2, 4, 72$ so the OR value will be $79$ and is the largest possible result.


## Solution

### 题意

给定 $n$ 个数 $a_1 ... a_n$，可以进行 $k$ 次操作，每次可以给任意一个数乘上 $x$，求 $a_1 | a_2 | ... | a_n$ 最大为多少。

### 题解

**贪心 前缀和**

因为 $x \ge 2$，因此一个数乘上 $x$ 后二进制位数必然增加一位。

由于或运算是 $0 | 1$ 时才会使答案增加，因此让 $k$ 个 $x$ 乘在同一个数上就行。

计算一下前缀和和后缀和，然后暴力枚举每一个数乘以 $x^k$，找到最大值即可。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 2e5 + 10;
typedef long long ll;

ll a[maxn], s1[maxn], s2[maxn];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    ll n, k, x;
    cin >> n >> k >> x;
    for(int i = 1; i <= n; ++i) {
        cin >> a[i];
        s1[i] = s1[i - 1] | a[i];
    }
    for(int i = n; i >= 1; --i) {
        s2[i] = s2[i + 1] | a[i];
    }
    ll ans = 0;
    ll tmp = x;
    for(int i = 2; i <= k; ++i) {
        tmp *= x;
    }
    for(int i = 1; i <= n; ++i) {
        ans = max(ans, s1[i - 1] | tmp * a[i] | s2[i + 1]);
    }
    cout << ans << endl;
    return 0;
}
```