---
title: 2019 ACM-ICPC 上海网络赛 B. Light bulbs (差分)
date: 2019-09-15 22:41:20
tags:
- ACM刷题
- 差分
- STL
categories: 竞赛
mathjax: true
---

题目链接：[Light bulbs](https://nanti.jisuanke.com/t/41399)
比赛链接：[The Preliminary Contest for ICPC Asia Shanghai 2019](https://www.jisuanke.com/contest/3003?view=challenges)

## 题意

给定 $N$ 个灯泡 (编号从 $0$ 到 $N - 1$)，初始都是关闭的。

给定 $M$ 个操作，每个操作包含 $L$ 和 $R$，对 $[L, R]$ 内的所有灯泡改变状态。

求最后有几个灯泡是亮的。

<!--more-->

## 思路

题目挺简单的，翻转奇数次的灯泡是亮的，所以要求每个灯泡翻转的次数。

容易想到可以用差分。

对所有操作的两个端点排序，求差分数组 $d[]$。

然后根据差分数组求前缀和，差分数组相邻两个数 $d[l]$ 和 $d[r]$ 所在的区间 $[l, r)$ 内的每个数都加上 $d[l]$，那么如果 $d[l]$ 为奇数，$ans += (r - l)$。时间复杂度 $O(MlogM)$。

于是就有了下面的代码。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e6 + 10;

int pos[2010];

int main() {
    int T;
    scanf("%d", &T);
    int kase = 0;
    while(T--) {
        int n, m;
        scanf("%d%d", &n, &m);
        vector<int> d(n + 10);
        memset(pos, 0, sizeof(pos));
        unordered_map<int, int> mp;
        int cnt = 0;
        for(int i = 1; i <= m; ++i) {
            int l, r;
            scanf("%d%d", &l, &r);
            ++d[l];
            --d[r + 1];
            if(mp[l] == 0) {
                pos[cnt++] = l;
                mp[l] = 1;
            }
            if(mp[r + 1] == 0) {
                pos[cnt++] = r + 1;
                mp[r + 1] = 1;
            }
        }
        sort(pos, pos + cnt);
        ll ans = 0;
        for(int i = 1; i < cnt; ++i) {
            if(d[pos[i - 1]] & 1) ans += pos[i] - pos[i - 1];
            d[pos[i]] = d[pos[i - 1]] + d[pos[i]];
        }
        if(d[pos[cnt - 1]] & 1) ans += n - pos[cnt - 1];
        printf("Case #%d: %lld\n", ++kase, ans);
    }
    return 0;
}
```

然后就 TLE 了。这题时间和空间卡的很紧。

## AC 代码

用 map 存差分数组，还自动排序了。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e6 + 10;

int main() {
    int T;
    scanf("%d", &T);
    int kase = 0;
    while(T--) {
        int n, m;
        scanf("%d%d", &n, &m);
        map<int, int> d;
        int cnt = 0;
        for(int i = 1; i <= m; ++i) {
            int l, r;
            scanf("%d%d", &l, &r);
            ++d[l];
            --d[r + 1];
        }
        ll ans = 0;
        auto it = d.begin();
        int p = it->first;
        int v = it->second;
        ++it;
        for(; it != d.end(); ++it) {
            if(v & 1) ans += it->first - p;
            it->second = v + it->second;
            p = it->first;
            v = it->second;
        }
        if(v & 1) ans += n - p;
        printf("Case #%d: %lld\n", ++kase, ans);
    }
    return 0;
}
```