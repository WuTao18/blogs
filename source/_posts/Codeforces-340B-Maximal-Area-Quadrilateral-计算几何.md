---
title: Codeforces 340B - Maximal Area Quadrilateral (计算几何)
date: 2019-09-01 19:25:54
tags:
- ACM刷题
- 计算几何
- 枚举
- 暴力
categories: 竞赛
mathjax: true
---

[Codeforces Round #198 (Div. 2)](https://codeforces.com/contest/340)

题目链接：[Maximal Area Quadrilateral](https://codeforces.com/contest/340/problem/B)

> Iahub has drawn a set of $n$ points in the cartesian plane which he calls "special points". A quadrilateral is a simple polygon without self-intersections with four sides (also called edges) and four vertices (also called corners). Please note that a quadrilateral doesn't have to be convex. A special quadrilateral is one which has all four vertices in the set of special points. Given the set of special points, please calculate the maximal area of a special quadrilateral.

<!--more-->

## Input

> The first line contains integer $n (4 \le n \le 300)$. Each of the next $n$ lines contains two integers: $x_i, y_i ( - 1000 \le xi, yi \le 1000)$ — the cartesian coordinates of ith special point. It is guaranteed that no three points are on the same line. It is guaranteed that no two points coincide.

## Output

> Output a single real number — the maximal area of a special quadrilateral. The answer will be considered correct if its absolute or relative error does't exceed $10 ^{- 9}$.

## Examples

> **input**
> 
> >     5
> >     0 0
> >     0 4
> >     4 0
> >     4 4
> >     2 3
> 
> **output**
> 
> >     16.000000
> 

## Note

> In the test example we can choose first $4$ points to be the vertices of the quadrilateral. They form a square by side $4$, so the area is $4\cdot 4 = 16$.

## Solution

### 题意

给定 $n$ 个点的坐标，选择其中 $4$ 个点构成四边形，求最大四边形面积。

### 题解

四边形的面积等于两个三角形面积的和。枚举四边形的对角线，以及左右两边的点，选择两个面积最大的三角形，更新 $ans$。

三重循环枚举，时间复杂度 $O(n^3)$。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef double db;
const db eps = 1e-10;  
const db pi = acos(-1.0);
const int inf = 0x3f3f3f3f;
const int maxn = 300 + 5;

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

vector<Point> p;

map<pair<int, int>, int> mp;

int main() {
    double ans = 0;
    int n;
    scanf("%d", &n);
    for(int i = 0; i < n; ++i) {
        Point tmp;
        tmp.input();
        p.push_back(tmp);
    }
    sort(p.begin(), p.end());
    for(int i = 0; i < p.size(); ++i) {
        for(int j = i + 1; j < p.size(); ++j) {
            if(!mp[{i, j}]) {
                mp[{i, j}] = 1;
                double s1 = 0, s2 = 0;
                for(int k = 0; k < p.size(); ++k) {
                    if(k == i || k == j) continue;
                    if(dcmp((p[j] - p[i]).cross(p[k] - p[i])) > 0) {
                        s1 = max(s1, fabs((p[j] - p[i]).cross(p[k] - p[i])) * 0.5);
                    } else {
                        s2 = max(s2, fabs((p[j] - p[i]).cross(p[k] - p[i])) * 0.5);
                    }
                }
                if(dcmp(s1) == 0 || dcmp(s2) == 0) continue;
                ans = max(ans, s1 + s2);
            }
        }
    }
    printf("%.10lf\n", ans);
    return 0;
}
```