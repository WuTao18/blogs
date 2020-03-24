---
title: Codeforces 166B - Polygon (判断凸包位置关系)
date: 2019-08-27 22:17:44
tags:
- ACM刷题
- 计算几何
- 凸包
categories: 竞赛
mathjax: true
---

[Codeforces Round #113 (Div. 2)](https://codeforces.com/contest/166)

题目链接：[Polygons](https://codeforces.com/contest/166/problem/B)

> You've got another geometrical task. You are given two non-degenerate polygons $A$ and $B$ as vertex coordinates. Polygon $A$ is strictly convex. Polygon $B$ is an arbitrary polygon without any self-intersections and self-touches. The vertices of both polygons are given in the clockwise order. For each polygon no three consecutively following vertices are located on the same straight line.
> 
> Your task is to check whether polygon $B$ is positioned strictly inside polygon $A$. It means that any point of polygon $B$ should be strictly inside polygon $A$. "Strictly" means that the vertex of polygon $B$ cannot lie on the side of the polygon $A$.

## Input

> The first line contains the only integer $n (3 \le n \le 10^5)$ — the number of vertices of polygon $A$. Then $n$ lines contain pairs of integers $x_i, y_i (|x_i|, |y_i| \le 10^9)$ — coordinates of the i-th vertex of polygon $A$. The vertices are given in the clockwise order.
> 
> The next line contains a single integer $m (3 \le m \le 2·10^4)$ — the number of vertices of polygon $B$. Then following $m$ lines contain pairs of integers $x_j, y_j (|x_j|, |y_j| \le 10^9)$ — the coordinates of the $j$-th vertex of polygon $B$. The vertices are given in the clockwise order.
> 
> The coordinates of the polygon's vertices are separated by a single space. It is guaranteed that polygons $A$ and $B$ are non-degenerate, that polygon $A$ is strictly convex, that polygon $B$ has no self-intersections and self-touches and also for each polygon no three consecutively following vertices are located on the same straight line.

## Output

> Print on the only line the answer to the problem — if polygon $B$ is strictly inside polygon $A$, print "YES", otherwise print "NO" (without the quotes).

## Examples

> **input**
> 
> >     6
> >     -2 1
> >     0 3
> >     3 3
> >     4 1
> >     3 -2
> >     2 -2
> >     4
> >     0 1
> >     2 2
> >     3 1
> >     1 0
> 
> **output**
> 
> >     YES
> 
> **input**
> >     5
> >     1 2
> >     4 2
> >     3 -3
> >     -2 -2
> >     -2 1
> >     4
> >     0 1
> >     1 2
> >     4 1
> >     2 -1
> 
> **output**
> 
> >     NO
> 
> **input**
> 
> >     5
> >     -1 2
> >     2 3
> >     4 1
> >     3 -2
> >     0 -3
> >     5
> >     1 0
> >     1 1
> >     3 1
> >     5 -1
> >     2 -1
> 
> **output**
> 
> >     NO

## Solution

### 题意

给定两个凸包 $A$ 和 $B$。判断凸包 $B$ 是否严格在凸包 $A$ 内。

### 题解

对凸包 $A$ 和 $B$ 的所有点构造凸包，判断该凸包是否等于凸包 $A$。若相等，则凸包 $B$ 严格在凸包 $A$ 内。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const double eps = 1e-8;
const double pi = acos(-1.0);

int dcmp(double x)  {
    if (fabs(x) <= eps)
        return 0;
    return x > 0 ? 1 : -1;
}

class Point {
public:
    double x, y;
    Point(double x = 0, double y = 0) : x(x), y(y) {}
    Point operator+(Point a) {
        return Point(a.x + x, a.y + y);
    }
    Point operator-(Point a) {
        return Point(x - a.x, y - a.y);
    }
    bool operator<(const Point &a) const {
        if (x == a.x)
            return y < a.y;
        return x < a.x;
    }
    bool operator==(const Point &a) const {
        if (fabs(x - a.x) < eps && fabs(y - a.y) < eps)
            return 1;
        return 0;
    }
    bool operator!=(const Point &a) const {
        if ((*this) == a)
            return 0;
        return 1;
    }
    double length() {
        return sqrt(x * x + y * y);
    }
};

typedef Point Vector;

double cross(Vector a, Vector b) {
    return a.x * b.y - a.y * b.x;
}

double dot(Vector a, Vector b) {
    return a.x * b.x + a.y * b.y;
}

bool isclock(Point p0, Point p1, Point p2) {
    Vector a = p1 - p0;
    Vector b = p2 - p0;
    if (dcmp(cross(a, b)) <= 0)  // 为了让凸包边上可以有点
        return true;
    return false;
}

double getDistance(Point a, Point b) {
    return sqrt(pow(a.x - b.x, 2) + pow(a.y - b.y, 2));
}

typedef vector<Point> Polygon;
Polygon Andrew(Polygon s) {
    Polygon u, l;
    if(s.size() < 3) return s;
    sort(s.begin(), s.end());
    u.push_back(s[0]);
    u.push_back(s[1]);
    l.push_back(s[s.size() - 1]);
    l.push_back(s[s.size() - 2]);
    for(int i = 2 ; i < s.size() ; ++i) {
        for(int n = u.size() ; n >= 2 && !isclock(u[n - 2], u[n - 1], s[i]); --n) {
            u.pop_back();
        }
        u.push_back(s[i]);
    }
    for(int i = s.size() - 3 ; i >= 0 ; --i) {
        for(int n = l.size() ; n >=2 && !isclock(l[n-2],l[n-1],s[i]); --n) {
            l.pop_back();
        }
        l.push_back(s[i]);
    }
    for(int i = 1 ; i < u.size() - 1 ; i++) l.push_back(u[i]);
    return l;
}

// 判断点在线段上
bool OnSegment(Point p, Point a1, Point a2) {
    return dcmp(cross(a1 - p, a2 - p)) == 0 && dcmp(dot(a1 - p, a2 - p)) < 0;
}

// 判断点在凸包内
int isPointInPolygon(Point p, vector<Point> s) {
    int wn = 0, cc = s.size();
    for (int i = 0; i < cc; i++) {
        Point p1 = s[i];
        Point p2 = s[(i + 1) % cc];
        if (p1 == p || p2 == p || OnSegment(p, p1, p2)) return -1;
        int k = dcmp(cross(p2 - p1, p - p1));
        int d1 = dcmp(p1.y - p.y);
        int d2 = dcmp(p2.y - p.y);
        if (k > 0 && d1 <= 0 && d2 > 0) wn++;
        if (k < 0 && d2 <= 0 && d1 > 0) wn--;
    }
    if (wn != 0) return 1;
    return 0;
}

int main() {
    Polygon A, B;
    int n;
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        double x, y;
        scanf("%lf%lf", &x, &y);
        A.push_back(Point(x, y));
        B.push_back(Point(x, y));
    }
    scanf("%d", &n);
    for (int i = 0; i < n; ++i) {
        double x, y;
        scanf("%lf%lf", &x, &y);
        B.push_back(Point(x, y));
    }
    A = Andrew(A);
    B = Andrew(B);
    if(A.size() != B.size()) {
        printf("NO\n");
        return 0;
    }
    int flag = 1;
    sort(A.begin(), A.end());
    sort(B.begin(), B.end());
    for(int i = 0; i < A.size(); ++i) {
        if(A[i] != B[i]) {
            flag = 0;
            break;
        }
    }
    if(flag) {
        printf("YES\n");
    } else {
        printf("NO\n");
    }
    return 0;
}
```