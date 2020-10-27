---
title: HDU 6590 Code (判断凸包相交)
date: 2019-08-21 22:32:16
tags:
- ACM刷题
- 计算几何
- 凸包
- 模板题
categories: 竞赛
mathjax: true
---

2019 杭电多校 1 1013

题目链接：[HDU 6590](http://acm.hdu.edu.cn/showproblem.php?pid=6590)

比赛链接：[2019 Multi-University Training Contest 1](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+1&source=1&searchmode=source)

## Problem Description
> After returning with honour from ICPC(International Cat Programming Contest) World Finals, Tom decides to say goodbye to ICPC and start a new period of life. He quickly gets interested in AI.
> 
> In the subject of Machine Learning, there is a classical classification model called perceptron, defined as follows:
> 
> 
> Assuming we get a set of training samples: $D={(\boldsymbol{x_1},y_1),(\boldsymbol{x_2},y_2),...,(\boldsymbol{x_N},y_N)}$, with their inputs $\boldsymbol{x}\in \mathbb{R}^d$, and outputs $y\in \{−1,1\}$. We will try to find a function $f(\boldsymbol{x})=sign(\sum_{i=1}^d w_i\cdot x_i+b)=sign(\boldsymbol{w^T} \cdot \boldsymbol{x}+b)$ so that $f(\boldsymbol{x_i})=y_i,i=1,2,...,N$.
> 
> $\boldsymbol{w}, \boldsymbol{x}$ mentioned above are all d-dimensional vectors, i.e. $\boldsymbol{w}=(w_1,w_2,...,w_d), \boldsymbol{x}=(x_1,x_2,...,x_d)$. To simplify the question, let $w_0=b$, $x_0=1$, then $f(\boldsymbol{x})=sign(\sum_{i = 0}^d w_i\cdot x_i)=sign(\boldsymbol{w^T}\cdot \boldsymbol{x})$. Therefore, finding a satisfying function $f(\boldsymbol{x})$ is equivalent to finding a proper $\boldsymbol{w}$.
> 
> To solve the problem, we have a algorithm, PLA(Popcorn Label Algorithm).
> 
> Accoding to *PLA*, we will randomly generate $\boldsymbol{w}$.
> 
> If $f(\boldsymbol{x})=sign(\boldsymbol{w^T}\cdot \boldsymbol{x})$ fails to give any element $(\boldsymbol{x_i},y_i)\in D$ the right classification, i.e. $f(\boldsymbol{x_i})\neq y_i$, then we will replace $w$ with another random vector. We will do this repeatedly until all the samples $\in D$ are correctly classified.
> 
> As a former-JBer, Tom excels in programming and quickly wrote the pseudocode of *PLA*.
> 
> 
>       w := a random vector
>       while true do
>           flag:=true
>           for i:=1 to N do
>               if f(x[ i ]) != y[ i ] then
>                   flag:=false
>                   break
>           if flag then
>               break
>           else
>               w := a random vector
>       return w
> 
> 
> But Tom found that, in some occasions, *PLA* will end up into an infinite loop, which confuses him a lot. You are required to help Tom determine, when performed on a given sample set $D$, if *PLA* will end up into an infinite loop. Print Infinite loop! if so, or Successful! otherwise.
> 
> We only consider cases when $d=2$ for simplification.
> 
> Note: 
> $$sign(x)= \begin{cases} -1& x < 0 \\ 0& x = 0 \\ 1&  x > 0 \end{cases}$$
 
<!--more-->

## Input
> The first line contains an integer $T(1\le T\le 1000)$, the number of test cases.
> 
> Each test case begins with a line containing a single integer $n(1\le n\le 100)$, size of the set of training samples $D$.
> 
> Then $n$ lines follow, the ith of which contains three integers $x_{i,1},x_{i,2},y_i (−10^5\le x_{i,1},x_{i,2}\le 10^5, y_i\in {−1,1})$, indicating the ith sample $(x_i,y_i)$ in $D$, where $x_i=(x_{i,1},x_{i,2})$.
 

## Output
> For each test case, output a single line containing the answer: “Infinite loop!” or “Successful!”.
 

## Sample Input

```markdown
3
2
1 1 1
2 0 -1
4
0 0 1
2 0 -1
1 1 1
1 -1 -1
6
0 0 1
2 0 -1
1 1 1
1 -1 -1
1 0 1
0 1 -1
```

## Sample Output

```markdown
Successful!
Successful!
Infinite loop!
```

## Solution

### 题意

给出两类点的坐标，问能否用一条直线将两类点分开。

### 题解

题目看懂了就很好做了。

就是分别对两类点求凸包，然后判断两个凸包是否相交。若不相交，则能够用一条直线分开两类点，否则不能。

其实就是判断凸包是否相交的模板题。

类似的题目有：
- [UVA 10256 The Great Divide](https://wutao18.github.io/2019/08/21/UVA-10256-The-Great-Divide-%E5%88%A4%E6%96%AD%E5%87%B8%E5%8C%85%E7%9B%B8%E4%BA%A4/)
- [POJ 3805 Separate Points](https://wutao18.github.io/2019/08/21/POJ-3805-Separate-Points-%E5%88%A4%E6%96%AD%E5%87%B8%E5%8C%85%E7%9B%B8%E4%BA%A4/#more)

## Code

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
            printf("Infinite loop!\n");
            return;
        }
    }
    for(int i = 0; i < c2; ++i) {
        if(isPointInPolygon(s2[i], s1)) {
            printf("Infinite loop!\n");
            return;
        }
    }
    for (int i = 0; i < c1; i++) {
        for (int j = 0; j < c2; j++) {
            if (Intersection(s1[i], s1[(i + 1) % c1], s2[j], s2[(j + 1) % c2])) {
                printf("Infinite loop!\n");
                return;
            }
        }
    }
    printf("Successful!\n");
}

int main() {
    int T;
    cin >> T;
    while (T--) {
        int n;
        scanf("%d", &n);
        Polygon s1, s2;
        for (int i = 0; i < n; ++i) {
            double x1, x2, y;
            scanf("%lf%lf%lf", &x1, &x2, &y);
            if(y == 1) {
                s1.push_back(Point(x1, x2));
            } else {
                s2.push_back(Point(x1, x2));
            }
        }
        if(n == 1) {
            printf("Successful!\n");
            continue;
        }
        if(s1.size()) s1 = Andrew(s1);
        if(s2.size()) s2 = Andrew(s2);
        solve(s1, s2);
    }
    return 0;
}
```
