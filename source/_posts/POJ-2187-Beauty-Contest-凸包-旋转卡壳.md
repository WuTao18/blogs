---
title: POJ 2187 Beauty Contest (凸包 旋转卡壳)
date: 2019-08-20 18:25:05
tags:
- ACM刷题
- 计算几何
- 凸包
- 旋转卡壳
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2187](http://poj.org/problem?id=2187)

## Description

> Bessie, Farmer John's prize cow, has just won first place in a bovine beauty contest, earning the title 'Miss Cow World'. As a result, Bessie will make a tour of N (2 <= N <= 50,000) farms around the world in order to spread goodwill between farmers and their cows. For simplicity, the world will be represented as a two-dimensional plane, where each farm is located at a pair of integer coordinates (x,y), each having a value in the range -10,000 ... 10,000. No two farms share the same pair of coordinates. 
> 
> Even though Bessie travels directly in a straight line between pairs of farms, the distance between some farms can be quite large, so she wants to bring a suitcase full of hay with her so she has enough food to eat on each leg of her journey. Since Bessie refills her suitcase at every farm she visits, she wants to determine the maximum possible distance she might need to travel so she knows the size of suitcase she must bring.Help Bessie by computing the maximum distance among all pairs of farms. 

<!--more-->

## Input

> $*$ Line 1: A single integer, N 
> 
> $*$ Lines 2..N+1: Two space-separated integers x and y specifying coordinate of each farm

## Output
> $*$ Line 1: A single integer that is the squared distance between the pair of farms that are farthest apart from each other. 

## Sample Input

```markdown
4
0 0
0 1
1 1
1 0
```

## Sample Output

```markdown
2
```

## Hint
> Farm 1 (0, 0) and farm 3 (1, 1) have the longest distance (square root of 2) 


## Solution

### 题意

给定 $n$ 个点的坐标，求这些点构成的凸包的直径的平方。

### 题解

**凸包 旋转卡壳**

旋转卡壳求凸包直径的模板题。

```cpp
#include <cstdio>
#include <iostream>
#include <cmath>
#include <vector>
#include <algorithm>
using namespace std;
typedef long long ll;
const double eps = 1e-8;
const int inf = 0x3f3f3f3f;
const int maxn = 100000 + 5;

int n;

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
        if (x == a.x && y == a.y)
            return 1;
        return 0;
    }
};

typedef Point Vector;

// 叉积
double cross(Vector a, Vector b) {
    return a.x * b.y - a.y * b.x;
}

// 点积
double dot(Vector a, Vector b) {
    return a.x * b.x + a.y * b.y;
}

// 判断 p2 是否在向量 p0p1 的顺时针方向
bool isclock(Point p0, Point p1, Point p2) {
    Vector a = p1 - p0;
    Vector b = p2 - p0;
    if (cross(a, b) < -eps)
        return true;
    return false;
}

// 点a 点b的距离
double getDistance(Point a, Point b) {
    return (pow(a.x - b.x, 2) + pow(a.y - b.y, 2));
}

typedef vector<Point> Polygon;
// 凸包
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
        for(int n = l.size() ; n >= 2 && !isclock(l[n - 2], l[n - 1], s[i]); --n) {
            l.pop_back();
        }
        l.push_back(s[i]);
    }
    for(int i = 1 ; i < u.size() - 1 ; i++) l.push_back(u[i]);
    return l;
}

// 点 x到直线 ab 的距离
double dis(Point a, Point b, Point x) {
    return fabs(dot((a - x), (b - x)) / getDistance(a, b));
}

// 旋转卡壳求凸包直径
double rotating_caliper(vector<Point> v) {
	double max_dis = 0.0;
	int n = v.size();
	if (n == 2) {
		return getDistance(v[0], v[1]);
	}

    v.push_back(v[0]);
    int j = 2;
    for (int i = 0; i < n; ++i) {
        while (dis(v[i], v[i + 1], v[j]) < dis(v[i], v[i + 1], v[j + 1])) {
            j = (j + 1) % n;
        }
        max_dis = max(max_dis, max(getDistance(v[j], v[i]), getDistance(v[j], v[i + 1])));
    }
	
	return max_dis;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    cin >> n;
    Polygon s;
    for(int i = 0; i < n; ++i) {
        Point p;
        cin >> p.x >> p.y;
        s.push_back(p);
    }
    Polygon p = Andrew(s);
    double d = rotating_caliper(p);
    cout << d << endl;
    return 0;
}
```