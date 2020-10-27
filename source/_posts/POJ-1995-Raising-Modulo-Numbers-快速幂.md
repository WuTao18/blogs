---
title: POJ 1995 Raising Modulo Numbers (快速幂)
date: 2019-08-06 19:56:28
tags: 
- 快速幂
- ACM刷题
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[POJ 1995](http://poj.org/problem?id=1995)

## Description

> People are different. Some secretly read magazines full of interesting girls' pictures, others create an A-bomb in their cellar, others like using Windows, and some like difficult mathematical games. Latest marketing research shows, that this market segment was so far underestimated and that there is lack of such games. This kind of game was thus included into the KOKODáKH. The rules follow: 
>
> Each player chooses two numbers Ai and Bi and writes them on a slip of paper. Others cannot see the numbers. In a given moment all players show their numbers to the others. The goal is to determine the sum of all expressions AiBi from all players including oneself and determine the remainder after division by a given number M. The winner is the one who first determines the correct result. According to the players' experience it is possible to increase the difficulty by choosing higher numbers. 
> 
> You should write a program that calculates the result and is able to find out who won the game. 

<!--more-->

## Input

> The input consists of Z assignments. The number of them is given by the single positive integer Z appearing on the first line of input. Then the assignements follow. Each assignement begins with line containing an integer M (1 <= M <= 45000). The sum will be divided by this number. Next line contains number of players H (1 <= H <= 45000). Next exactly H lines follow. On each line, there are exactly two numbers Ai and Bi separated by space. Both numbers cannot be equal zero at the same time.

## Output

> For each assingnement there is the only one line of output. On this line, there is a number, the result of expression 
> 
> $(A_1 ^ {B_1} + A_2 ^ {B_2} + ... + A_H ^ {B_H})\ mod\ M$

## Sample Input

```markdown
3
16
4
2 3
3 4
4 5
5 6
36123
1
2374859 3029382
17
1
3 18132
```

## Sample Output

```markdown
2
13195
13
```

## Solution
### 题意
给出 $Z$ 组数据，每组数据包含 $H$ 个 $A$ 和 $H$ 个 $B$，求 $(A_1 ^ {B_1} + A_2 ^ {B_2} + ... + A_H ^ {B_H})\ mod\ M$。 

### 题解
快速幂模板题

## Code

```cpp
#include <iostream>
#include <cstdio>
using namespace std;

typedef long long ll;

ll qmod(ll a, ll b, ll m) {
    if(!b) return 1 % m;
    ll ans = 1;
    while(b) {
        if(b & 1) ans = (a * ans) % m;
        a = (a * a) % m;
        b >>= 1;
    }
    return ans;
}

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        ll m;
        int h;
        scanf("%lld%d", &m, &h);
        ll ans = 0;
        for(int i = 0; i < h; ++i) {
            ll a, b;
            scanf("%lld%lld", &a, &b);
            ans += qmod(a, b, m);
            ans %= m;
        }
        printf("%lld\n", ans);
    }
    return 0;
}
```