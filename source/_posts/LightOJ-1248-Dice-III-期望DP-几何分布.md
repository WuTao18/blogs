---
title: LightOJ 1248 Dice (III) (期望DP / 几何分布)
date: 2019-10-21 21:48:43
tags:
- ACM刷题
- 期望DP
- 期望
- 概率
- 数学
categories: 竞赛
mathjax: true
---

题目链接：[LightOJ - 1248](http://lightoj.com/volume_showproblem.php?problem=1248)

## Description
> Given a dice with **n** sides, you have to find the expected number of times you have to throw that dice to see all its faces at least once. Assume that the dice is fair, that means when you throw the dice, the probability of occurring any face is equal.
>
> For example, for a fair two sided coin, the result is 3. Because when you first throw the coin, you will definitely see a new face. If you throw the coin again, the chance of getting the opposite side is 0.5, and the chance of getting the same side is 0.5. So, the result is
> 
> $1 + (1 + 0.5 * (1 + 0.5 * ...))$
> 
> $= 2 + 0.5 + 0.5^2 + 0.5^3 + ...$
> 
> $= 2 + 1 = 3$

<!--more-->

## Input
> Input starts with an integer $T (≤ 100)$, denoting the number of test cases.
> 
> Each case starts with a line containing an integer $n (1 ≤ n ≤ 10^5)$.

## Output

> For each case, print the case number and the expected number of times you have to throw the dice to see all its faces at least once. Errors less than $10^{-6}$ will be ignored.

## Sample Input

```markdown
5
1
2
3
6
100
```

## Sample Output

```markdown
Case 1: 1
Case 2: 3
Case 3: 5.5
Case 4: 14.7
Case 5: 518.7377517640
```


## Solution

### 题意

给定一个 $n$ 面的骰子，每个面出现的概率相同，现在要所有的面都至少出现一次，求投掷次数的期望。

### 思路

**期望DP**

期望DP一般是倒推的。

设 $dp[i]$ 为已经出现了 $i$ 个面，还需要投掷次数的期望值。

那么每次投掷只有两种情况：出现已经出现过的面、出现未出现的面。前者概率为 $\frac{i}{n}$，后者概率为 $\frac{n - i}{n}$。

则状态转移方程为 $dp[i] = (dp[i] + 1) * \frac{i}{n} + (dp[i + 1] + 1) * \frac{n - i}{n}$

化简得 $dp[i] = dp[i + 1] + \frac{n}{n - i}$

$dp[n] = 0$，倒推即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 10;

double dp[maxn];

int main() {
    int T;
    scanf("%d", &T);
    int kase = 0;
    while(T--) {
        memset(dp, 0, sizeof(dp));
        int n;
        scanf("%d", &n);
        dp[n] = 0;
        for(int i = n - 1; i >= 0; --i) {
            dp[i] = dp[i + 1] + 1.0 * n / (n - i);
        }
        printf("Case %d: %.10lf\n", ++kase, dp[0]);
    }
    return 0;
}
```

其实是满足[几何分布](https://en.wikipedia.org/wiki/Geometric_distribution)的。

第一个出现的面可以是 $1, 2, 3, ..., n$，有 $n$ 个。

如果第一个出现的面是 $1$，那么第二个出现的面可以是 $2, 3, ..., n$，有 $n - 1$ 个。

...

第一个面第一次出现的概率为 $p_1 = \frac{n}{n}$

第二个面第一次出现的概率为 $p_2 = \frac{n - 1}{n}$

第三个面第一次出现的概率为 $p_3 = \frac{n - 2}{n}$

...

第 $i$ 个面第一次出现的概率为 $p_i = \frac{n - i + 1}{n}$

几何分布的期望 $E(X) = \frac{1}{p}$

所以所有面至少出现一次的期望为 $\sum_{i=1}^n \frac{1}{p_i} = \sum_{i=1}^n \frac{n}{n - i + 1}$。

注：几何分布指在 $n$ 次伯努利试验中，试验 $k$ 次才得到第一次成功的机率。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int T;
    scanf("%d", &T);
    int kase = 0;
    while(T--) {
        int n;
        scanf("%d", &n);
        double ans = 0.0;
        for(int i = 1; i <= n; ++i) {
            ans += n * 1.0 / i;
        }
        printf("Case %d: %.10lf\n", ++kase, ans);
    }
    return 0;
}
```

## Reference

> [LightOJ - 1248 Dice (III) （概率期望+几何分布）
](https://blog.csdn.net/lidengdengter/article/details/82527426)