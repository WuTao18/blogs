---
title: POJ 3304 Segments (判断直线与线段相交)
date: 2019-08-26 10:44:19
tags:
- ACM刷题
- 计算几何
- 枚举
- ToLeftTest
categories: 竞赛
mathjax: true
---

题目链接：[POJ 3304](http://poj.org/problem?id=3304)

## Problem Description
> Given n segments in the two dimensional space, write a program, which determines if there exists a line such that after projecting these segments on it, all projected segments have at least one point in common.
 

## Input
> Input begins with a number T showing the number of test cases and then, T test cases follow. Each test case begins with a line containing a positive integer n ≤ 100 showing the number of segments. After that, n lines containing four real numbers x1 y1 x2 y2 follow, in which (x1, y1) and (x2, y2) are the coordinates of the two endpoints for one of the segments.
 

## Output
> For each test case, your program must output "Yes!", if a line with desired property exists and must output "No!" otherwise. You must assume that two floating point numbers a and b are equal if |a - b| < 10-8.
 

## Sample Input

```markdown
3
2
1.0 2.0 3.0 4.0
4.0 5.0 6.0 7.0
3
0.0 0.0 0.0 1.0
0.0 1.0 0.0 2.0
1.0 1.0 2.0 1.0
3
0.0 0.0 0.0 1.0
0.0 2.0 0.0 3.0
1.0 1.0 2.0 1.0
```

## Sample Output

```markdown
Yes!
Yes!
No!
```

## Solution

### 题意

给定 $n$ 条线段，判断是否存在一条直线，使得所有线段在这条直线上的投影有公共部分。

### 题解

**枚举 ToLeftTest**

如果存在满足条件的一条直线，那么该直线的一条垂线一定与所有线段相交。

如果 $n \le 2$ 时一定存在。

枚举所有线段的端点中任意两点构造的直线，判断是否所有线段都与之相交。

判断线段与直线相交的方法：判断线段的两个端点是否在直线的同一侧。

注意不要枚举两个相同的端点。

## Code

```cpp
#include <cstdio>
#include <iostream>
#include <cmath>
#include <algorithm>
#include <vector>
using namespace std;
typedef long long ll;
typedef double db;
const db eps = 1e-10;  
const db pi = acos(-1.0);  
const ll inf = 0x3f3f3f3f3f3f3f3f;  
const ll maxn = 1e3 + 10;

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
    db dis2(const Point a) {
        return pow(x - a.x, 2) + pow(y - a.y, 2);
    }
    db dis(const Point a) {
        return sqrt(dis2(a));
    }

    db dis2() {
        return x * x + y * y;
    }
    db dis() {
        return sqrt(dis2());
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
typedef Point Vector;

class Line {
public:
    Point s, e;
    Line() {}
    Line(Point s, Point e) : s(s), e(e) {}
    void input() {
        scanf("%lf%lf%lf%lf", &s.x, &s.y, &e.x, &e.y);
    }
    int toLeftTest(Point p) {
        if((e - s).cross(p - s) > 0) return 1;
        else if((e - s).cross(p - s) < 0) return -1;
        return 0;
    }
};

Line line[maxn];
Point point[maxn];

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        int n;
        scanf("%d", &n);
        for(int i = 0; i < n; ++i) {
            line[i].input();
            point[i * 2] = line[i].s;
            point[i * 2 + 1] = line[i].e;
        }
        if(n < 3) {
            printf("Yes!\n");
            continue;
        }
        int fg = 0;
        for(int i = 0; i < 2 * n; ++i) {
            for(int j = i + 1; j < 2 * n; ++j) {
                if(point[i] == point[j]) continue;
                Line l = Line(point[i], point[j]);
                int flag = 1;
                for(int k = 0; k < n; ++k) {
                    if(l.toLeftTest(line[k].s) + l.toLeftTest(line[k].e) && l.toLeftTest(line[k].s) == l.toLeftTest(line[k].e)) {
                        flag = 0;
                        break;
                    }
                }
                if(flag == 1) {
                    printf("Yes!\n");
                    fg = 1;
                    break;
                }
            }
            if(fg == 1) {break;}
        }
        if(!fg) {
            printf("No!\n");
        }
    }
    return 0;
}
```
