---
title: POJ 3130 How I Mathematician Wonder What You Are! (半平面交)
date: 2019-09-11 18:21:08
tags:
- ACM刷题
- 计算几何
- 半平面交
categories: 竞赛
mathjax: true
---

题目链接：[POJ 3130](http://poj.org/problem?id=3130)

## Problem Description
> After counting so many stars in the sky in his childhood, Isaac, now an astronomer and a mathematician uses a big astronomical telescope and lets his image processing program count stars. The hardest part of the program is to judge if shining object in the sky is really a star. As a mathematician, the only way he knows is to apply a mathematical definition of stars.
> 
> The mathematical definition of a star shape is as follows: A planar shape $F$ is star-shaped if and only if there is a point $C \in F$ such that, for any point $P \in F$, the line segment $CP$ is contained in $F$. Such a point $C$ is called a center of $F$. To get accustomed to the definition let’s see some examples below.
> 
> ![](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/09/11/1568197222877-1568197222880.png)
> 
> The first two are what you would normally call stars. According to the above definition, however, all shapes in the first row are star-shaped. The two in the second row are not. For each star shape, a center is indicated with a dot. Note that a star shape in general has infinitely many centers. Fore Example, for the third quadrangular shape, all points in it are centers.
> 
> Your job is to write a program that tells whether a given polygonal shape is star-shaped or not.

<!--more-->

## Input
> The input is a sequence of datasets followed by a line containing a single zero. Each dataset specifies a polygon, and is formatted as follows.
> 
> >     n	
> >     x1	y1
> >     x2	y2
> >     
> >     …
> >     
> >     xn	yn
> 
> The first line is the number of vertices, $n$, which satisfies $4 \le n \le 50$. Subsequent $n$ lines are the $x$- and $y$-coordinates of the $n$ vertices. They are integers and satisfy $0 \le x_i \le 10000$ and $0 \le yi \le 10000 (i = 1, …, n)$. Line segments $(x_i, y_i)–(x_{i + 1}, y_{i + 1}) (i = 1, …, n − 1)$ and the line segment $(x_n, y_n)–(x_1, y_1)$ form the border of the polygon in the counterclockwise order. That is, these line segments see the inside of the polygon in the left of their directions.
> 
> You may assume that the polygon is simple, that is, its border never crosses or touches itself. You may assume assume that no three edges of the polygon meet at a single point even when they are infinitely extended.
 
## Output
> For each dataset, output “1” if the polygon is star-shaped and “0” otherwise. Each number must be in a separate line and the line should not contain any other characters.   
 

## Sample Input

```markdown
6 
66 13 
96 61 
76 98 
13 94 
4 0 
45 68 
8 
27 21 
55 14 
93 12 
56 95 
15 48 
38 46 
51 65 
64 31 
0
```

## Sample Output

```markdown
1
0
```

## Source

[Japan 2006](http://poj.org/searchproblem?field=source&key=Japan+2006)

## Solution

### 题意

给定 $n$ 个点的多边形，求多边形是否有核。

### 题解

**半平面交**

半平面交求多边形的核的面积，如果面积为 0，就没有核。

## Code

```cpp
#include <cstdio>
#include <iostream>
#include <cmath>
#include <algorithm>
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
    inline void input() {
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
    db ang(Point a) {
        return acos((a.dis() * dis()) / dot(a));
    }
};
typedef Point Vector;

Point p[maxn], ip[maxn];

class Line {
public:
    Point s, e;
    db angle;
    Line() {}
    Line(Point s, Point e) : s(s), e(e) {}
    inline void input() {
        s.input();e.input();
    }
    bool operator<(const Line &a) const {
        Line l = a;
        if(dcmp(angle - l.angle) == 0) {
            return l.toLeftTest(s) == 1;
        }
        return angle < l.angle;
    }
    void get_angle() {
        angle = atan2(e.y - s.y, e.x - s.x);
    }
    int toLeftTest(Point p) {
        if((e - s).cross(p - s) > 0) return 1;
        else if((e - s).cross(p - s) < 0) return -1;
        return 0;
    }
    int linecrossline(Line l) {
        if(dcmp((e - s).cross(l.e - l.s)) == 0) {
            if(dcmp((l.s - e).cross(l.e - s)) == 0) {
                return 0;
            }
            return 1;
        }
        return 2;
    }
    Point crosspoint(Line l) {
		db a1 = (l.e - l.s).cross(s - l.s);
		db a2 = (l.e - l.s).cross(e - l.s);
        db x = (s.x * a2 - e.x * a1) / (a2 - a1);
        db y = (s.y * a2 - e.y * a1) / (a2 - a1);
        if(dcmp(x) == 0) x = 0;
        if(dcmp(y) == 0) y = 0;
		return Point(x, y);
	}
};

Line l[maxn], q[maxn];

db half_plane(int cnt) {
    sort(l + 1, l + 1 + cnt);
    int tmp = 1;
    for(int i = 2; i <= cnt; ++i) {
        if(dcmp(l[i].angle - l[tmp].angle) == 1) l[++tmp] = l[i];
    }
    cnt = tmp;
    int head = 1, tail = 2;
    q[1] = l[1], q[2] = l[2];
    for(int i = 3; i <= cnt; ++i) {
        while(head < tail && l[i].toLeftTest(q[tail].crosspoint(q[tail - 1])) == -1) {
            --tail;
        }
        while(head < tail && l[i].toLeftTest(q[head].crosspoint(q[head + 1])) == -1) {
            ++head;
        }
        q[++tail] = l[i];
    }

    while(head < tail && q[head].toLeftTest(q[tail].crosspoint(q[tail - 1])) == -1) {
        --tail;
    }
    while(head < tail && q[tail].toLeftTest(q[head].crosspoint(q[head + 1])) == -1) {
        ++head;
    }

    if(tail - head + 1 <= 2) {
        return 0.0;
    }

    tmp = 0;
    for(int i = head; i < tail; ++i) {
        ip[++tmp] = q[i].crosspoint(q[i + 1]);
    }
    ip[++tmp] = q[head].crosspoint(q[tail]);
    db ans = 0;
    for(int i = 3; i <= tmp; ++i) {
        ans += (ip[i - 1] - ip[1]).cross(ip[i] - ip[1]);
    }
    return ans * 0.5;
}

int main() {
    int n;
    while(~scanf("%d", &n) && n) {
        int cnt = 0;
        for(int i = 1; i <= n; ++i) {
            p[i].input();
            if(i > 1) {
                l[++cnt].e = p[i];
                l[cnt].s = p[i - 1];
                l[cnt].get_angle();
            }
        }
        l[++cnt].e = p[1];
        l[cnt].s = p[n];
        l[cnt].get_angle();
        if(dcmp(half_plane(cnt)) == 0) {
            printf("0\n");
        } else {
            printf("1\n");
        }
    }
    return 0;
}
```
