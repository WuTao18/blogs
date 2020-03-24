---
title: POJ 1860 Currency Exchange (Bellman-Ford)
date: 2019-11-09 23:39:55
tags:
- ACM刷题
- Bellman-Ford
- 最短路
- 图论
categories: 竞赛
mathjax: true
---

题目链接：[POJ 1860](http://poj.org/problem?id=1860)

## Description

> Several currency exchange points are working in our city. Let us suppose that each point specializes in two particular currencies and performs exchange operations only with these currencies. There can be several points specializing in the same pair of currencies. Each point has its own exchange rates, exchange rate of A to B is the quantity of B you get for 1A. Also each exchange point has some commission, the sum you have to pay for your exchange operation. Commission is always collected in source currency.
> 
> For example, if you want to exchange 100 US Dollars into Russian Rubles at the exchange point, where the exchange rate is 29.75, and the commission is 0.39 you will get (100 - 0.39) * 29.75 = 2963.3975RUR.
> 
> You surely know that there are N different currencies you can deal with in our city. Let us assign unique integer number from 1 to N to each currency. Then each exchange point can be described with 6 numbers: integer A and B - numbers of currencies it exchanges, and real RAB, CAB, RBA and CBA - exchange rates and commissions when exchanging A to B and B to A respectively.
> 
> Nick has some money in currency S and wonders if he can somehow, after some exchange operations, increase his capital. Of course, he wants to have his money in currency S in the end. Help him to answer this difficult question. Nick must always have non-negative sum of money while making his operations.


## Input

> The first line of the input contains four numbers: N - the number of currencies, M - the number of exchange points, S - the number of currency Nick has and V - the quantity of currency units he has. The following M lines contain 6 numbers each - the description of the corresponding exchange point - in specified above order. Numbers are separated by one or more spaces. 1<=S<=N<=100, 1<=M<=100, V is real number, 0<=V<=103.
> 
> For each point exchange rates and commissions are real, given with at most two digits after the decimal point, 10-2<=rate<=102, 0<=commission<=102.
> 
> Let us call some sequence of the exchange operations simple if no exchange point is used more than once in this sequence. You may assume that ratio of the numeric values of the sums at the end and at the beginning of any simple sequence of the exchange operations will be less than 104.
 
## Output

> If Nick can increase his wealth, output YES, in other case output NO to the output file.


## Sample Input

```markdown
3 2 1 20.0
1 2 1.00 1.00 1.00 1.00
2 3 1.10 1.00 1.10 1.00
```

## Sample Output

```markdown
YES
```

## Source

[Northeastern Europe 2001, Northern Subregion](http://poj.org/searchproblem?field=source&key=Northeastern+Europe+2001)

## Solution

### 题意

有 $n$ 种货币，给出一些两种货币之间的汇率及税价。

求原来持有的货币能否通过一些兑换过程使得价值增加。

### 思路

把货币看成结点，兑换的过程看成有向边，那么其实问题就是判断图中是否存在正环。

使用 $Bellman-Ford$ 算法，与判断负环的方法类似，改变一下松弛的条件即可。注意初始化也需要修改。

## Code

```cpp
#include <cstdio>
#include <iostream>
#include <cmath>
#include <string>
#include <cstring>
#include <vector>
using namespace std;
const int maxn = 1e3;
const double eps = 1e-8;

int n, m, s;
double v;
int tot;
double dis[maxn];

struct Edge {
    int from, to;
    double r, c;
    Edge(int f = 0, int t = 0, double r = 0, double c = 0): from(f), to(t), r(r), c(c) {}
} edges[maxn];

void add(int f, int t, double r, double c) {
    edges[tot++] = Edge(f, t, r, c);
}

bool Bellman_Ford() {
    memset(dis, 0, sizeof(dis));
    dis[s] = v;
    for(int i = 1; i <= n - 1; ++i) {
        bool flag = false;
        for(int j = 0; j < tot; ++j) {
            int f = edges[j].from, t = edges[j].to;
            double r = edges[j].r, c = edges[j].c;
            double tmp = (dis[f] - c) * r;
            if(dis[t] < tmp) {
                dis[t] = tmp;
                flag = true;
            }
        }
        if(!flag) {
            break;
        }
    }
    for(int i = 0; i < tot; ++i) {
        if(dis[edges[i].to] < (dis[edges[i].from] - edges[i].c) * edges[i].r) {
            return true;
        }
    }
    return false;
}

int main() {
    while(~scanf("%d%d%d%lf", &n, &m, &s, &v)) {
        tot = 0;
        int f, t;
        double r1, c1, r2, c2;
        for(int i = 0; i < m; ++i) {
            scanf("%d%d%lf%lf%lf%lf", &f, &t, &r1, &c1, &r2, &c2);
            add(f, t, r1, c1);
            add(t, f, r2, c2);
        }
        if(Bellman_Ford()) {
            printf("YES\n");
        } else {
            printf("NO\n");
        }
    }
    return 0;
}
```
