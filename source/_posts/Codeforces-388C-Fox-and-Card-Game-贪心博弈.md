---
title: Codeforces 388C Fox and Card Game (贪心博弈)
date: 2019-10-20 22:01:21
tags:
- ACM刷题
- 贪心
- 博弈
categories: 竞赛
mathjax: true
---

[Codeforces Round #228 (Div. 1)](https://codeforces.com/contest/388)

题目链接：[C. Fox and Card Game](https://codeforces.com/problemset/problem/388/C)

> Fox Ciel is playing a card game with her friend Fox Jiro. There are n piles of cards on the table. And there is a positive integer on each card.
> 
> The players take turns and Ciel takes the first turn. In Ciel's turn she takes a card from the top of any non-empty pile, and in Jiro's turn he takes a card from the bottom of any non-empty pile. Each player wants to maximize the total sum of the cards he took. The game ends when all piles become empty.
> 
> Suppose Ciel and Jiro play optimally, what is the score of the game?

<!--more-->

## Input

> The first line contain an integer $n (1 ≤ n ≤ 100)$. Each of the next $n$ lines contains a description of the pile: the first integer in the line is $s_i (1 ≤ s_i ≤ 100)$ — the number of cards in the $i$-th pile; then follow $s_i$ positive integers $c_1, c_2, ..., c_k, ..., c_{s_i} (1 ≤ c_k ≤ 1000)$ — the sequence of the numbers on the cards listed from top of the current pile to bottom of the pile.

## Output

> Print two integers: the sum of Ciel's cards and the sum of Jiro's cards if they play optimally.

## Examples

> **input**
> 
> >     2 
> >     1 100
> >     2 1 10
> 
> **output**
> 
> >     101 10
>
> **input**
> 
> >     1 
> >     9 2 8 6 5 9 4 7 1 3
> 
> **output**
> 
> >     30 15
> 
> **input**
> 
> >     3
> >     3 1 3 2
> >     3 5 4 6 
> >     2 8 7
> 
> **output**
> 
> >     18 18
> 
> **input**
> 
> >     3
> >     3 1000 1000 1000
> >     6 1000 1000 1000 1000 1000 1000
> >     5 1000 1000 1000 1000 1000
> 
> **output**
> 
> >     7000 7000


## Note
> In the first example, Ciel will take the cards with number 100 and 1, Jiro will take the card with number 10.
> 
> In the second example, Ciel will take cards with numbers 2, 8, 6, 5, 9 and Jiro will take cards with numbers 4, 7, 1, 3.




## Solution

### 题意

给定 $n$ 叠牌，第 $i$ 叠牌有 $s_i$ 张，第 $k$ 张牌的值为 $c_k$。

Ciel 先手，每次选择一叠牌，拿走最上面的一张牌，Jiro 后手，每次选择一叠牌，拿走最下面的一张牌。

求两者在采取最优策略的情况下各自的分数。

### 题解

贪心博弈。如果一叠牌的数量是偶数，那么两个人各自取一半，如果是奇数，则中间的一叠牌单独取，其余的牌一人一半。

对所有的中间的牌排序后再轮流取。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
vector<int> a;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin >> n;
    int sum1 = 0, sum2 = 0;
    for(int i = 0; i < n; ++i) {
        int k, x;
        cin >> k;
        for(int j = 1; j <= k / 2; ++j) {
            cin >> x;
            sum1 += x;
        }
        if(k & 1) {
            cin >> x;
            a.push_back(x);
        }
        for(int j = 1; j <= k / 2; ++j) {
            cin >> x;
            sum2 += x;
        }
    }
    sort(a.begin(), a.end(), [](int a, int b){return a > b;});
    for(int i = 0; i < a.size(); ++i) {
        if(i & 1) {
            sum2 += a[i];
        } else {
            sum1 += a[i];
        }
    }
    printf("%d %d\n", sum1, sum2);
    return 0;
}
```