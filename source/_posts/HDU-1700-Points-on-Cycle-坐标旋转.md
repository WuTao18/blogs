---
title: HDU 1700 Points on Cycle (坐标旋转)
date: 2019-09-28 20:58:08
tags:
- ACM刷题
- 坐标旋转
- 计算几何
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1700](http://acm.hdu.edu.cn/showproblem.php?pid=1700)

## Problem Description
> There is a cycle with its center on the origin.
> 
> Now give you a point on the cycle, you are to find out the other two points on it, to maximize the sum of the distance between each other
> 
> you may assume that the radius of the cycle will not exceed 1000.

## Input
> There are T test cases, in each case there are 2 decimal number representing the coordinate of the given point.

## Output
> For each testcase you are supposed to output the coordinates of both of the unknow points by 3 decimal places of precision
> 
> Alway output the lower one first(with a smaller Y-coordinate value), if they have the same Y value output the one with a smaller X.
> 
>   > NOTE
>   >
>   > when output, if the absolute difference between the coordinate values X1 and X2 is smaller than 0.0005, we assume they are equal.

 

## Sample Input

```markdown
2
1.500 2.000
563.585 1.251
```

## Sample Output

```markdown
0.982 -2.299 -2.482 0.299
-280.709 -488.704 -282.876 487.453
```

## Source

[2007省赛集训队练习赛（1）](http://acm.hdu.edu.cn/search.php?field=problem&key=2007%CA%A1%C8%FC%BC%AF%D1%B5%B6%D3%C1%B7%CF%B0%C8%FC%A3%A81%A3%A9&source=1&searchmode=source)

## Solution

### 题意

给定一个圆形在原点的圆和一个圆上的点，在圆上另外找两个点使得三个点组成的三角形周长最大。

### 思路

**坐标旋转**

坐标旋转模板题。

三角形是等边三角形是周长最大。

让给定的点绕原点旋转 $120\degree$ 和 $240\degree$ 即可。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef double db;
const db eps = 1e-10;
const db pi = acos(-1.0);
const ll inf = 0x3f3f3f3f3f3f3f3f;
const ll maxn = 1e5 + 10;

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
        return (!dcmp(y - a.y))? dcmp(x - a.x) < 0: y < a.y;
    }
    Point Rotate(double rad) {
        return Point(x * cos(rad) - y * sin(rad), x * sin(rad) + y * cos(rad));
    }
};

Point ans[2];

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        Point p;
        p.input();
        double r = 2.0 * pi / 3.0;
        ans[0] = p.Rotate(r);
        ans[1] = p.Rotate(r * 2);
        sort(ans, ans + 2);
        printf("%.3lf %.3lf %.3lf %.3lf\n", ans[0].x, ans[0].y, ans[1].x, ans[1].y);
    }
    return 0;
}
```
