---
title: POJ 3468 A Simple Problem with Integers (分块)
date: 2019-08-12 22:20:20
tags:
- ACM刷题
- 模板题
- 分块
categories: 竞赛
mathjax: true
---

题目链接：[POJ 3468](http://poj.org/problem?id=3468)

## Description

> You have $N$ integers, $A_1, A_2, ... , A_N$. You need to deal with two kinds of operations. One type of operation is to add some given number to each number in a given interval. The other is to ask for the sum of numbers in a given interval.

<!--more-->

## Input

> The first line contains two numbers $N$ and $Q$. $1 ≤ N,Q ≤ 100000$.
> 
> The second line contains $N$ numbers, the initial values of $A_1, A_2, ... , A_N$. $-1000000000 \le A_i \le 1000000000$.
> 
> Each of the next $Q$ lines represents an operation.
> 
> "$C\ a\ b\ c$" means adding c to each of $A_a, A_{a+1}, \ ...\ , A_b$. $-10000 \le c \le 10000$.
> 
> "$Q\ a\ b$" means querying the sum of $A_a, A_{a+1}, \ ...\ , A_b$.

## Output
> You need to answer all $Q$ commands in order. One answer in a line.

## Sample Input

```markdown
10 5
1 2 3 4 5 6 7 8 9 10
Q 4 4
Q 1 10
Q 2 4
C 3 6 3
Q 2 4
```

## Sample Output

```markdown
4
55
9
15
```

## Hint
> The sums may exceed the range of 32-bit integers.


## Solution

### 题意

给定 $n$ 个数和 $q$ 个询问，询问包含两种：$C\ a\ b\ c$ 代表区间 $[a, b]$ 的每个数加上 $c$，$Q\ a\ b$ 输出区间 $[a, b]$ 的和。

### 题解

**分块**

区间更新模板题，本题可以使用树状数组、线段树和分块解决，这里使用的是分块。

```cpp
#include <cstdio>
#include <iostream>
#include <cmath>
using namespace std;

typedef long long ll;

const int maxn = 1e5 + 10;

ll a[maxn], sum[maxn], add[maxn]; // add[] 是增量标记
int L[maxn], R[maxn]; // 存放每个块的左右边界
int block[maxn]; // 存放下标为 i 的元素的块号
int n, q;
int block_size; // 块的大小

// 分块 + 预处理
void init() {
    block_size = sqrt(n);
    for(int i = 1; i <= block_size; ++i) {
        L[i] = (i - 1) * block_size + 1;
        R[i] = i * block_size;
    }
    // 处理最后一块
    if(R[block_size] < n) {
        ++block_size;
        L[block_size] = R[block_size - 1] + 1;
        R[block_size] = n;
    }
    // 预处理每个块的区间和
    for(int i = 1; i <= block_size; ++i) {
        for(int j = L[i]; j <= R[i]; ++j) {
            block[j] = i;
            sum[i] += a[j];
        }
    }
}

// 将区间 [l, r] 内的所有元素加 c
void change(int l, int r, ll c) {
    int p = block[l], q = block[r]; // 取出左右区间所在的块号
    if(p == q) {
        // 在同一块直接块内暴力
        for(int i = l; i <= r; ++i) {
            a[i] += c;
        }
        sum[p] += c * (r - l + 1);
    } else {
        // 不在同一块，块内暴力，块间整块处理
        for(int i = p + 1; i <= q - 1; ++i) {
            add[i] += c;
        }
        // 块内暴力
        for(int i = l; i <= R[p]; ++i) {
            a[i] += c;
        }
        sum[p] += c * (R[p] - l + 1);
        for(int i = L[q]; i <= r; ++i) {
            a[i] += c;
        }
        sum[q] += c * (r - L[q] + 1);
    }
}

ll query(int l, int r) {
    int p = block[l], q = block[r]; // 取出左右区间所在的块号
    ll ans = 0;
    if(p == q) {
        for(int i = l; i <= r; ++i) {
            ans += a[i];
        }
        ans += add[p] * (r - l + 1);
    } else {
        // 块间暴力
        for(int i = p + 1; i <= q - 1; ++i) {
            ans += sum[i] + add[i] * (R[i] - L[i] + 1); // 注意不是乘以 block_size
        }
        // 块内暴力
        for(int i = l; i <= R[p]; ++i) {
            ans += a[i];
        }
        ans += add[p] * (R[p] - l + 1);
        for(int i = L[q]; i <= r; ++i) {
            ans += a[i];
        }
        ans += add[q] * (r - L[q] + 1);
    }
    return ans;
}

int main() {
    scanf("%d%d", &n, &q);
    for(int i = 1; i <= n; ++i) {
        scanf("%lld", &a[i]);
    }
    init();
    for(int i = 0; i < q; ++i) {
        char op;
        getchar(); scanf("%c", &op);
        int l, r;
        scanf("%d%d", &l, &r);
        if(op == 'C') {
            ll c;
            scanf("%lld", &c);
            change(l, r, c);
        } else {
            printf("%lld\n", query(l, r));
        }
    }
    return 0;
}
```

## Reference

> [《算法竞赛进阶指南》](https://book.douban.com/subject/30136932/) 李煜东 著