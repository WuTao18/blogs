---
title: POJ 2318 TOYS (二分 叉积)
date: 2019-08-25 21:31:33
tags:
- ACM刷题
- 计算几何
- 二分
- ToLeftTest
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2318](http://poj.org/problem?id=2318)

## Problem Description
> Calculate the number of toys that land in each bin of a partitioned toy box. 
> 
> Mom and dad have a problem - their child John never puts his toys away when he is finished playing with them. They gave John a rectangular box to put his toys in, but John is rebellious and obeys his parents by simply throwing his toys into the box. All the toys get mixed up, and it is impossible for John to find his favorite toys. 
> 
> 
> John's parents came up with the following idea. They put cardboard partitions into the box. Even if John keeps throwing his toys into the box, at least toys that get thrown into different bins stay separated. The following diagram shows a top view of an example toy box. 
> 
> ![](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/08/25/1566739388887-1566739388889.png)
> 
> For this problem, you are asked to determine how many toys fall into each partition as John throws them into the toy box.
 
<!--more-->

## Input
> The input file contains one or more problems. The first line of a problem consists of six integers, n m x1 y1 x2 y2. The number of cardboard partitions is n (0 < n <= 5000) and the number of toys is m (0 < m <= 5000). The coordinates of the upper-left corner and the lower-right corner of the box are (x1,y1) and (x2,y2), respectively. The following n lines contain two integers per line, Ui Li, indicating that the ends of the i-th cardboard partition is at the coordinates (Ui,y1) and (Li,y2). You may assume that the cardboard partitions do not intersect each other and that they are specified in sorted order from left to right. The next m lines contain two integers per line, Xj Yj specifying where the j-th toy has landed in the box. The order of the toy locations is random. You may assume that no toy will land exactly on a cardboard partition or outside the boundary of the box. The input is terminated by a line consisting of a single 0.
 

## Output
> The output for each problem will be one line for each separate bin in the toy box. For each bin, print its bin number, followed by a colon and one space, followed by the number of toys thrown into that bin. Bins are numbered from 0 (the leftmost bin) to n (the rightmost bin). Separate the output of different problems by a single blank line.
 

## Sample Input

```markdown
5 6 0 10 60 0
3 1
4 3
6 8
10 10
15 30
1 5
2 1
2 8
5 5
40 10
7 9
4 10 0 10 100 0
20 20
40 40
60 60
80 80
 5 10
15 10
25 10
35 10
45 10
55 10
65 10
75 10
85 10
95 10
0
```

## Sample Output

```markdown
0: 2
1: 1
2: 1
3: 1
4: 0
5: 1

0: 2
1: 2
2: 2
3: 2
4: 2

```

## Hint

As the example illustrates, toys that fall on the boundary of the box are "in" the box.

## Source

[Rocky Mountain 2003](http://poj.org/searchproblem?field=source&key=Rocky+Mountain+2003)

## Solution

### 题意

有 $n$ 条线段将一个矩形分成 $n + 1$ 个区域，现在给定 $m$ 个点的坐标，求每个区域包含多少个点。

### 题解

**二分 叉积**

用叉积判断某个点 $p$ 在有向线段 $se$ 的左边还是右边。若 $\boldsymbol{sp}\times \boldsymbol{se} > 0$，则点 $p$ 在坐厕，小于 $0$ 在右侧。

给定的线段是从左到右的，因此每个点所在的区域可以用二分求解。

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
    int toLeftTest(Point p) {
        if((e - s).cross(p - s) > 0) return 1;
        else if((e - s).cross(p - s) < 0) return -1;
        return 0;
    }
};

int n, m;
Line line[maxn];
Point point[maxn];
int ans[maxn];

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
    for(int i = 0; i <= n; ++i) {
        printf("%d: %d\n", i, ans[i]);
    }
    printf("\n");
}

int main() {
    while(scanf("%d", &n) != EOF && n) {
        memset(ans, 0, sizeof(ans));
        scanf("%d", &m);
        db xl, yl, xr, yr;
        scanf("%lf%lf%lf%lf", &xl, &yl, &xr, &yr);
        line[0].s = Point(xl, yr);
        line[0].e = Point(xl, yl);
        line[n + 1].s = Point(xr, yr);
        line[n + 1].e = Point(xr, yl);
        for(int i = 1; i <= n; ++i) {
            db u, l;
            scanf("%lf%lf", &u, &l);
            line[i].s = Point(l, yr);
            line[i].e = Point(u, yl);
        }
        for(int i = 1; i <= m; ++i) {
            scanf("%lf%lf", &point[i].x, &point[i].y);
        }
        solve();
    }
    return 0;
}
```
