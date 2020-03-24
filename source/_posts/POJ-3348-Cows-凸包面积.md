---
title: POJ 3348 Cows (凸包面积)
date: 2019-08-17 22:09:49
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

> Your friend to the south is interested in building fences and turning plowshares into swords. In order to help with his overseas adventure, they are forced to save money on buying fence posts by using trees as fence posts wherever possible. Given the locations of some trees, you are to help farmers try to create the largest pasture that is possible. Not all the trees will need to be used.
> 
> However, because you will oversee the construction of the pasture yourself, all the farmers want to know is how many cows they can put in the pasture. It is well known that a cow needs at least 50 square metres of pasture to survive.

## Input
> The first line of input contains a single integer, $n (1 \le n \le 10000)$, containing the number of trees that grow on the available land. The next $n$ lines contain the integer coordinates of each tree given as two integers $x$ and $y$ separated by one space (where $-1000 \le x, y \le 1000$). The integer coordinates correlate exactly to distance in metres (e.g., the distance between coordinate $(10; 11)$ and $(11; 11)$ is one metre).


## Output

> You are to output a single integer value, the number of cows that can survive on the largest field you can construct using the available trees.

## Sample Input
```markdown
4
0 0
0 101
75 0
75 101
```

## Sample Output
```markdown
151
```

## Solution

### 题意

给定一些点，圈出一个最大面积，问每 50 平方养一头牛最多能养多少牛。

### 题解

**凸包面积**

先求出凸包，然后选择其中一个点，将凸包分割成若干个三角形，求这些三角形面积的和即可。

![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/08/17/1566050553724-1566050553750.png)

三角形的面积可以用向量的叉积求。

$$S_{\triangle ABC} = \frac{|\overrightarrow{AB} \times \overrightarrow{AC}|}{2}$$

## Code

```cpp
#include <iostream>
#include <cstdio>
#include <cmath>
#include <algorithm>
#include <vector>
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
    while(cin >> n) {
        for (int i = 1; i <= n; ++i) {
            scanf("%lf%lf", &p[i].x, &p[i].y);
        }
        int t = Andrew();
        double ans = 0;
        for(int i = 2; i < t - 1; ++i) {
            ans += 0.5 * abs(cross(stk[i] - stk[1], stk[i + 1] - stk[1]));
        }
        printf("%d\n", (int)(ans / (50.0))); // 需要强制类型转换
    }
    return 0;
}

```