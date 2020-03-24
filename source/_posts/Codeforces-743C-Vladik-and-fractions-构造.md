---
title: Codeforces 743C - Vladik and fractions (构造)
date: 2019-09-03 21:46:21
tags:
- ACM刷题
- 构造
- 数学
categories: 竞赛
mathjax: true
---

[Codeforces Round #384 (Div. 2)](http://codeforces.com/contest/743)

题目链接：[Vladik and fractions](http://codeforces.com/problemset/problem/743/C)

> Vladik and Chloe decided to determine who of them is better at math. Vladik claimed that for any positive integer $n$ he can represent fraction $\frac{2}{n}$ as a sum of three distinct positive fractions in form $\frac{1}{m}$.
> 
> Help Vladik with that, i.e for a given $n$ find three distinct positive integers $x, y$ and $z$ such that $\frac{2}{n} = \frac{1}{x} + \frac{1}{y} + \frac{1}{z}$. Because Chloe can't check Vladik's answer if the numbers are large, he asks you to print numbers not exceeding $10^9$.
> 
> If there is no such answer, print $-1$.

## Input

> The single line contains single integer $n (1 \le  n \le  10^4)$.

## Output

> If the answer exists, print 3 distinct numbers $x, y$ and $z (1 \le  x, y, z \le  10^9, x \neq y, x \neq  z, y \neq  z)$. Otherwise print $-1$.
> 
> If there are multiple answers, print any of them.

## Examples

> **input**
> 
> >     3
> 
> **output**
> 
> >     2 7 42
> 
> **input**
> 
> >     7
> 
> **output**
> 
> >     7 8 56
> 


## Solution

### 题意

给定一个正整数 $n$，求正整数 $x,y,z$ 满足 $\frac{2}{n} = \frac{1}{x} + \frac{1}{y} + \frac{1}{z}$。

其中 $x \neq y,\ x \neq z,\ y \neq z$。若无解输出 $-1$。

### 题解

**构造**

$$\frac{1}{n} - \frac{1}{n + 1} = \frac{1}{n(n+1)}$$

$$\frac{1}{n} = \frac{1}{n + 1} + \frac{1}{n(n+1)}$$

$$\frac{2}{n} = \frac{1}{n + 1} + \frac{1}{n(n+1)} + \frac{1}{n}$$

当 $n=1$ 时，$\frac{2}{n}=2$。而 $(\frac{1}{x}+\frac{1}{y}+\frac{1}{z})_{max} = \frac{1}{1}+\frac{1}{2}+\frac{1}{3} < 2$，所以当 $n=1$ 时无解。

## Code 

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin >> n;
    if(n == 1) cout << -1 << endl;
    else cout << (n + 1) << " " << (n * (n + 1)) << " " << n << endl;
    return 0;
}
```