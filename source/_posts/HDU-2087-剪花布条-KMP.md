---
title: HDU 2087 剪花布条 (KMP)
date: 2019-11-01 19:33:09
tags:
- ACM刷题
- KMP
- 字符串
categories: 竞赛
mathjax: true
---

题目链接：[HDU 2087](http://acm.hdu.edu.cn/showproblem.php?pid=2087)

## Problem Description
> 一块花布条，里面有些图案，另有一块直接可用的小饰条，里面也有一些图案。对于给定的花布条和小饰条，计算一下能从花布条中尽可能剪出几块小饰条来呢？
 
<!--more-->

## Input
> 输入中含有一些数据，分别是成对出现的花布条和小饰条，其布条都是用可见ASCII字符表示的，可见的ASCII字符有多少个，布条的花纹也有多少种花样。花纹条和小饰条不会超过1000个字符长。如果遇见#字符，则不再进行工作。
 

## Output
> 输入中含有一些数据，分别是成对出现的花布条和小饰条，其布条都是用可见ASCII字符表示的，可见的ASCII字符有多少个，布条的花纹也有多少种花样。花纹条和小饰条不会超过1000个字符长。如果遇见#字符，则不再进行工作。

## Sample Input

```markdown
abcde a3
aaaaaa  aa
#
```

## Sample Output

```markdown
0
3
```

## Source

[冬练三九之二](http://acm.hdu.edu.cn/search.php?field=problem&key=%B6%AC%C1%B7%C8%FD%BE%C5%D6%AE%B6%FE&source=1&searchmode=source)

## Solution

### 题意

如题。

### 思路

KMP 裸题，每次匹配成功后模式串指针指向模式串首就行了。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int pi[1010];

void prefix_function(string s) {
    memset(pi, 0, sizeof(pi));
    for(int i = 1; i < s.size(); ++i) {
        int j = pi[i - 1];
        while(j > 0 && s[i] != s[j]) j = pi[j - 1];
        if(s[i] == s[j]) ++j;
        pi[i] = j;
    }
}

int kmp(string t, string p) {
    prefix_function(p);
    int ans = 0;
    int i = 0, j = 0;
    while(i < t.size()) {
        if(t[i] == p[j]) {
            ++i;
            ++j;
        } else {
            if(j == 0) ++i;
            else j = pi[j - 1];
        }
        if(j == p.size()) {
            ++ans;
            j = 0;
        }
    }
    return ans;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    string t, p;
    while(cin >> t) {
        if(t[0] == '#') break;
        cin >> p;
        cout << kmp(t, p) << endl;
    }
    return 0;
}
```