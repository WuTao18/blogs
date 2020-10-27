---
title: HDU 6693 Valentine's Day (概率)
date: 2019-08-22 14:15:48
tags:
- ACM刷题
- 数学
- 概率
- 贪心
categories: 竞赛
mathjax: true
---

2019 杭电多校 10 1003

题目链接：[HDU 6693](http://acm.hdu.edu.cn/showproblem.php?pid=6693)

比赛链接：[2019 Multi-University Training Contest 10](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+10&source=1&searchmode=source)

## Problem Description
> Oipotato loves his girlfriend very much. Since Valentine's Day is coming, he decides to buy some presents for her. 
> 
> There are $n$ presents in the shop, and Oipotato can choose to buy some of them. We know that his girlfriend will possibly feel extremely happy if she receives a present. Therefore, if Oipotato gives $k$ presents to his girlfriend, she has $k$ chances to feel extremely happy. However, Oipotato doesn't want his girlfriend to feel extremely happy too many times for the gifts. 
> 
> Formally, for each present $i$, it has a possibility of $P_i$ to make Oipotato's girlfriend feel extremely happy. Please help Oipotato decide what to buy and maximize the possibility that his girlfriend feels extremely happy for **exactly** one time. 
 
<!--more-->

## Input
> There are multiple test cases. The first line of the input contains an integer $T (1\le T\le 100)$, indicating the number of test cases. For each test case: 
> 
> The first line contains an integer $n (1\le n\le 10 000)$, indicating the number of possible presents. 
> 
> The second line contains $n$ decimals $P_i (0\le Pi\le 1)$ with exactly six digits after the decimal point, indicating the possibility that Oipotato's girlfriend feels extremely happy when receiving present $i$. 
> 
> It is guaranteed that the sum of $n$ in all test cases does not exceed $450000$. 
 

## Output
> For each test case output one line, indicating the answer. Your answer will be considered correct if and only if the absolute error of your answer is less than $10^{−6}$. 
 

## Sample Input

```markdown
2
3
0.100000 0.200000 0.900000
3
0.100000 0.300000 0.800000
```

## Sample Output

```markdown
0.900000000000
0.800000000000
```

## Solution

### 题意

有 $n$ 种礼物，第 $i$ 种礼物能让女朋友开心的概率为 $P_i$，挑一些礼物，问让女朋友开心一次的概率最大为多少。

### 题解

**概率 贪心**

如果有概率大于等于 $0.5$ 的礼物，输出其中最大的。

否则，对概率从大到小。暴力枚举选择前 $k$ 大的礼物的的概率，求最大值即可。

此题有原题。见 [CodeForces 442B](https://codeforces.com/problemset/problem/442/B)

相关证明见官方题解：[Codeforces #253 editorial](https://codeforces.com/blog/entry/12739)

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const double eps = 1e-8;
const int inf = 0x3f3f3f3f;
const int maxn = 100000 + 5;

double p[maxn];

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        int n;
        scanf("%d", &n);
        double ans = 0, maxa = -1;
        int flag = 0;
        for(int i = 0; i < n; ++i) {
            scanf("%lf", &p[i]);
            if(p[i] >= 0.5) {
                flag = 1;
                if(maxa = -1) maxa = p[i];
                else maxa = max(maxa, p[i]);
            }
        }
        if(flag) {
            printf("%.6lf\n", maxa);
        } else {
            sort(p, p + n);
            double tmp1 = p[n - 1];
            double tmp2 = p[n - 1] * (1 - p[n - 2]) + p[n - 2] * (1 - p[n - 1]);
            ans = tmp2;
            int k = 3;
            while(tmp1 < tmp2 && k <= n) {
                ans = tmp2;
                tmp1 = tmp2;
                tmp2 = 0;
                for(int i = n - 1; i >= n - k; --i) {
                    double tmp = 1;
                    for(int j = n - 1; j >= n - k; --j) {
                        if(j == i) {
                            tmp *= p[j];
                        } else {
                            tmp *= 1 - p[j];
                        }
                    }
                    tmp2 += tmp;
                }
                k ++;
            }
            printf("%.6lf\n", max(ans, tmp2));
        }
    }
    return 0;
}
```
