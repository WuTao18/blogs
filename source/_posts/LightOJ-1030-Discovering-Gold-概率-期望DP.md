---
title: LightOJ 1030 Discovering Gold (概率/期望DP)
date: 2019-10-13 20:20:14
tags:
- ACM刷题
- 概率DP
- 期望DP
- 概率
- 期望
- 动态规划
- 数学
categories: 竞赛
mathjax: true
---

题目链接：[LightOJ - 1030](http://lightoj.com/volume_showproblem.php?problem=1030)

## Description
> You are in a cave, a long cave! The cave can be represented by a $1 \times N$ grid. Each cell of the cave can contain any amount of gold.
> 
> Initially you are in position $1$. Now each turn you throw a perfect $6$ sided dice. If you get $X$ in the dice after throwing, you add $X$ to your position and collect all the gold from the new position. If your new position is outside the cave, then you keep throwing again until you get a suitable result. When you reach the $N^{th}$ position you stop your journey. Now you are given the information about the cave, you have to find out the **expected** number of gold you can collect using the given procedure.

<!--more-->

## Input
> Input starts with an integer $T (≤ 100)$, denoting the number of test cases.
> 
> Each case contains a blank line and an integer $N (1 ≤ N ≤ 100)$ denoting the dimension of the cave. The next line contains $N$ space separated integers. The $i^{th}$ integer of this line denotes the amount of gold you will get if you come to the $i^{th}$ cell. You may safely assume that all the given integers will be non-negative and no integer will be greater than $1000$.

## Output

> For each case, print the case number and the expected number of gold you will collect. Errors less than $10^{-6}$ will be ignored.

## Sample Input

```markdown
3

1
101

2
10 3

3
3 6 9
```

## Sample Output

```markdown
Case 1: 101.0000000000
Case 2: 13.000
Case 3: 15
```


## Solution

### 题意

有 $N$ 个格子，每个格子有价值为 $val[i]$ 的金子，初始你在第一个格子。

每次抛一个 $6$ 面的骰子，抛到的数为 $X_i$，就往前走 $X_i$ 个格子，如果超过格子 $N$，就重新抛，走到格子 $N$ 就结束。求拿到金子的价值的期望。 

### 思路

可以用 **概率 DP** 或 **期望 DP** 解决。

**概率DP**

求出到达每个格子的概率，然后乘上每个格子的价值再累加起来就行。

设 $dp[i]$ 为到格子 $i$ 的概率，则 $dp[i + j] = dp[i + j] + dp[i] / k\ (1\le j\le k,\ k = min(6, n - i))$。

**期望DP**

设 $dp[i]$ 为格子 $i$ 到 $N$ 的能获得黄金的期望，则 $dp[N] = val[N]$。

状态转移方程为 $dp[i] = \frac{1}{k} \sum_{j=1}^kdp[i+j]\ (k = min(6, n - i))$。

## Code

**概率DP**

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 110;

int v[maxn];
double dp[maxn];

int main() {
    int T;
    scanf("%d", &T);
    int kase = 0;
    while(T--) {
        int n;
        scanf("%d", &n);
        for(int i = 1; i <= n; ++i) {
            scanf("%d", &v[i]);
        }
        memset(dp, 0, sizeof(dp));
        dp[1] = 1.0;
        for(int i = 1; i <= n; ++i) {
            int k = min(6, n - i);
            for(int j = 1; j <= k; ++j) {
                dp[i + j] += dp[i] * 1.0 / k;
            }
        }
        double ans = 0.0;
        for(int i = 1; i <= n; ++i) {
            ans += dp[i] * v[i];
        }
        printf("Case %d: %.7lf\n", ++kase, ans);
    }
    return 0;
}
```

**期望DP**

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 110;

int v[maxn];
double dp[maxn];

int main() {
    int T;
    scanf("%d", &T);
    int kase = 0;
    while(T--) {
        int n;
        scanf("%d", &n);
        for(int i = 1; i <= n; ++i) {
            scanf("%d", &v[i]);
        }
        memset(dp, 0, sizeof(dp));
        dp[n] = v[n];
        for(int i = n - 1; i >= 1; --i) {
            dp[i] = v[i];
            int k = min(6, n - i);
            for(int j = 1; j <= k; ++j) {
                dp[i] += dp[i + j] * 1.0 / k;
            }
        }
        printf("Case %d: %.7lf\n", ++kase, dp[1]);
    }
    return 0;
}
```