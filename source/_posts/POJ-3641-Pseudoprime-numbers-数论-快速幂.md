---
title: POJ 3641 Pseudoprime numbers (数论+快速幂)
date: 2019-08-06 20:40:18
tags: 
- 快速幂
- 数论
- 素数
- ACM刷题
categories: 竞赛
mathjax: true
---

题目链接：[POJ 3641](http://poj.org/problem?id=3641)

## Description

> Fermat's theorem states that for any prime number p and for any integer a > 1, ap = a (mod p). That is, if we raise a to the pth power and divide by p, the remainder is a. Some (but not very many) non-prime values of p, known as base-a pseudoprimes, have this property for some a. (And some, known as Carmichael Numbers, are base-a pseudoprimes for all a.)
>
> Given 2 < p ≤ 1000000000 and 1 < a < p, determine whether or not p is a base-a pseudoprime.

<!--more-->

## Input

> Input contains several test cases followed by a line containing "0 0". Each test case consists of a line containing p and a.

## Output

> For each test case, output "yes" if p is a base-a pseudoprime; otherwise output "no".

## Sample input

```markdown
3 2
10 3
341 2
341 3
1105 2
1105 3
0 0
```

## Sample output

```markdown
no
no
yes
no
yes
yes
```

## Solution

### 题意

给定 $p$ 和 $a$，判断 $p$ 是否为合数且满足 $a^p\equiv a(mod\ p)$。

### 题解

水题 快速幂 + 素数判断

## Code

```cpp
#include <iostream>
#include <cstdio>
using namespace std;

typedef long long ll;

bool is_prime(ll a) {
    for(ll i = 2; i <= a / i; ++i) {
        if(a % i == 0) return 0;
    }
    return 1;
}

ll qmod(ll a, ll b, ll p) {
    if(!b) return 1 % p;
    ll ans = 1;
    while(b) {
        if(b & 1) ans = (ans * a) % p;
        a = (a * a) % p;
        b >>= 1;
    }
    return ans;
}

int main() {
    ll a, p;
    while(~scanf("%lld%lld", &p, &a) && a + p) {
        if(is_prime(p) == 0 && qmod(a, p, p) == a) {
            printf("yes\n");
        } else {
            printf("no\n");
        }
    }
    return 0;
}
```