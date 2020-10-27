---
title: HDU 2266 How Many Equations Can You Find (DFS)
date: 2019-08-06 20:54:29
tags: 
- DFS
- ACM刷题
categories: 竞赛
mathjax: true
---

题目链接：[HDU 2266](http://acm.hdu.edu.cn/showproblem.php?pid=2266)

## Description

> Now give you an string which only contains 0, 1 ,2 ,3 ,4 ,5 ,6 ,7 ,8 ,9.You are asked to add the sign ‘+’ or ’-’ between the characters. Just like give you a string “12345”, you can work out a string “123+4-5”. Now give you an integer N, please tell me how many ways can you find to make the result of the string equal to N .You can only choose at most one sign between two adjacent characters.

<!--more-->

## Input

> Each case contains a string s and a number N . You may be sure the length of the string will not exceed 12 and the absolute value of N will not exceed 999999999999.

## Output

> The output contains one line for each data set : the number of ways you can find to make the equation.

## Sample Input

```markdown
123456789 3
21 1
```

## Sample Output

```markdown
18
1
```

## Solution

### 题意

给定一串只包含数字的字符串，和一个数 n，在串中任意两个相邻的字符之间加上 + 或 - 构成表达式，问表达式的值有多少种情况等于 n。

### 题解

DFS 水题

每个位置可以选择加入符号或者不加，用 DFS 搜索即可，计算最终表达式是否等于 n，注意开头不能放减号。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
char str[20];
ll n;
ll len;
ll cnt;

// l为插入符号的位置 sum为当前表达式的值
void dfs(ll l, ll sum) {
    if(l == len && sum == n) {
        // 如果没有位置可以放入符号了并且表达式值等于 n 就累加 cnt
        cnt++;
        return;
    }

    ll ans = 0;
    // 搜索剩余表达式
    for(ll i = l; i < len; ++i) {
        ans = ans * 10 + str[i] - '0';  // 从 l 到 i 置为数字
        dfs(i + 1, sum + ans);   // i + 1 位置置为加号
        if(l) dfs(i + 1, sum - ans);  // i + 1 位置置为减号
    }
}

int main() {
    while(~scanf("%s %lld", str, &n)) {  // 多组输入
        len = strlen(str);  // 字符串长度
        cnt = 0;  // 最终答案
        dfs(0, 0);
        printf("%lld\n", cnt);
    }
    return 0;
}

```