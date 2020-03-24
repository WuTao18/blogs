---
title: POJ 1474 Video Surveillance (半平面交)
date: 2019-09-14 18:56:30
tags:
- ACM刷题
- 计算几何
- 半平面交
categories: 竞赛
mathjax: true
---

题目链接：[POJ 1474](http://poj.org/problem?id=1474)

## Description
> A friend of yours has taken the job of security officer at the Star-Buy Company, a famous depart- ment store. One of his tasks is to install a video surveillance system to guarantee the security of the customers (and the security of the merchandise of course) on all of the store's countless floors. As the company has only a limited budget, there will be only one camera on every floor. But these cameras may turn around to look in every direction.
> 
> The first problem is to choose where to install the camera for every floor. The only requirement is that every part of the room must be visible from there. In the following figure the left floor can be completely surveyed from the position indicated by a dot, while for the right floor, there is no such position, the given position failing to see the lower left part of the floor.
> 
> 
> 
> Before trying to install the cameras, your friend first wants to know whether there is indeed a suitable position for them. He therefore asks you to write a program that, given a ground plan, de- termines whether there is a position from which the whole floor is visible. All floor ground plans form rectangular polygons, whose edges do not intersect each other and touch each other only at the corners.


## Input
> The input contains several floor descriptions. Every description starts with the number $n$ of vertices that bound the floor $(4 \le n \le 100)$. The next $n$ lines contain two integers each, the $x$ and $y$ coordinates for the $n$ vertices, given in clockwise order. All vertices will be distinct and at corners of the polygon. Thus the edges alternate between horizontal and vertical.
> 
> A zero value for $n$ indicates the end of the input.
 
## Output
> For every test case first output a line with the number of the floor, as shown in the sample output. Then print a line stating "Surveillance is possible." if there exists a position from which the entire floor can be observed, or print "Surveillance is impossible." if there is no such position.
> 
> Print a blank line after each test case.
 

## Sample Input

```markdown
4
0 0
0 1
1 1
1 0
8
0 0
0 2
1 2
1 1
2 1
2 2
3 2
3 0
0
```

## Sample Output

```markdown
Floor #1
Surveillance is possible.

Floor #2
Surveillance is impossible.

```

## Source

[Southwestern European Regional Contest 1997](http://poj.org/searchproblem?field=source&key=Southwestern+European+Regional+Contest+1997)

## Solution

### 题意

要在房间内安装一个可以 360° 旋转的监控，问是否存在一个位置使得监控能看到整个房间。

### 题解

**半平面交**

半平面交求多边形是否有核。

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
    db cross(const Point a) {
        return x * a.y - y * a.x;
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

int half_plane(int cnt) {
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

    return tail - head + 1 > 2;
}

int main() {
    int n;
    int _ = 0;
    while(~scanf("%d", &n) && n) {
        printf("Floor #%d\n", ++_);
        int cnt = 0;
        db s = 0;
        for(int i = 1; i <= n; ++i) {
            p[i].input();
        }
        for(int i = n - 1; i >= 1; --i) {
            l[++cnt].e = p[i];
            l[cnt].s = p[i + 1];
            l[cnt].get_angle();
        }
        l[++cnt].e = p[n];
        l[cnt].s = p[1];
        l[cnt].get_angle();
        if(half_plane(cnt) == 0) {
            printf("Surveillance is impossible.\n");
        } else {
            printf("Surveillance is possible.\n");
        }
        printf("\n");
    }
    return 0;
}
```
