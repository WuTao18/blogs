---
title: HDU 1392 Surround the Trees (凸包周长)
date: 2019-08-16 22:26:47
tags:
- ACM刷题
- 计算几何
- 模板题
- 凸包
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1392](http://acm.hdu.edu.cn/showproblem.php?pid=1392)

## Problem Description
> There are a lot of trees in an area. A peasant wants to buy a rope to surround all these trees. So at first he must know the minimal required length of the rope. However, he does not know how to calculate it. Can you help him? 
>  
> The diameter and length of the trees are omitted, which means a tree can be seen as a point. The thickness of the rope is also omitted which means a rope can be seen as a line.
> 
> ![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/08/16/1565965282361-1565965283259.png)
> 
> There are no more than 100 trees.


## Input
> The input contains one or more data sets. At first line of each input data set is number of trees in this data set, it is followed by series of coordinates of the trees. Each coordinate is a positive integer pair, and each integer is less than 32767. Each pair is separated by blank.
> 
> Zero at line for number of trees terminates the input for your program.
 

## Output
> The minimal length of the rope. The precision should be 10^-2.

## Sample Input
```markdown
9 
12 7 
24 9 
30 5 
41 9 
80 7 
50 87 
22 9 
45 1 
50 7 
0 
```

## Sample Output
```markdown
243.06
```

## Source

> Asia 1997, Shanghai (Mainland China)

## Solution

### 题意：

给定若干个点的坐标，求凸包周长。

### 思路

**凸包**

凸包入门模板题，我用的是 $Andrew$ 算法 

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const double eps = 1e-8;
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
    while(cin >> n && n) {
        for (int i = 1; i <= n; ++i) {
            scanf("%lf%lf", &p[i].x, &p[i].y);
        }
        int t = Andrew();
        double ans = 0;
        for (int i = 1; i < t; i++) {
            ans += dist(stk[i], stk[i + 1]);
        }
        printf("%.2lf\n", n == 2 ? ans / 2 : ans);
    }
    return 0;
}

```