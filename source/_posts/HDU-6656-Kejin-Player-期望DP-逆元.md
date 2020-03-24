---
title: HDU 6656 Kejin Player (期望DP 逆元)
date: 2019-08-13 23:52:05
tags:
- ACM刷题
- 期望DP
- 逆元
- 动态规划
categories: 竞赛
mathjax: true
---

2019 杭电多校 7 1011

题目链接：[HDU 6656](http://acm.hdu.edu.cn/showproblem.php?pid=6656)

比赛链接：[2019 Multi-University Training Contest 7](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+7&source=1&searchmode=source)


## Problem Description
> Cuber QQ always envies those Kejin players, who pay a lot of RMB to get a higher level in the game. So he worked so hard that you are now the game designer of this game. He decided to annoy these Kejin players a little bit, and give them the lesson that RMB does not always work.
> 
> This game follows a traditional Kejin rule of "when you are level i, you have to pay $a_i$ RMB to get to level $i+1$". Cuber QQ now changed it a little bit: "when you are level $i$, you pay $a_i$ RMB, are you get to level $i+1$ with probability $p_i$; otherwise you will turn into level $x_i (x_i\le i)$".
> 
> Cuber QQ still needs to know how much money expected the Kejin players needs to ``ke'' so that they can upgrade from level $l$ to level $r$, because you worry if this is too high, these players might just quit and never return again.
 

## Input
> The first line of the input is an integer t, denoting the number of test cases.
> 
> For each test case, there is two space-separated integers $n (1\le n\le 500 000)$ and $q (1\le q\le 500 000)$ in the first line, meaning the total number of levels and the number of queries.
> 
> Then follows $n$ lines, each containing integers $r_i, s_i, x_i, a_i$$(1\le r_i\le s_i\le 10^9, 1\le x_i\le i, 0\le a_i\le 10^9)$ , space separated. Note that $p_i$ is given in the form of a fraction $\frac{r_i}{s_i}$.
> 
> The next $q$ lines are $q$ queries. Each of these queries are two space-separated integers $l$ and $r$ $(1\le l < r\le n+1)$.
> 
> The sum of $n$ and sum of $q$ from all $t$ test cases both does not exceed $10^6$.

 

## Output
> For each query, output answer in the fraction form modulo $10^9+7$, that is, if the answer is $\frac{P}{Q}$, you should output $P\cdot Q^{−1}$ modulo $10^9+7$, where $Q^{−1}$ denotes the multiplicative inverse of $Q$ modulo $10^9+7$.
 

## Sample Input
```markdown
1
3 2
1 1 1 2
1 2 1 3
1 3 3 4
1 4
3 4
```

## Sample Output
```markdown
22
12
```

## Solution

### 题意：

从 $i$ 级升级到 $i + 1$ 级需要花费 $a_i$ RMB，成功的概率为 $p_i = \frac{r_i}{s_i}$，若失败则降到 $x_i$ 级，然后给出 $q$ 个询问求 $l$ 级升级到 $r$ 级花费的期望。 

### 题解：

**期望DP 逆元**

设 $g(l, r)$ 为 $l$ 升到 $r$ 的期望，这种期望满足减法 $g(l, r) = g(1, r) − g(1, l)$。因为升级只能一级一级升, 所以要从 $1$ 升级到 $r$, 必然要经过 $l$。可以降维，用 $dp[i]$ 表示从 $1$ 升到 $i$ 的期望，则 $g(l, r) = dp[r] − dp[l]$。

从 $dp[i]$ 转移至 $dp[i + 1]$，假设尝试了 $t$ 次才成功，那么也就是前面 $t - 1$ 次都是失败的，所以下一状态的花费为当前状态的花费 + 成功的花费 + 失败的花费 + 失败后再次回到当前状态的花费。于是：

$$dp[i + 1] = dp[i] + 1 \times a[i] + (t - 1) \times a[i] + (t- 1) \times (dp[i] - dp[x_i])$$

又 $\frac{t - 1}{t} = 1 - \frac{r_i}{s_i}$，即 $t = \frac{s_i}{r_i}$

于是状态转移方程为：

$$dp[i + 1] = dp[i] + \frac{s_i}{r_i} \times a[i] + (\frac{s_i}{r_i} - 1) \times (dp[i] - dp[x_i])$$

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 5e5 + 10;
const ll mod = 1e9 + 7;

ll r[maxn], s[maxn], x[maxn], a[maxn];

ll dp[maxn];

ll qmod(ll a, ll b, ll p) {
    ll ans = 1;
    while(b) {
        if(b & 1) ans = (a * ans) % p;
        a = (a * a) % p;
        b >>= 1;
    }
    return ans;
}

int main() {
    int T;
    cin >> T;
    while(T--) {
        int n, q;
        scanf("%d%d", &n, &q);
        for(int i = 1; i <= n; ++i) {
            scanf("%lld%lld%lld%lld", &r[i], &s[i], &x[i], &a[i]);
            ll t = (s[i] * qmod(r[i], mod - 2, mod)) % mod;
            dp[i + 1] = (dp[i] + (t * a[i]) % mod + ((t - 1) * (dp[i] - dp[x[i]])) % mod + mod) % mod;
        }
        for(int i = 0; i < q; ++i) {
            int l, r;
            scanf("%d%d", &l, &r);
            printf("%lld\n", (dp[r] - dp[l] + mod) % mod);
        }
    }
    return 0;
}
```