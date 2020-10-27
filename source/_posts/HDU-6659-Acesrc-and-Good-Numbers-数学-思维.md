---
title: HDU 6659 Acesrc and Good Numbers (数学 思维)
date: 2019-08-19 21:43:46
tags:
- ACM刷题
- 数学
- 思维
categories: 竞赛
mathjax: true
---

2019 杭电多校 8 1003

题目链接：[HDU 6659](http://acm.hdu.edu.cn/showproblem.php?pid=6659)

比赛链接：[2019 Multi-University Training Contest 8](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+8&source=1&searchmode=source)

## Problem Description
> Acesrc is a famous mathematician at Nanjing University second to none. Playing with interesting numbers is his favorite. Today, he finds a manuscript when cleaning his room, which reads
> 
> ... Let $f(d,n)$ denote the number of occurrences of digit $d$ in decimal representations of integers $1,2,3,⋯,n$. The function has some fantastic properties ...
> 
> ... Obviously, there exist some nonnegative integers $k$, such that $f(d,k)=k$, and I decide to call them $d$-good numbers ...
> 
> ... I have found all d-good numbers not exceeding $10^{1000}$, but the paper is too small to write all these numbers ...
> 
> Acesrc quickly recollects all $d$-good numbers he found, and he tells Redsun a question about $d$-good numbers: what is the maximum $d$-good number no greater than $x$? However, Redsun is not good at mathematics, so he wants you to help him solve this problem.
 
<!--more-->

## Input
> The first line of input consists of a single integer $q (1\le q\le 1500)$, denoting the number of test cases. Each test case is a single line of two integers $d (1\le d\le 9)$ and $x (0\le x\le 10^{18})$.
 

## Output
> For each test case, print the answer as a single integer in one line. Note that $0$ is trivially a $d$-good number for arbitrary $d$.
 

## Sample Input

```markdown
3
1 1
1 199999
3 0
```

## Sample Output

```markdown
1
199990
0
```

## Solution

### 题意

定义 $f(d, n)$ 为十进制下 $1$ 到 $n$ 所有数的数位中数字 $d$ 出现的次数。给定 $x$，找出最大的 $n(n \le x)$ 满足 $f(d, n) = n$。

### 题解

看到了一个神仙做法。

显然如果 $f(d, x) = x$ 时就直接输出。

否则，需要缩小 $x$。令 $f(d, x) = y$，则需要将 $x$ 缩小 $\lceil \frac{|x - y|}{18} \rceil$。即 $x = x - abs(f(d, x) - x) / 18$。原因是 $f(d, x)$ 与 $f(d, x - 1)$ 最多相差 $18$ 个 $d$ $\ (e.g. \ f(9, 10^{18}-1)\ to\ f(9, 10^{18}-2))$。



## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

// 计算 1 到 n 中数字 x 出现的次数
ll f(ll d, ll n) {
    ll cnt = 0, k;
    for (ll i = 1; k = n / i; i *= 10) {
        cnt += (k / 10) * i;
        int cur = k % 10;
        if (cur > d) {
            cnt += i;
        }
        else if (cur == d) {
            cnt += n - k * i + 1;
        }
    }
    return cnt;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int T;
    cin >> T;
    while (T--) {
        ll d, x;
        cin >> d >> x;
        while (true) {
            ll num = f(d, x);
            if (num == x) {
                cout << x << endl;
                break;
            } else {
                x -= max(1LL, abs(num - x) / 18);
            }
        }
    }
    return 0;
}
```

## Reference

[2019 Multi-University Training Contest 8——Acesrc and Good Numbers（数学 想法）](https://blog.csdn.net/jk_chen_acmer/article/details/99595262)