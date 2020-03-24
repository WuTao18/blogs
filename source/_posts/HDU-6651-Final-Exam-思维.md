---
title: HDU 6651 Final Exam (思维)
date: 2019-08-12 20:38:43
tags:
- ACM刷题
- 思维
categories: 竞赛
mathjax: true
---

2019 杭电多校 7 1006

题目链接：[HDU 6651](http://acm.hdu.edu.cn/showproblem.php?pid=6651)

比赛链接：[2019 Multi-University Training Contest 7](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+7&source=1&searchmode=source)


## Problem Description
> Final Exam is coming! Cuber QQ has now one night to prepare for tomorrow's exam.
> 
> The exam will be a exam of problems sharing altogether $m$ points. Cuber QQ doesn't know about the exact distribution. Of course, different problems might have different points; in some extreme cases, some problems might worth $0$ points, or all $m$ points. Points must be integers; a problem cannot have $0.5$ point.
> 
> What he knows, is that, these $n$ problems will be about $n$ totally different topics. For example, one could be testing your understanding of Dynamic Programming, another might be about history of China in 19th century. So he has to divide your night to prepare each of these topics separately. Also, if one problem is worth $x$ points in tomorrow's exam, it takes at least $x+1$ hours to prepare everything you need for examination. If he spends less than $x+1$ hours preparing, he shall fail at this problem.
> 
> Cuber QQ's goal, strangely, is not to take as much points as possible, but to solve at least $k$ problems no matter how the examination paper looks like, to get away from his parents' scoldings. So he wonders how many hours at least he needs to achieve this goal.
 

## Input
> The first line of the input is an integer $t (1\le t\le 20\ 000)$, denoting the number of test cases.
> 
> Each test case are three space-separated integers $n,m,k (0\le m\le 10^9, 1\le k\le n\le 10^9)$. 
 

## Output
> For each test case, output the number of hours Cuber QQ needs.
 

## Sample Input
```markdown
2
1 10 1
10 109 10
```

## Sample Output
```markdown
11
1100
```


## Hint

> Cuber QQ should solve one problem in sample 1, so he at least prepares 11 hours when the problem one is 10 point. 
> 
> Cuber QQ should solve all the ten problems in sample 2, so he at least prepares 110 hours for each problem because there may be one problem is 109 point.


## Solution

### 题意：

一次考试共有 $n$ 道题，总分为 $m$ 分。每道题的分数不一定，可能是 $0$ 分，也可能是 $m$ 分，分数一定是整数。如果一道题分数为 $x$，那么复习这道题的时间为 $x + 1$，现在要保证在考试中做出 $k$ 题，求准备考试的时间最少为多少。

### 题解：

**思维**

如果做不出 $k$ 题，那么也就是复习时间最少的 $n − k + 1$ 道题的难度都小于等于复习的时间。因此想要做出 $k$ 题，只要让复习时间最少的 $n − k + 1$ 道题的复习时间总和 $> m$ 即可。

也就是 $n - k + 1$ 道题的复习时间总和为 $m + 1$，剩下 $k - 1$ 道题的复习时间不是最少的 $k - 1$ 道题即可。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

int main() {
    int T;
    cin >> T;
    while(T--) {
        ll n, m, k;
        scanf("%lld%lld%lld", &n, &m, &k);
        printf("%lld\n", m + 1 + (m / (n - k + 1) + 1) * (k - 1));
    }
    return 0;
}
```