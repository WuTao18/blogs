---
title: 'LOJ #103. 子串查找 (Hash)'
date: 2019-10-10 23:09:42
tags:
- ACM刷题
- 哈希/Hash
- 字符串
categories: 竞赛
mathjax: true
---

## 题意

给定两个字符串 $A$ 和 $B$，求 $B$ 在 $A$ 中的出现次数。

<!--more-->

## 思路

这是一道 $KMP$ 的模板题。

不过 $Hash$ 是个好东西，可以用 $Hash$ 代替 $KMP$ 算法。

预处理两个字符串的哈希值，然后将 $A$ 中所有长度为 $len(B)$ 的子串的哈希值与 $B$ 的哈希值比较即可。

时间复杂度 $O(n + m)$，与 $KMP$ 算法一样！

缺点就是常数略大，而且不能用 $KMP$ 的 $next$ 数组。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef unsigned long long ull;
const int maxn = 1e6 + 10;
const int base = 131;

ull h1[maxn], h2;
ull p[maxn];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    string s1, s2;
    cin >> s1 >> s2;
    p[0] = 1;
    for(int i = 0; i < s1.size(); ++i) {
        h1[i + 1] = h1[i] * base + (ull)s1[i];
        p[i + 1] = p[i] * base;
    }
    for(int i = 0; i < s2.size(); ++i) {
        h2 = h2 * base + (ull)s2[i];
    }
    int ans = 0;
    for(int i = s2.size(); i <= s1.size(); ++i) {
        if(h1[i] - h1[i - s2.size()] * p[s2.size()] == h2) {
            ++ans;
        }
    }
    cout << ans << endl;
    return 0;
}
```

## 参考

[LOJ 103子串查找——用hash代替kmp算法](https://www.cnblogs.com/lfri/p/11375376.html)