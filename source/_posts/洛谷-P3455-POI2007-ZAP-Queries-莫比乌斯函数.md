---
title: '洛谷 P3455 [POI2007]ZAP-Queries (莫比乌斯函数)'
date: 2019-10-08 21:15:04
tags:
- ACM刷题
- 莫比乌斯函数
- 数论
categories: 竞赛
mathjax: true
---

题目链接：[P3455 [POI2007]ZAP-Queries](https://www.luogu.org/problem/P3455)

## 题意

> 给定 $a,b,d$，求 $\sum_{x=1}^{a} \sum_{y=1}^{b}[gcd(x, y) = d]$。

## 思路

莫比乌斯函数的一个性质：

$$[x = 1] = \sum_{d|x} \mu(d)$$

设 $a \le b$，对原式转化：

$$\sum_{x=1}^{a} \sum_{y=1}^{b}[gcd(x, y) = d] \\
= \sum_{x=1}^{\lfloor \frac{a}{d} \rfloor} \sum_{y=1}^{\lfloor \frac{b}{d} \rfloor}[gcd(x, y) = 1] \\
= \sum_{x=1}^{\lfloor \frac{a}{d} \rfloor} \sum_{y=1}^{\lfloor \frac{b}{d} \rfloor} \sum_{d'|gcd(x, y)} \mu(d') \\
= \sum_{x=1}^{\lfloor \frac{a}{d} \rfloor} \sum_{y=1}^{\lfloor \frac{b}{d} \rfloor} \sum_{d'=1}^{\lfloor \frac{a}{d} \rfloor} \mu(d') \cdot [d'|gcd(x, y)] \\
= \sum_{d'=1}^{\lfloor \frac{a}{d} \rfloor} \mu(d') \sum_{x=1}^{\lfloor \frac{a}{d} \rfloor} \sum_{y=1}^{\lfloor \frac{b}{d} \rfloor} [d'|gcd(x, y)] \\
= \sum_{d'=1}^{\lfloor \frac{a}{d} \rfloor} \mu(d') \sum_{x=1}^{\lfloor \frac{a}{d} \rfloor} \sum_{y=1}^{\lfloor \frac{b}{d} \rfloor} [d'|x \wedge d'|y] \\
= \sum_{d'=1}^{\lfloor \frac{a}{d} \rfloor} \mu(d') \sum_{x=1}^{\lfloor \frac{a}{d} \rfloor} [d'|x] \sum_{y=1}^{\lfloor \frac{b}{d} \rfloor} [d'|y] \\
= \sum_{d'=1}^{\lfloor \frac{a}{d} \rfloor} \mu(d') \lfloor \frac{a}{dd'} \rfloor {\lfloor \frac{b}{dd'} \rfloor}\\
$$

然后预处理 $\mu$ 的前缀和，用一下**整除分块**求解。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 5e4 + 10;
ll a, b, d;

ll mu[maxn], vis[maxn], sum[maxn];
vector<ll> p;

// 莫比乌斯函数的前缀和
void init() {
    sum[1] = mu[1] = 1;
    for(int i = 2; i < maxn; ++i) {
        if(!vis[i]) {
            mu[i] = -1;
            p.push_back(i);
        }
        for(int j = 0; j < p.size() && i * p[j] < maxn; ++j) {
            vis[i * p[j]] = 1;
            if(i % p[j] == 0) break;
            mu[i * p[j]] = -mu[i];
        }
    }
    for(int i = 2; i < maxn; ++i) {
        sum[i] = sum[i - 1] + mu[i];
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    init();
    int T;
    cin >> T;
    while(T--) {
        cin >> a >> b >> d;
        if(a > b) {
            swap(a, b);
        }
        a /= d;
        b /= d;
        ll ans = 0;
        // 整除分块
        for(int i = 1, j; i <= a; i = j + 1) {
            j = min(a / (a / i), b / (b / i));
            ans += (sum[j] - sum[i - 1]) * (a / i) * (b / i);
        }
        cout << ans << endl;
    }
    return 0;
}
```