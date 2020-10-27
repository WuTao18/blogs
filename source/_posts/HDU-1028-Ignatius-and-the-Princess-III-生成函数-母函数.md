---
title: HDU 1028 Ignatius and the Princess III (生成函数/母函数)
date: 2019-11-10 15:49:55
tags:
- ACM刷题
- 生成函数/母函数
- 组合数学
- 数学
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1028](http://acm.hdu.edu.cn/showproblem.php?pid=1028)

<!--more-->

## Problem Description

> "Well, it seems the first problem is too easy. I will let you know how foolish you are later." feng5166 says.
> 
> "The second problem is, given an positive integer N, we define an equation like this:
> 
>   N=a[1]+a[2]+a[3]+...+a[m];
> 
>   a[i]>0,1<=m<=N;
> 
> My question is how many different equations you can find for a given N.
> 
> For example, assume N is 4, we can find:
> 
>   4 = 4;
> 
>   4 = 3 + 1;
> 
>   4 = 2 + 2;
> 
>   4 = 2 + 1 + 1;
> 
>   4 = 1 + 1 + 1 + 1;
> 
> so the result is 5 when N is 4. Note that "4 = 3 + 1" and "4 = 1 + 3" is the same in this problem. Now, you do it!"

## Input

> The input contains several test cases. Each test case contains a positive integer N(1<=N<=120) which is mentioned above. The input is terminated by the end of file.

## Output

> For each test case, you have to output a line contains an integer P which indicate the different equations you have found.


## Sample Input

```markdown
4
10
20
```

## Sample Output

```markdown
5
42
627
```

## Solution

### 题意

给定 $n$，求 $n$ 的划分数。

### 思路

普通母函数。母函数 $G(x) = (1+x+x^2+...)(1+x^2+x^4+...)(1+x^3+x^6+...)...$。

$(1+x+x^2+...)=(x^{0\times1}+x^{1\times1}+x^{2\times1}+...)$ 代表不用数字 $1$，用一次数字 $1$，用两次数字 $1$……

动态规划的版本见[这里](https://wutao18.github.io/2019/11/04/HDU-1028-Ignatius-and-the-Princess-III-%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92/)。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 200;

int c1[maxn], c2[maxn];

void init() {
    for(int i = 0; i < maxn; ++i) {
        c1[i] = 1;
        c2[i] = 0;
    }
    for(int i = 2; i < maxn; ++i) {
        for(int j = 0; j < maxn; ++j) {
            for(int k = 0; k + j < maxn; k += i) {
                c2[k + j] += c1[j];
            }
        }
        for(int j = 0; j < maxn; ++j) {
            c1[j] = c2[j];
            c2[j] = 0;
        }
    }
}

int main() {
    init();
    int n;
    while(~scanf("%d", &n)) {
        printf("%d\n", c1[n]);
    }
    return 0;
}
```