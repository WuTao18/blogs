---
title: HDU 1028 Ignatius and the Princess III (动态规划)
date: 2019-11-04 21:21:24
tags:
- ACM刷题
- 动态规划
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1028](http://acm.hdu.edu.cn/showproblem.php?pid=1028)

## Problem Description

> "Well, it seems the first problem is too easy. I will let you know how foolish you are later." feng5166 says.
> 
> "The second problem is, given an positive integer N, we define an equation like this:
> 
>   N=a[1]+a[2]+a[3]+...+a[m];
> 
>   a[i]>0,1<=m<=N;
> 
> My question is how many different equations you can find for a given N.
> 
> For example, assume N is 4, we can find:
> 
>   4 = 4;
> 
>   4 = 3 + 1;
> 
>   4 = 2 + 2;
> 
>   4 = 2 + 1 + 1;
> 
>   4 = 1 + 1 + 1 + 1;
> 
> so the result is 5 when N is 4. Note that "4 = 3 + 1" and "4 = 1 + 3" is the same in this problem. Now, you do it!"

## Input

> The input contains several test cases. Each test case contains a positive integer N(1<=N<=120) which is mentioned above. The input is terminated by the end of file.

## Output

> For each test case, you have to output a line contains an integer P which indicate the different equations you have found.


## Sample Input

```markdown
4
10
20
```

## Sample Output

```markdown
5
42
627
```

## Solution

### 题意

给定 $n$，求 $n$ 的划分数。

### 思路

最容易想到的就是直接递归，但是复杂度很高，可以用动态规划降低复杂度。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 150;

int dp[maxn][maxn];  // dp[i][j] 表示将i划分成最大数不超过j的划分数

void solve() {
    for(int i = 1; i < maxn; ++i) {
        for(int j = 1; j < maxn; ++j) {
            if(i == 1 || j == 1) {
                dp[i][j] = 1;
            } else if(i < j) {
                dp[i][j] = dp[i][i];
            } else if(i == j) {
                dp[i][j] = dp[i][j - 1] + 1;
            } else {
                // dp[i][j - 1]表示最大数不超过j-1的方案数, dp[i - j][j]表示拿出一个j后最大数不超过j的方案数
                dp[i][j] = dp[i][j - 1] + dp[i - j][j];
            }
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    solve();
    int n;
    while(cin >> n) {
        cout << dp[n][n] << endl;
    }
    return 0;
}
```