---
title: POJ 1113 Wall (凸包)
date: 2019-08-17 15:26:13
tags:
- ACM刷题
- 计算几何
- 模板题
- 凸包
categories: 竞赛
mathjax: true
---

题目链接：[POJ 1113](https://vjudge.net/problem/POJ-1113)

## Description

> Once upon a time there was a greedy King who ordered his chief Architect to build a wall around the King's castle. The King was so greedy, that he would not listen to his Architect's proposals to build a beautiful brick wall with a perfect shape and nice tall towers. Instead, he ordered to build the wall around the whole castle using the least amount of stone and labor, but demanded that the wall should not come closer to the castle than a certain distance. If the King finds that the Architect has used more resources to build the wall than it was absolutely necessary to satisfy those requirements, then the Architect will loose his head. Moreover, he demanded Architect to introduce at once a plan of the wall listing the exact amount of resources that are needed to build the wall. 
> 
> ![](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/08/17/1566026079827-1566026080317.png)
> 
> Your task is to help poor Architect to save his head, by writing a program that will find the minimum possible length of the wall that he could build around the castle to satisfy King's requirements. 
> 
> The task is somewhat simplified by the fact, that the King's castle has a polygonal shape and is situated on a flat ground. The Architect has already established a Cartesian coordinate system and has precisely measured the coordinates of all castle's vertices in feet.

<!--more-->

## Input
> The first line of the input file contains two integer numbers N and L separated by a space. N (3 <= N <= 1000) is the number of vertices in the King's castle, and L (1 <= L <= 1000) is the minimal number of feet that King allows for the wall to come close to the castle. 
> 
> Next N lines describe coordinates of castle's vertices in a clockwise order. Each line contains two integer numbers Xi and Yi separated by a space (-10000 <= Xi, Yi <= 10000) that represent the coordinates of ith vertex. All vertices are different and the sides of the castle do not intersect anywhere except for vertices.


## Output

> Write to the output file the single number that represents the minimal possible length of the wall in feet that could be built around the castle to satisfy King's requirements. You must present the integer number of feet to the King, because the floating numbers are not invented yet. However, you must round the result in such a way, that it is accurate to 8 inches (1 foot is equal to 12 inches), since the King will not tolerate larger error in the estimates.

## Sample Input
```markdown
9 100
200 400
300 400
300 300
400 300
400 400
500 400
500 200
350 200
200 200
```

## Sample Output
```markdown
1628
```

## Hint
> 结果四舍五入就可以了

## Solution

### 题意

给定一个有 $N$ 个顶点的城堡，现在要造墙将城堡围起来，且墙必须离城堡的边至少 $L$ 单位远，并且墙的总长度尽量小。求此长度。

### 题解

**凸包**

凸包的周长加上以 $L$ 为半径的圆的周长即可。

## Code

```cpp
#include <iostream>
#include <cmath>
#include <cstdio>
#include <algorithm>
using namespace std;
const double eps = 1e-8;
const double pi = acos(-1.0);
const int maxn = 1e5 + 10;

int n;
struct Point {
    double x, y;
    Point() {}
    Point(double a, double b) : x(a), y(b) {}
    bool operator<(const Point &b) const {
        if (x < b.x) return 1;
        if (x > b.x) return 0;
        return y < b.y;
    }
    Point operator-(const Point &b) {
        return Point(x - b.x, y - b.y);
    }
} p[maxn], stk[maxn];
typedef Point Vec;

int sgn(double x)  {
    if (fabs(x) <= eps)
        return 0;
    return x > 0 ? 1 : -1;
}

double dist(Point a, Point b) {
    return sqrt((a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y));
}

double cross(Vec a, Vec b) {
    return a.x * b.y - a.y * b.x;
}

int Andrew() {
    sort(p + 1, p + 1 + n);
    int len = 0;
    for (int i = 1; i <= n; ++i) {
        while (len > 1 && sgn(cross(stk[len] - stk[len - 1], p[i] - stk[len - 1])) == -1) {
            len--;
        }
        stk[++len] = p[i];
    }
    int k = len;
    for (int i = n - 1; i >= 1; --i) {
        while (len > k && sgn(cross(stk[len] - stk[len - 1], p[i] - stk[len - 1])) == -1) {
            len--;
        }
        stk[++len] = p[i];
    }
    return len;
}

int main() {
    int l;
    while(cin >> n >> l) {
        for (int i = 1; i <= n; ++i) {
            scanf("%lf%lf", &p[i].x, &p[i].y);
        }
        int t = Andrew();
        double ans = 0;
        for (int i = 1; i < t; i++) {
            ans += dist(stk[i], stk[i + 1]);
        }
        ans += 2 * pi * l;
        printf("%d\n", int(ans + 0.5));
    }
    return 0;
}

```