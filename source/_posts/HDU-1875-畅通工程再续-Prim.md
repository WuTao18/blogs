---
title: HDU 1875 畅通工程再续 (Prim)
date: 2019-10-18 23:04:17
tags:
- ACM刷题
- Prim
- 最小生成树
- 图论
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1875](http://acm.hdu.edu.cn/showproblem.php?pid=1875)

## Problem Description

> 相信大家都听说一个“百岛湖”的地方吧，百岛湖的居民生活在不同的小岛中，当他们想去其他的小岛时都要通过划小船来实现。现在政府决定大力发展百岛湖，发展首先要解决的问题当然是交通问题，政府决定实现百岛湖的全畅通！经过考察小组RPRush对百岛湖的情况充分了解后，决定在符合条件的小岛间建上桥，所谓符合条件，就是2个小岛之间的距离不能小于10米，也不能大于1000米。当然，为了节省资金，只要求实现任意2个小岛之间有路通即可。其中桥的价格为 100元/米。

## Input

> 输入包括多组数据。输入首先包括一个整数T(T <= 200)，代表有T组数据。
> 
> 每组数据首先是一个整数C(C <= 100),代表小岛的个数，接下来是C组坐标，代表每个小岛的坐标，这些坐标都是 0 <= x, y <= 1000的整数。

## Output

> 每组输入数据输出一行，代表建桥的最小花费，结果保留一位小数。如果无法实现工程以达到全部畅通，输出”oh!”.


## Sample Input

```markdown
2
2
10 10
20 20
3
1 1
2 2
1000 1000
```

## Sample Output

```markdown
1414.2
oh!
```

## Author

> 8600

## Source

> [2008浙大研究生复试热身赛（2）——全真模拟](http://acm.hdu.edu.cn/search.php?field=problem&key=2008%D5%E3%B4%F3%D1%D0%BE%BF%C9%FA%B8%B4%CA%D4%C8%C8%C9%ED%C8%FC%A3%A82%A3%A9%A1%AA%A1%AA%C8%AB%D5%E6%C4%A3%C4%E2&source=1&searchmode=source)

## Solution

### 题意

给定 $n$ 个小岛的坐标，要在小岛间建桥，联通所有的岛。建桥的条件是两个小岛间距离为 $[10, 1000]$，建桥的价格是 $100$ 元/米，求最小造价。

### 思路

最小生成树裸题。

只有小岛距离在 $[10, 1000]$ 才建边。

注意 $double$。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef double db;
const int N = 110;
const db inf = 1e18;
typedef pair<db, int> P;
int n, m;

struct Point {
    db x, y;
};

struct Edge {
    int to;
    db w;
    Edge(int to, db w): to(to), w(w) {}
};
Point p[N];
vector<Edge> G[N];
db d[N];
int v[N];

void init() {
    for(int i = 0; i < N; ++i) {
        G[i].clear();
    }
}

void add(int x, int y, db z) {
    G[x].push_back(Edge(y, z));
}

db prim(int s) {
    priority_queue<P, vector<P>, greater<P>> q;
    for(int i = 0; i <= n; ++i) {
        d[i] = inf;
    }
    memset(v, 0, sizeof(v));
    db ans = 0;
    d[s] = 0;
    q.push(P(0, s));
    while(q.size()) {
        P p = q.top(); q.pop();
        int x = p.second;
        if(v[x]) continue;
        v[x] = 1;
        ans += d[x];
        for(int i = 0; i < G[x].size(); ++i) {
            Edge e = G[x][i];
            if (d[e.to] > e.w && !v[e.to]) {
                d[e.to] = e.w;
                q.push(P(d[e.to], e.to));
            }
        }
    }
    return ans;
}

db dis(Point a, Point b) {
    return sqrt(pow(a.x - b.x, 2) + pow(a.y - b.y, 2));
}

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        init();
        scanf("%d", &n);
        for(int i = 1; i <= n; ++i) {
            scanf("%lf%lf", &p[i].x, &p[i].y);
        }
        for(int i = 1; i <= n; ++i) {
            for(int j = i + 1; j <= n; ++j) {
                db tmp = dis(p[i], p[j]);
                if(tmp >= 10 && tmp <= 1000) {
                    add(i, j, tmp);
                    add(j, i, tmp);
                }
            }
        }
        db ans = prim(1) * 100;
        if(ans == 0) printf("oh!\n");
        else printf("%.1lf\n", ans);
    }
    return 0;
}
```
