---
title: 2017 ICPC Asia Urumqi A.coins (概率DP + 期望)
date: 2019-08-10 20:28:39
tags:
- ACM刷题
- 概率DP
- 数学
- 动态规划
categories: 竞赛
mathjax: true
---

题目链接：[Coins](https://nanti.jisuanke.com/t/40512)

## Description

> Alice and Bob are playing a simple game. They line up a row of nn identical coins, all with the heads facing down onto the table and the tails upward.
> 
> For exactly mm times they select any kk of the coins and toss them into the air, replacing each of them either heads-up or heads-down with the same possibility. Their purpose is to gain as many coins heads-up as they can.

<!--more-->

## Input

> The input has several test cases and the first line contains the integer $t (1 \le t \le 1000)$ which is the total number of cases.
> 
> For each case, a line contains three space-separated integers $n$, $m (1 \le n, m \le 100)$ and $k (1 \le k \le n)$.

## Output

> For each test case, output the expected number of coins heads-up which you could have at the end under the optimal strategy, as a real number with the precision of $3$ digits.

## Sample input

```markdown
6
2 1 1
2 3 1
5 4 3
6 2 3
6 100 1
6 100 2
```

## Sample output

```markdown
0.500
1.250
3.479
3.000
5.500
5.000
```

## Solution

### 题意

桌上放置着 $n$ 个反面朝上的硬币，有 $m$ 此操作，每次选择任意 $k$ 个硬币抛向空中，每个硬币落到桌子后正面朝上和反面朝上的概率相同，求最终正面朝上的硬币的期望。

### 题解

**概率DP 期望**

期望 = 概率 * 总数

$f(i, j)$ 表示为抛 $i$ 枚硬币 $j$ 枚硬币朝上的概率。则有 $f(i, j) = 0.5 \times f(i - 1, j) + 0.5 \times f(i - 1, j - 1)$，其中 $f(i, 0) = 2 ^ i$ 。

$DP(i, j)$ 表示第 $i$ 次操作后有 $j$ 枚正面朝上的硬币的概率，则反面硬币的个数为 $n - j$。

如果 $n - j >= k$，那么只要在反面朝上的硬币中选择 $k$ 枚抛即可。抛完 $k$ 枚硬币后有 $0 \sim k$ 枚硬币可能会正面朝上，递推方程为 $DP(i + 1, j + l) = \sum_{l = 0}^{k} DP(i, j) * f(k, l)$。

如果 $n - j < k$，那么除了要抛 $n - j$ 枚反面朝上的硬币，还要选择 $k - (n - j)$ 枚正面朝上的硬币，这样最后正面朝上的个数是本来正面就朝上的 $j-(k-(n-j))$ 枚加上抛了之后朝上的 $l\ (0\le l\le k)$ 枚，递推方程为 $DP(i + 1, j - (k - (n - j)) + l) = \sum_{l = 0}^{k} DP(i, j) * f(k, l)$。

## Code

```cpp
#include <bits/stdc++.h>

using namespace std;
const int maxn = 110;
double dp[maxn][maxn];
double f[maxn][maxn];
int n, k, m;

void init() {
    f[0][0] = 1;
    for (int i = 1; i <= 100; ++i) {
        f[i][0] = pow(0.5, i);
        for (int j = 1; j <= 100; ++j) {
            f[i][j] = (f[i - 1][j] + f[i - 1][j - 1]) / 2.0;
        }
    }
}

int main() {
    init();
    int T;
    scanf("%d", &T);
    while (T--) {
        scanf("%d%d%d", &n, &m, &k);
        memset(dp, 0, sizeof(dp));
        dp[0][0] = 1;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j <= n; ++j) {
                for (int l = 0; l <= k; ++l) {
                    if (n - j >= k) {
                        dp[i + 1][j + l] += dp[i][j] * f[k][l];
                    } else {
                        dp[i + 1][j + l - (k - (n - j))] += dp[i][j] * f[k][l];
                    }
                }
            }
        }
        double ans = 0;
        for (int i = 1; i <= n; ++i) {
            ans += 1.0 * i * dp[m][i];
        }
        printf("%.3f\n", ans);
    }
    return 0;
}
```