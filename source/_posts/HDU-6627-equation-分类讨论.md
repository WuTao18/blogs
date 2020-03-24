---
title: HDU 6627 equation (分类讨论)
date: 2019-08-15 11:28:20
tags:
- ACM刷题
- 数学
- 分类讨论
categories: 竞赛
mathjax: true
---

2019 杭电多校 5 1004

题目链接：[HDU 6627](http://acm.hdu.edu.cn/showproblem.php?pid=6627)

比赛链接：[2019 Multi-University Training Contest 5](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+5&source=1&searchmode=source)


## Problem Description
> You are given two integers $N,C$ and two integer sequences $a$ and $b$ of length $N$. The sequences are indexed from $1$ to $N$. 
> 
> Please solve the following equation for $x$: 
> 
> $∑_{i=1}^N|a_i\cdot x + b_i| = C$, where $|v|$ means the absolute value of $v$.
 

## Input
> The first line contains an integer $T$ indicating there are $T$ tests. Each test consists of $N+1$ lines. The first line contains two integers $N,C$. The $i$-th line of following $N$ lines consists of two integers $a_i,b_i$. 
> 
> $*\ 1\le T\le 50$ 
> 
> $*\ 1\le N\le 10^5$ 
> 
> $*\ 1\le a_i\le 1000$ 
> 
> $*\ −1000\le b_i\le 1000$ 
> 
> $*\ 1\le C\le 10^9$ 
> 
> $*$ only $5$ tests with $N$ larger than $1000$
 

## Output
> For each test, output one line. 
> 
> If there are an infinite number of solutions, this line consists only one integer $−1$. 
> 
> Otherwise, this line first comes with an integer $m$ indicating the number of solutions, then you must print $m$ fractions from the smallest to the largest indicating all possible answers. (It can be proved that all solutions can be written as fractions). The fraction should be in the form of "a/b" where a must be an integer, b must be a positive integer, and $gcd(abs(a),b)=1$. If the answer is $0$, you should output "$0/1$".

## Sample Input
```markdown
4
2 3
1 2
1 -1
3 3
2 1
2 2
2 3
2 1
3 5
4 -1
3 2
1 -1
1 -2
1 -3
```

## Sample Output
```markdown
-1
2 -3/2 -1/2
0
1 2/1
```

## Solution

### 题意：

给定一个整数 $C$ 和 $N$ 组 $a_i,b_i$，求 $∑_{i=1}^N|a_i\cdot x + b_i| = C$ 的所有解，如果有无穷多个解就输出 -1.

### 思路

**分类讨论**

分类讨论去绝对值。根据 $b_i / a_i$ 排序，可得各段区间：$[-b_0/a_0, ∞),\ [-b_1/a_1, -b_0/a_0),\ [-b_2/a_2, -b_1/a_1),\ ...\ ,[-b_n/a_n, -b_{n-1}/a_{n-1}),\ [∞, -b_n/a_n)$ 设 $suma = \sum_{i=1}^Na_i, sumb = \sum_{i=1}^Nb_i$，依次让 $a_ix+b_i$ 变成 $-a_ix-b_i$，也就是 $suma - 2a_i, sumb-2b_i$，求出 $x_i = \frac{c - sumb}{suma}$ 并判断是否在区间内。无穷解的情况：$suma = 0, sumb = c$。 

感谢杭电没有卡 $double$。

```cpp
#include <cstdio>
#include <iostream>
#include <cmath>
#include <vector>
#include <algorithm>
using namespace std;

const int maxn = 1e5 + 10;

typedef long long ll;

struct Coefficient {
    ll a, b;
} co[maxn];

int cmp(Coefficient c1, Coefficient c2) {
    return c1.b * 1.0 / c1.a < c2.b * 1.0 / c2.a;
}

vector<Coefficient> ans;

ll gcd(ll a, ll b) {
    return b == 0? a: gcd(b, a % b);
}

int main() {
    int T;
    cin >> T;
    while(T--) {
        ans.clear();
        int n;
        ll c;
        scanf("%d%lld", &n, &c);
        ll suma = 0, sumb = 0;
        for(int i = 1; i <= n; ++i) {
            scanf("%lld%lld", &co[i].a, &co[i].b);
            suma += co[i].a;
            sumb += co[i].b;
        }
        sort(co + 1, co + n + 1, cmp);
        Coefficient t;
        t.a = suma, t.b = sumb;
        if((c - sumb) * 1.0 / suma >= -co[1].b * 1.0 / co[1].a) {
            t.b = c - t.b;
            ans.push_back(t);
        }

        int flag = 1;
        for(int i = 1; i <= n; ++i) {
            suma -= co[i].a * 2;
            sumb -= co[i].b * 2;
            t.a = suma;
            t.b = sumb;
            if(!suma) {
                if(sumb == c) {
                    flag = 0;
                    break;
                }
            }
            if(i < n) {
                if((c - sumb) * 1.0 / suma >= -co[i + 1].b * 1.0 / co[i + 1].a && (c - sumb) * 1.0 / suma < -co[i].b * 1.0 / co[i].a) {
                    t.b = c - t.b;
                    ans.push_back(t);
                }
            } else {
                if((c - sumb) * 1.0 / suma < -co[i].b * 1.0 / co[i].a) {
                    t.b = c - t.b;
                    ans.push_back(t);
                }
            }
        }

        if(!flag) printf("-1\n");
        else {
            sort(ans.begin(), ans.end(), cmp);
            printf("%d", ans.size());
            for(int i = 0; i < ans.size(); ++i) {
                if(ans[i].b * 1.0 / ans[i].a < 0) printf(" -");
                else printf(" ");
                ll g = gcd(abs(ans[i].b), abs(ans[i].a));
                if(ans[i].b == 0) printf("0/1");
                else printf("%lld/%lld", abs(ans[i].b) / g, abs(ans[i].a) / g);
            }
            printf("\n");
        }
        
    }
    return 0;
}
```