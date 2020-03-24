---
title: POJ 3090 Visible Lattice Points (欧拉函数)
date: 2019-09-19 19:51:31
tags:
- ACM刷题
- 数论
- 欧拉函数
- 线性筛
categories: 竞赛
mathjax: true
---

题目链接：[POJ 3090](http://poj.org/problem?id=3090)

## Description
> A lattice point $(x, y)$ in the first quadrant ($x$ and $y$ are integers greater than or equal to $0$), other than the origin, is visible from the origin if the line from $(0, 0)$ to $(x, y)$ does not pass through any other lattice point. For example, the point $(4, 2)$ is not visible since the line from the origin passes through $(2, 1)$. The figure below shows the points $(x, y)$ with $0 \le x, y \le 5$ with lines from the origin to the visible points.
> 
> ![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/09/19/1568893002934-1568893003180.png)
> 
> Write a program which, given a value for the size, $N$, computes the number of visible points $(x, y)$ with $0 \le x, y \le N$.


## Input
> The first line of input contains a single integer $C (1 \le C \le 1000)$ which is the number of datasets that follow.
> 
> Each dataset consists of a single line of input containing a single integer $N (1 \le N \le 1000)$, which is the size.
 
## Output
> For each dataset, there is to be one line of output consisting of: the dataset number starting at $1$, a single space, the size, a single space and the number of visible points for that size.
 

## Sample Input

```markdown
4
2
4
5
231
```

## Sample Output

```markdown
1 2 5
2 4 13
3 5 21
4 231 32549
```

## Source

[Greater New York 2006](http://poj.org/searchproblem?field=source&key=Greater+New+York+2006)

## Solution

### 题意

给定一个大小为 $N * N$ 的矩形，每个格点插着钉子，问从 $(0, 0)$ 点能看到多少钉子。

### 题解

**欧拉函数**

容易发现除了 $(0, 1)$ $(1, 0)$ $(1, 1)$，如果一个钉子 $(x, y)$ 能被看到，那么 $gcd(x, y) = 1,\ x \neq y$。由于能看到的钉子关于过 $(0, 0)$ 和 $(N, N)$ 的直线对称，因此只考虑一半即可。对于每个 $x \in [2, N]$，需要求出多少个 $y$ 满足 $gcd(x, y) = 1, 1 \le y < x$，也就是求 $x$ 的欧拉函数。

因此答案为 $3 + 2 * \sum_{i=2}^N \varphi(i)$，其中 $\varphi(i)$ 为欧拉函数。

## Code

```cpp
#include <iostream>
#include <cstdio>
using namespace std;
typedef long long ll;

const int maxn = 1e4 + 10;

int primes[maxn], cnt;
int phi[maxn];
bool v[maxn];

void get_eulers(int n) {
    for(int i = 2; i <= n; ++i) {
        if(!v[i]) {
            primes[cnt++] = i;
            phi[i] = i - 1;
        }
        for(int j = 0; primes[j] <= n / i; ++j) {
            v[primes[j] * i] = 1;
            if(i % primes[j] == 0) {
                phi[primes[j] * i] = primes[j] * phi[i];
                break;
            }
            phi[primes[j] * i] = phi[i] * (primes[j] - 1);
        }
    }

}

int main() {
    get_eulers(1001);
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    int kase = 0;
    while(T--) {
        int n;
        cin >> n;
        ll sum = 0;
        for(int i = 2; i <= n; ++i) {
            sum += phi[i];
        }
        cout << ++kase << " " << n << " ";
        cout << 3 + 2 * sum << endl;
    }
    return 0;
}
```
