---
title: POJ 2398 Toy Storage (二分 叉积)
date: 2019-08-25 21:44:43
tags:
- ACM刷题
- 计算几何
- 二分
- ToLeftTest
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2398](http://poj.org/problem?id=2398)

## Problem Description
> Mom and dad have a problem: their child, Reza, never puts his toys away when he is finished playing with them. They gave Reza a rectangular box to put his toys in. Unfortunately, Reza is rebellious and obeys his parents by simply throwing his toys into the box. All the toys get mixed up, and it is impossible for Reza to find his favorite toys anymore. 
> 
> Reza's parents came up with the following idea. They put cardboard partitions into the box. Even if Reza keeps throwing his toys into the box, at least toys that get thrown into different partitions stay separate. The box looks like this from the top: 
> 
> ![](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/08/25/1566739388887-1566739388889.png)
> 
> We want for each positive integer t, such that there exists a partition with t toys, determine how many partitions have t, toys.
 
<!--more-->

## Input
> The input consists of a number of cases. The first line consists of six integers n, m, x1, y1, x2, y2. The number of cardboards to form the partitions is n (0 < n <= 1000) and the number of toys is given in m (0 < m <= 1000). The coordinates of the upper-left corner and the lower-right corner of the box are (x1, y1) and (x2, y2), respectively. The following n lines each consists of two integers Ui Li, indicating that the ends of the ith cardboard is at the coordinates (Ui, y1) and (Li, y2). You may assume that the cardboards do not intersect with each other. The next m lines each consists of two integers Xi Yi specifying where the ith toy has landed in the box. You may assume that no toy will land on a cardboard. 
> 
> A line consisting of a single 0 terminates the input.
 

## Output
> For each box, first provide a header stating "Box" on a line of its own. After that, there will be one line of output per count (t > 0) of toys in a partition. The value t will be followed by a colon and a space, followed the number of partitions containing t toys. Output will be sorted in ascending order of t for each box.
 

## Sample Input

```markdown
4 10 0 10 100 0
20 20
80 80
60 60
40 40
5 10
15 10
95 10
25 10
65 10
75 10
35 10
45 10
55 10
85 10
5 6 0 10 60 0
4 3
15 30
3 1
6 8
10 10
2 1
2 8
1 5
5 5
40 10
7 9
0
```

## Sample Output

```markdown
Box
2: 5
Box
1: 4
2: 1
```

## Source

[Tehran 2003 Preliminary](http://poj.org/searchproblem?field=source&key=Tehran+2003+Preliminary)

## Solution

### 题意

有 $n$ 条线段将一个矩形分成 $n + 1$ 个区域，现在给定 $m$ 个点的坐标，统计每个区域内点的数量，求每种数量有几个区域。

### 题解

**二分 叉积**

与 [POJ 2318](https://wutao18.github.io/2019/08/25/POJ-2318-TOYS-%E4%BA%8C%E5%88%86-%E5%8F%89%E7%A7%AF/) 类似，不同的是这次给定的线段是乱序的，因此先要排序。输出也不同，改一下即可。

## Code

```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cmath>
#include <cstring>
using namespace std;
typedef long long ll;
typedef double db;
const int maxn = 5e3 + 5;
const db eps = 1e-10;

int dcmp(db x) {
    if(fabs(x) < eps) {
        return 0;
    }
    return x > 0? 1: -1;
}

struct Point {
    double x, y;
    Point(double xx = 0, double yy = 0) : x(xx), y(yy) {}
    void input() {
        scanf("%lf%lf", &x, &y);
    }
    bool operator<(const Point &a) const {
        return (!dcmp(x - a.x))? dcmp(y - a.y) < 0: x < a.x;
    }
    Point operator-(const Point a) {
        return Point(x - a.x, y - a.y);
    }
    db cross(const Point a) {
        return x * a.y - y * a.x;
    }
};
typedef Point Vector;

struct Line {
    Point s, e;
    Line() {}
    Line(Point s, Point e) : s(s), e(e) {}
    bool operator<(const Line &a) const {
        return s < a.s;
    }
    int toLeftTest(Point p) {
        if((e - s).cross(p - s) > 0) return 1;
        else if((e - s).cross(p - s) < 0) return -1;
        return 0;
    }
};

int cmp(Line a, Line b) {
    return a.s < b.s;
}

int n, m;
Line line[maxn];
Point point[maxn];
int ans[maxn];
int cnt[maxn];

void solve() {
    for(int i = 1; i <= m; ++i) {
        int l = 0, r = n + 1;
        int mid = (l + r) >> 1;
        while(l + 1 < r) {
            if(line[mid].toLeftTest(point[i]) > 0) {
                r = mid;
            } else {
                l = mid;
            }
            mid = (l + r) >> 1;
        }
        ans[l]++;
    }
    int max_size = 0;
    for(int i = 0; i <= n; ++i) {
        if(ans[i]) {
            cnt[ans[i]]++;
            max_size = max(max_size, ans[i]);
        }
    }
    printf("Box\n");
    for(int i = 1; i <= max_size; ++i) {
        if(cnt[i]) {
            printf("%d: %d\n", i, cnt[i]);
        }
    }
}

int main() {
    while(scanf("%d", &n) != EOF && n) {
        memset(ans, 0, sizeof(ans));
        memset(cnt, 0, sizeof(cnt));
        scanf("%d", &m);
        db xl, yl, xr, yr;
        scanf("%lf%lf%lf%lf", &xl, &yl, &xr, &yr);
        line[0].s = Point(xl, yr);
        line[0].e = Point(xl, yl);
        line[n + 1].s  = Point(xr, yr);
        line[n + 1].e = Point(xr, yl);
        for(int i = 1; i <= n; ++i) {
            db u, l;
            scanf("%lf%lf", &u, &l);
            line[i].s = Point(l, yr);
            line[i].e = Point(u, yl);
        }
        sort(line + 1, line + 1 + n); // 要对线段排序
        for(int i = 1; i <= m; ++i) {
            scanf("%lf%lf", &point[i].x, &point[i].y);
        }
        solve();
    }
    return 0;
}
```
