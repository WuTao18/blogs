---
title: 'PTA 1067 Sort with Swap(0, i) (贪心)'
date: 2019-11-16 23:40:53
tags:
- ACM刷题
- 贪心
- 思维
categories: 竞赛
mathjax: true
---
# PTA 1067 Sort with Swap(0, i) (贪心)

题目链接：[1067 Sort with Swap(0, i) (25 分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805403651522560)

![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/11/16/1573916930346-1573916930573.png)

## 题意

给定长度为 $n$ 的排列，如果每次只能把某个数和第 $0$ 个数交换，那么要使排列是升序的最少需要交换几次。

## 思路

**贪心**

由于是排列，所以排序后第 $i$ 个位置上的数就是 $i$。所以当 $a[0] \neq 0$ 时，把 $a[0]$ 位置上的元素交换到相应位置。如果 $a[0] = 0$，就找到第一个不在正确位置上的数，把它与第 $0$ 个数交换，那么下一次又是第一种情况了，也就是下一次交换可以换到正确的位置。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 10;

int arr[maxn];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> arr[i];
    }

    int c = 0, cnt = 0;
    while (c < n) {
        if (arr[0] != 0) {
            swap(arr[0], arr[arr[0]]);
            cnt++;
        } else {
            for (; c < n && arr[c] == c; ++c);
            if (c == n) break;
            swap(arr[0], arr[c]);
            cnt++;
        }
    }
    cout << cnt << endl;
    return 0;
}
```