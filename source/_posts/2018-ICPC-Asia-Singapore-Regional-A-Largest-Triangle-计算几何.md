---
title: 2018 ICPC Asia Singapore Regional A. Largest Triangle (计算几何)
date: 2019-10-05 22:35:31
tags:
- ACM刷题
- 凸包
- 计算几何
- 枚举
categories: 竞赛
mathjax: true
---

题目链接：[Kattis - largesttriangle](https://open.kattis.com/problems/largesttriangle)

## Description
> Given $N$ points on a $2$-dimensional space, determine the area of the largest triangle that can be formed using $3$ of those $N$ points. If there is no triangle that can be formed, the answer is $0$.


## Input
> The first line contains an integer $N (3≤N≤5000)$ denoting the number of points. Each of the next $N$ lines contains two integers $x$ and $y (0≤x,y≤4⋅10^7)$. There are no specific constraints on these $N$ points, i.e. the points are not necessarily distinct, the points are not given in specific order, there may be $3$ or more collinear points, etc.
 
## Output
> Print the answer in one line. Your answer should have an absolute error of at most $10^{−5}$.
 

## Sample Input

```markdown
7
0 0
0 5
7 7
0 10
0 0
20 0
10 10
```

## Sample Output

```markdown
100.00000
```

## Source

[2018 ICPC Asia Singapore Regional](https://open.kattis.com/problem-sources/2018%20ICPC%20Asia%20Singapore%20Regional)

## Solution

### 题意

给出 $N$ 个点，选择其中 $3$ 个点组成三角形，求最大面积的三角形的面积，如果不能组成三角形，输出 $0$。

### 题解

最大面积的三角形一定在凸包上，所以先求凸包。

接下来在凸包上枚举三个点。直接三重循环肯定超时。

可以枚举凸包上的两个点，另外一个点根据面积的单调性枚举。时间复杂度 $O(N^2)$。

还有 $O(NlogN)$ 的做法，可以参考[这篇论文](https://arxiv.org/abs/1705.11035)。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef double db;
const db eps = 1e-10;
const db pi = acos(-1.0);
const ll maxn = 5010;

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

db area(Point A, Point B, Point C) {
    return abs((A - B).cross(A - C));
}

typedef vector<Point> Polygon;
Polygon Andrew(vector<Point> p) {
    int n = p.size(), cnt = 0;
    Polygon ans(2 * n);
    sort(p.begin(), p.end());
    for (int i = 0; i < n; ++i) {
        while (cnt >= 2 && (ans[cnt - 1] - ans[cnt - 2]).cross(p[i] - ans[cnt - 2]) < eps) {
            --cnt;
        }
        ans[cnt++] = p[i];
    }
    int t = cnt + 1;
    for (int i = n - 1; i > 0; --i) {
        while (cnt >= t && (ans[cnt - 1] - ans[cnt - 2]).cross(p[i - 1] - ans[cnt - 2]) < eps) {
            --cnt;
        }
        ans[cnt++] = p[i - 1];
    }
    ans.resize(cnt - 1);
    return ans;
}

vector<Point> p;

int main() {
    int n;
    scanf("%d", &n);
    for(int i = 0; i < n; ++i) {
        Point tmp;
        tmp.input();
        p.push_back(tmp);
    }
    p = Andrew(p);
    n = p.size();
    db ans = 0.0;
    if(n < 3) {
        printf("%.5lf\n", ans);
        return 0;
    }
    for(int i = 0; i < n; ++i) {
        int k = i + 2;
        for(int j = i + 1; j < n; ++j) {
            while(k + 1 < n && area(p[i], p[j], p[k]) < area(p[i], p[j], p[k + 1])) {
                ++k;
            }
            ans = max(ans, area(p[i], p[j], p[k]));
        }
    }
    printf("%.5lf\n", ans * 0.5);
    return 0;
}
```
