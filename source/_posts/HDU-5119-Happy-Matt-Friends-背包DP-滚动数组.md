---
title: HDU 5119 Happy Matt Friends (背包DP + 滚动数组)
date: 2019-09-13 23:34:57
tags:
- ACM刷题
- 动态规划
- 背包DP
- 滚动数组
categories: 竞赛
mathjax: true
---

题目链接：[HDU 5119](http://acm.hdu.edu.cn/showproblem.php?pid=5119)

## Problem Description
> Matt has N friends. They are playing a game together.
> 
> Each of Matt’s friends has a magic number. In the game, Matt selects some (could be zero) of his friends. If the xor (exclusive-or) sum of the selected friends’magic numbers is no less than M , Matt wins.
> 
> Matt wants to know the number of ways to win.
 
<!--more-->

## Input
> The first line contains only one integer $T$ , which indicates the number of test cases.
> 
> For each test case, the first line contains two integers $N, M (1 \le N \le 40, 0 \le M \le 10^6)$.
> 
> In the second line, there are $N$ integers $k_i (0 ≤ k_i ≤ 10^6)$, indicating the $i$-th friend’s magic number.
 

## Output
> For each test case, output a single line “Case #x: y”, where x is the case number (starting from 1) and y indicates the number of ways where Matt can win.
 

## Sample Input

```markdown
2
3 2
1 2 3
3 3
1 2 3
```

## Sample Output

```markdown
Case #1: 4
Case #2: 2
```

## Hint

> In the ﬁrst sample, Matt can win by selecting:
> 
> friend with number 1 and friend with number 2. The xor sum is 3.
> 
> friend with number 1 and friend with number 3. The xor sum is 2.
> 
> friend with number 2. The xor sum is 2.
> 
> friend with number 3. The xor sum is 3. Hence, the answer is 4.

## Source

[2014ACM/ICPC亚洲区北京站-重现赛（感谢北师和上交）](http://acm.hdu.edu.cn/search.php?field=problem&key=2014ACM%2FICPC%D1%C7%D6%DE%C7%F8%B1%B1%BE%A9%D5%BE-%D6%D8%CF%D6%C8%FC%A3%A8%B8%D0%D0%BB%B1%B1%CA%A6%BA%CD%C9%CF%BD%BB%A3%A9&source=1&searchmode=source)

## Solution

### 题意

给定 $n$ 个数 $k[i]$，从中取出一些数使得异或和大于等于 $m$，求有几种取法。

### 思路

**背包DP 滚动数组**

设 $dp[i][j]$ 表示前 $i$ 个数中异或和为 $j$ 的所有取法。状态转移方程为 $dp[i][j] = dp[i - 1][j] + dp[i - 1][j\ xor\ k[i]]$。

由于当前状态只和前一个状态有关，因此可以用滚动数组优化。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1 << 20;

ll dp[10][maxn];
ll k[50];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    for(int _ = 1; _ <= T; ++_) {
        memset(dp, 0, sizeof(dp));
        dp[0][0] = 1;
        int n, m;
        cin >> n >> m;
        for(int i = 1; i <= n; ++i) {
            cin >> k[i];
        }
        for(int i = 1; i <= n; ++i) {
            for(int j = 0; j < maxn; ++j) {
                dp[i & 1][j] = dp[(i - 1) & 1][j] + dp[(i - 1) & 1][j ^ k[i]];
            }
        }
        ll ans = 0;
        for(int i = m; i < maxn; ++i) {
            ans += dp[n & 1][i];
        }
        cout << "Case #" << _ << ": " << ans << endl;
    }
    return 0;
}
```
