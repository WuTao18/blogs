---
title: Codeforces 1119E Pavel and Triangles (贪心)
date: 2019-09-27 21:53:38
tags:
- ACM刷题
- 贪心
- 思维
categories: 竞赛
mathjax: true
---

[Codeforces Global Round 2](https://codeforces.com/contest/1119)

题目链接：

[E. Pavel and Triangles](https://codeforces.com/problemset/problem/1119/E)

> Pavel has several sticks with lengths equal to powers of two.
> 
> He has $a_0$ sticks of length $2^0=1$, $a1$ sticks of length $2^1=2$, ..., $a_{n−1}$ sticks of length $2^{n−1}$.
> 
> Pavel wants to make the maximum possible number of triangles using these sticks. The triangles should have strictly positive area, each stick can be used in at most one triangle.
> 
> It is forbidden to break sticks, and each triangle should consist of exactly three sticks.
> 
> Find the maximum possible number of triangles.

<!--more-->

## Input

> The first line contains a single integer $n (1\le n\le 300000)$ — the number of different lengths of sticks.
> 
> The second line contains $n$ integers $a_0, a_1, ..., a_{n−1} (1\le a_i\le 10^9)$, where ai is the number of sticks with the length equal to $2^i$.

## Output

> Print a single integer — the maximum possible number of non-degenerate triangles that Pavel can make.

## Examples

> **input**
> 
> >     5 
> >     1 2 2 2 2 
> 
> **output**
> 
> >     3

> **input**
> 
> >     3
> >     1 1 1
> 
> **output**
> 
> >     0

> **input**
> 
> >     3
> >     3 3 3
> 
> **output**
> 
> >     3
> 

## Note
> In the first example, Pavel can, for example, make this set of triangles (the lengths of the sides of the triangles are listed): $(2^0,2^4,2^4), (2^1,2^3,2^3), (2^1,2^2,2^2)$.
> 
> In the second example, Pavel cannot make a single triangle.
> 
> In the third example, Pavel can, for example, create this set of triangles (the lengths of the sides of the triangles are listed): $(2^0,2^0,2^0), (2^1,2^1,2^1), (2^2,2^2,2^2)$.


## Solution

### 题意

给定 $n$ 个数，第 $i$ 个数 $a[i]$ 表示长度为 $2^i$ 的木棒的数量，求最多可以拼成多少个三角形。

### 题解

**贪心**

本来以为要 FFT 的，结果贪心就完事了。

构成三角形只有两种情况：等腰三角形和等边三角形。优先采用等边三角形，剩下的边去凑等腰三角形，贪心一下就可以了。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    ll n;
    cin >> n;
    ll ans = 0, k = 0;
    for(int i = 0; i < n; ++i) {
        ll a;
        cin >> a;
        if(k) {
            ll t = min(k, a / 2);
            a -= t * 2;
            ans += t;
            k -= t;
        }
        ans += a / 3;
        k += a % 3;
    }
    cout << ans << endl;
    return 0;
}
```