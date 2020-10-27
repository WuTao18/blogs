---
title: POJ 1066 Treasure Hunt (线段相交)
date: 2019-10-17 20:18:51
tags:
- ACM刷题
- 计算几何
categories: 竞赛
mathjax: true
---

题目链接：[POJ 1066](http://poj.org/problem?id=1066)

## Description
> Archeologists from the Antiquities and Curios Museum (ACM) have flown to Egypt to examine the great pyramid of Key-Ops. Using state-of-the-art technology they are able to determine that the lower floor of the pyramid is constructed from a series of straightline walls, which intersect to form numerous enclosed chambers. 
> 
> Currently, no doors exist to allow access to any chamber. This state-of-the-art technology has also pinpointed the location of the treasure room. What these dedicated (and greedy) archeologists want to do is blast doors through the walls to get to the treasure room. However, to minimize the damage to the artwork in the intervening chambers (and stay under their government grant for dynamite) they want to blast through the minimum number of doors. For structural integrity purposes, doors should only be blasted at the midpoint of the wall of the room being entered. You are to write a program which determines this minimum number of doors.
> 
> An example is shown below:
> ![](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/10/17/1571312763972-1571312764176.png)

<!--more-->

## Input
> The input will consist of one case. The first line will be an integer n (0 <= n <= 30) specifying number of interior walls, followed by n lines containing integer endpoints of each wall x1 y1 x2 y2 . The 4 enclosing walls of the pyramid have fixed endpoints at (0,0); (0,100); (100,100) and (100,0) and are not included in the list of walls. The interior walls always span from one exterior wall to another exterior wall and are arranged such that no more than two walls intersect at any point. You may assume that no two given walls coincide. After the listing of the interior walls there will be one final line containing the floating point coordinates of the treasure in the treasure room (guaranteed not to lie on a wall).
 
## Output
> Print a single line listing the minimum number of doors which need to be created, in the format shown below.


## Sample Input

```markdown
7 
20 0 37 100 
40 0 76 100 
85 0 0 75 
100 90 0 90 
0 71 100 61 
0 14 100 38 
100 47 47 100 
54.5 55.4 
```

## Sample Output

```markdown
Number of doors = 2
```

## Source

[East Central North America 1999](http://poj.org/searchproblem?field=source&key=East+Central+North+America+1999)

## Solution

### 题意

给定 $n$ 条线段，代表围墙，最外层为 $100 \times 100$ 的正方形围墙。给定宝藏的坐标，求从外面进去拿到宝藏最少要穿过几堵墙。

### 思路

枚举正方形边上的所有端点与宝藏构成的线段，与多少堵墙相交，相交最少的即为答案。

线段 $A$ 与线段 $B$ 相交的判断：$A$ 的两个端点在线段 $B$ 的两边且线段 $B$ 的两个端点在线段 $A$ 的两边。 

## Code

```cpp
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cmath>
using namespace std;
typedef double db;
const db eps = 1e-8;
const int inf = 0x3f3f3f3f;

inline int dcmp(db x) {
    if(fabs(x) < eps) return 0;
    return x > 0? 1: -1;
}

class Point {
public:
    double x, y;
    Point(double x = 0, double y = 0) : x(x), y(y) {}
    void input() {
        scanf("%lf%lf", &x, &y);
    }
    bool operator<(const Point &a) const {
        return (!dcmp(x - a.x))? dcmp(y - a.y) < 0: x < a.x;
    }
    bool operator==(const Point &a) const {
        return dcmp(x - a.x) == 0 && dcmp(y - a.y) == 0;
    }
    Point operator+(const Point a) {
        return Point(x + a.x, y + a.y);
    }
    Point operator-(const Point a) {
        return Point(x - a.x, y - a.y);
    }
    Point operator*(double p) {
        return Point(x * p, y * p);
    }
    Point operator/(double p) {
        return Point(x / p, y / p);
    }
    db dot(const Point a) {
        return x * a.x + y * a.y;
    }
    db cross(const Point a) {
        return x * a.y - y * a.x;
    }
};

class Line {
public:
    Point s, e;
    db angle;
    Line() {}
    Line(Point s, Point e) : s(s), e(e) {}
    inline void input() {
        scanf("%lf%lf%lf%lf", &s.x, &s.y, &e.x, &e.y);
    }
    bool operator<(const Line &a) const {
        Line l = a;
        if(dcmp(angle - l.angle) == 0) {
            return l.toLeftTest(s) == 1;
        }
        return angle < l.angle;
    }
    int toLeftTest(Point p) {
        if((e - s).cross(p - s) > 0) return 1;
        else if((e - s).cross(p - s) < 0) return -1;
        return 0;
    }
    int segcrossseg(Line l) {
        if(l.toLeftTest(s) * l.toLeftTest(e) == -1 && toLeftTest(l.s) * toLeftTest(l.e) == -1) {
            return 1;
        } else {
            return 0;
        }
    }
    Point crosspoint(Line l) {
		double a1 = (l.e - l.s).cross(s - l.s);
		double a2 = (l.e - l.s).cross(e - l.s);
        double x = (s.x * a2 - e.x * a1) / (a2 - a1);
        double y = (s.y * a2 - e.y * a1) / (a2 - a1);
        if(dcmp(x) == 0) x = 0;
        if(dcmp(y) == 0) y = 0;
		return Point(x, y);
	}
};

Line l[100];
Point tar;
int n;

int f(Line l1) {
    int sum = 0;
    for(int i = 0; i < n; ++i) {
        if(l[i].segcrossseg(l1)) {
            ++sum;
        }
    }
    return sum;
}

int main() {
    scanf("%d", &n);
    for(int i = 0; i < n; ++i) {
        l[i].input();
    }
    tar.input();
    int ans = inf;
    for(int i = 0; i < n; ++i) {
        Line l1 = Line(l[i].s, tar);
        Line l2 = Line(l[i].e, tar);
        int cnt1 = 0, cnt2 = 0;
        for(int j = 0; j < n; ++j) {
            if(i == j) continue;
            if(l[j].segcrossseg(l1)) {
                ++cnt1;
            }
            if(l[j].segcrossseg(l2)) {
                ++cnt2;
            }
        }
        ans = min(ans, min(cnt1, cnt2));
    }
    Point tmp = Point(0, 0);
    ans = min(ans, f(Line(tmp, tar)));
    tmp = Point(0, 100);
    ans = min(ans, f(Line(tmp, tar)));
    tmp = Point(100, 0);
    ans = min(ans, f(Line(tmp, tar)));
    tmp = Point(100, 100);
    ans = min(ans, f(Line(tmp, tar)));
    printf("Number of doors = %d\n", ans + 1);
    return 0;
}
```
