---
title: LightOJ 1027 A Dangerous Maze (期望)
date: 2019-10-13 20:19:04
tags:
- ACM刷题
- 概率
- 期望
- 数学
categories: 竞赛
mathjax: true
---

题目链接：[LightOJ - 1027](http://lightoj.com/volume_showproblem.php?problem=1027)

## Description
> You are in a maze; seeing $n$ doors in front of you in beginning. You can choose any door you like. The probability for choosing a door is equal for all doors.
> 
> If you choose the $i^{th}$ door, it can either take you back to the same position where you begun in $x_i$ minutes, or can take you out of the maze after $x_i$ minutes. If you come back to the same position, you can't remember anything. So, every time you come to the beginning position, you have no past experience.
> 
> Now you want to find the expected time to get out of the maze.


## Input
> Input starts with an integer $T (≤ 100)$, denoting the number of test cases.
> 
> Each case contains a blank line and an integer $n (1 ≤ n ≤ 100)$ denoting the number of doors. The next line contains n space separated integers. If the $i^{th}$ integer $(x_i)$ is positive, you can assume that the $i^{th}$ door will take you out of maze after $x_i$ minutes. If it's negative, then the $i^{th}$ door will take you back to the beginning position after $abs(x_i)$ minutes. You can safely assume that $1 ≤ abs(x_i) ≤ 10000$.

## Output

> For each case, print the case number and the expected time to get out of the maze. If it's impossible to get out of the maze, print '$inf$'. Print the result in $p/q$ format. Where $p$ is the numerator of the result and $q$ is the denominator of the result and they are relatively prime. See the samples for details.

## Sample Input

```markdown
3

1
1

2
-10 -3

3
3 -6 -9
```

## Sample Output

```markdown
Case 1: 1/1
Case 2: inf
Case 3: 18/1
```

## Solution

### 题意

你在一个迷宫里，面前有 $n$ 扇门，如果第 $i$ 扇门的 $X_i$ 值为正值，就可以花费 $X_i$ 的时间的走出迷宫，否则花费 $abs(X_i)$ 的时间又回到原点，且不记得之前走过哪些门。每次等概率选择一扇门，问走出迷宫的时间的期望。不能走出去输出 $inf$。

### 思路

设有 $n_1$ 扇门能走出迷宫，$n_2$ 不能走出迷宫，则 $n_1 + n_2 = n$。

设 $n_1$ 扇门的 $X_i$ 值的平均值为 $t_1$，$n_2$ 扇门的 $X_i$ 值的平均值为 $t_2$.

设走出去的期望为 $E$。则 $E = \frac{n_1}{n} \cdot t_1 + \frac{n_2}{n} \cdot (t_2 + E)$。

化简后为 $E = \frac{1}{n_1}\sum_{i=1}^n abs(X_i)$。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 110;

int x[maxn];

int gcd(int a, int b) {
    return a == 0? b: gcd(b % a, a);
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    int kase = 0;
    while(T--) {
        int n;
        cin >> n;
        int sum = 0;
        int cnt = 0;
        for(int i = 1; i <= n; ++i) {
            cin >> x[i];
            if(x[i] > 0) {
                ++cnt;
                sum += x[i];
            } else {
                sum += -x[i];
            }
        }
        int d = gcd(sum, cnt);
        if(cnt == 0) printf("Case %d: inf\n", ++kase);
        else printf("Case %d: %d/%d\n", ++kase, sum / d, cnt / d);
    }
    return 0;
}
```
