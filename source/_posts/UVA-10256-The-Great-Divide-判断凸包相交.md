---
title: UVA 10256 The Great Divide (判断凸包相交)
date: 2019-08-21 22:43:19
tags:
- ACM刷题
- 计算几何
- 凸包
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[UVA 10256](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=1197)

## 题意

有n个红点和m个蓝点,问是否存在一条直线能够分开红点和蓝点。

<!--more-->

## 题解

分别求出红点和蓝点的凸包，判断两个凸包是否相交。

凸包不相交的条件：
- 凸包上的任意点都在另一个凸包的外面
- 凸包的任意线段都与另一个凸包不相交

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const double eps = 1e-8;
const double pi = acos(-1.0);
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
    if (cross(a, b) < -eps)
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

int dcmp(double x)  {
    if (fabs(x) <= eps)
        return 0;
    return x > 0 ? 1 : -1;
}

// 判断点在线段上
bool OnSegment(Point p, Point a1, Point a2) {
    return dcmp(cross(a1 - p, a2 - p)) == 0 && dcmp(dot(a1 - p, a2 - p)) < 0;
}

// 判断线段相交
bool Intersection(Point a1, Point a2, Point b1, Point b2) {
    double c1 = cross(a2 - a1, b1 - a1), c2 = cross(a2 - a1, b2 - a1),
            c3 = cross(b2 - b1, a1 - b1), c4 = cross(b2 - b1, a2 - b1);
    return dcmp(c1) * dcmp(c2) < 0 && dcmp(c3) * dcmp(c4) < 0;
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

void solve(Polygon s1, Polygon s2) {
    int c1 = s1.size(), c2 = s2.size();
    for(int i = 0; i < c1; ++i) {
        if(isPointInPolygon(s1[i], s2)) {
            printf("No\n");
            return;
        }
    }
    for(int i = 0; i < c2; ++i) {
        if(isPointInPolygon(s2[i], s1)) {
            printf("No\n");
            return;
        }
    }
    for (int i = 0; i < c1; i++) {
        for (int j = 0; j < c2; j++) {
            if (Intersection(s1[i], s1[(i + 1) % c1], s2[j], s2[(j + 1) % c2])) {
                printf("No\n");
                return;
            }
        }
    }
    printf("Yes\n");
}

int main() {
    int n, m;
    while (cin >> n >> m) {
        if(n == 0 && m == 0) break;
        Polygon s1, s2;
        for (int i = 0; i < n; ++i) {
            double x1, x2;
            scanf("%lf%lf", &x1, &x2);
            s1.push_back(Point(x1, x2));
        }
        for (int i = 0; i < m; ++i) {
            double x1, x2;
            scanf("%lf%lf", &x1, &x2);
            s2.push_back(Point(x1, x2));
        }
        if(s1.size()) s1 = Andrew(s1);
        if(s2.size()) s2 = Andrew(s2);
        solve(s1, s2);
    }
    return 0;
}
```

## 参考

> [《算法竞赛入门经典》 刘汝佳 / 陈锋 ](https://book.douban.com/subject/20254543/)