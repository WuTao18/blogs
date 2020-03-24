---
title: HDU 6697 Closest Pair of Segments (计算几何 暴力)
date: 2019-08-24 13:28:05
tags:
- ACM刷题
- 计算几何
- 暴力
- 枚举
- 剪枝
categories: 竞赛
mathjax: true
---

2019 杭电多校 10 1007

题目链接：[HDU 6697](http://acm.hdu.edu.cn/showproblem.php?pid=6697)

比赛链接：[2019 Multi-University Training Contest 10](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+10&source=1&searchmode=source)

## Problem Description
> The closest pair of points problem is a well-known problem of computational geometry. In this problem, you are given $n$ points in the Euclidean plane and you need to find a pair of points with the smallest distance between them. 
> 
> Now, Claris, the brilliant one who has participated in programming contests for several years, is trying to solve a harder problem named the closest pair of segments problem, which also has a quite simple description as above. 
> 
> However, the problem seems even too hard for Claris and she is asking you for help. 
> 
> Now $n$ segments are lying on the Euclidean plane, you are asked to pick two different segments and then pick a point on the two segments respectively to minimize the distance between these two points. 
> 
> For simplicity, any two given segments share no common point, and you don't need to show her the two chosen points, but the distance between them instead. 
 

## Input
> The input contains several test cases, and the first line contains a single integer $T (1\le T\le 200)$, the number of test cases. 
> 
> For each test case, the first line contains one integer $n (2\le n\le 10000)$, which is the number of segments on the Euclidean plane. 
> 
> The following $n$ lines describe all the segments lying on the Euclidean plane, the $i$-th of which contains for integers $x_1,y_1,x_2$ and $y_2$ describing a segment that connects $(x_1,y_1)$ and $(x_2,y_2)$, where $−10^9\le x_1,y_1,x_2,y_2\le 10^9$. 
> 
> It's guaranteed that the two endpoints of each segment do not coincide, any two given segments do not intersect with each other in each test case, and no more than $20$ test cases satisfy $n>1000$. 
 

## Output
> For each test case, output a line containing a single real number for the answer to the closest pair of segments problem with an absolute or relative error of at most $10^{−6}$. 
> 
> Precisely speaking, assume that your answer is $a$ and and the jury's answer is $b$, your answer will be considered correct if and only if $\frac{|a−b|}{max\{1,|b|\}}\le 10^{−6}$. 
 

## Sample Input

```markdown
2
2
0 1 1 2
1 1 2 0
2
0 1 1 2
2 2 3 1
```

## Sample Output

```markdown
0.707106781187
1.000000000000
```

## Solution

### 题意

类似于计算几何中的最近点对问题，本题求的是最近线段对。

给定 $n$ 条线段，求出最近线段对之间的距离。

### 题解

**暴力 剪枝**

比赛时我用了三角剖分，结果超时了。

赛后补题时看到了这篇博客：[HDU 6697 Closest Pair of Segments（线段距离）](https://www.cnblogs.com/dd-bond/p/11391771.html)

原来暴力加上剪枝就能过。思路是这样的：

首先将线段的左侧端点按照横坐标为第一关键字，纵坐标为第二关键字排序。然后暴力找所有线段对，维护最小值 $ans$。如果当前查询的线段对中，右侧线段的左端点与左侧线段的右端点的横坐标差值大于 $ans$ 时，就不用再找更右侧的直线了。这样剪枝能大大减少时间复杂度。

时限给了 20s，大概 1.3s 就能跑完。

~~题解看不懂~~

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
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

struct Point {
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
    db dot(const Point a) {
        return x * a.x + y * a.y;
    }
    db cross(const Point a) {
        return x * a.y - y * a.x;
    }

};
typedef Point Vector;

struct Line {
    Point s, e;
    Line() {}
    Line(Point s, Point e) : s(s), e(e) {}
    void input() {
        s.input();
        e.input();
    }
    db length() {
        return s.dis(e);
    }

    // 点到直线的距离
    db point_to_line(Point p) {
        return fabs((p - s).cross(e - s) / length());
    }

    // 点到线段的距离
    db point_to_seg(Point p) {
		if(dcmp((p - s).dot((e - s))) < 0 || dcmp((p - e).dot((s - e))) < 0) {
			return min(p.dis(s), p.dis(e));
		}
		return point_to_line(p);
    }

    // 线段到线段的距离
    db seg_to_seg(Line l) {
		return min(min(point_to_seg(l.s), point_to_seg(l.e)), min(l.point_to_seg(s), l.point_to_seg(e)));
    }
};

Line l[maxn];

int cmp(Line l1, Line l2) {
	return l1.s < l2.s;
}

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        int n;
        scanf("%d", &n);
        for(int i = 0; i < n; ++i) {
            l[i].input();
			if(l[i].e < l[i].s) swap(l[i].s, l[i].e);
        }
		sort(l, l + n, cmp);
		double ans = 1e10;
		for(int i = 0; i < n; ++i) {
			for(int j = i + 1; j < n; ++j) {
                // 剪枝部分
				if(dcmp((l[j].s.x - l[i].e.x) - ans) > 0) {
					break;
				}
				ans = min(ans, l[i].seg_to_seg(l[j])); // 更新最小值
			}
		}
		printf("%.12lf\n", ans);
    }
    return 0;
}
```
