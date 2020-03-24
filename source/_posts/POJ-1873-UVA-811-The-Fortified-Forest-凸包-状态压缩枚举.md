---
title: POJ 1873 UVA 811 The Fortified Forest (凸包 + 状态压缩枚举)
date: 2019-08-18 23:20:29
tags:
- ACM刷题
- 计算几何
- 状态压缩
- 枚举
- 凸包
categories: 竞赛
mathjax: true
---

题目链接：[UVA 811](https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&problem=752)

## Description

> Once upon a time, in a faraway land, there lived a king. This king owned a small collection of rare and valuable trees, which had been gathered by his ancestors on their travels. To protect his trees from thieves, the king ordered that a high fence be built around them. His wizard was put in charge of the operation. 
> 
> Alas, the wizard quickly noticed that the only suitable material available to build the fence was the wood from the trees themselves. In other words, it was necessary to cut down some trees in order to build a fence around the remaining trees. Of course, to prevent his head from being chopped off, the wizard wanted to minimize the value of the trees that had to be cut. The wizard went to his tower and stayed there until he had found the best possible solution to the problem. The fence was then built and everyone lived happily ever after. 
> 
> You are to write a program that solves the problem the wizard faced. 

## Input

> The input contains several test cases, each of which describes a hypothetical forest. Each test case begins with a line containing a single integer $n$, $2\le n\le 15$, the number of trees in the forest. The trees are identied by consecutive integers $1$ to $n$. Each of the subsequent lines contains $4$ integers $x_i,y_i,v_i,l_i$ that describe a single tree. $(x_i,y_i)$ is the position of the tree in the plane, $v_i$ is its value, and $l_i$ is the length of fence that can be built using the wood of the tree. $vi$ and $li$ are between $0$ and $10,000$.
> 
> The input ends with an empty test case (n= 0).

## Output

> For each test case, compute a subset of the trees such that, using the wood from that subset, the remaining trees can be enclosed in a single fence. Find the subset with minimum value. If more than one such minimum-value subset exists, choose one with the smallest number of trees. For simplicity, regard the trees as having zero diameter. 
> 
> Display, as shown below, the test case numbers (1, 2, ...), the identity of each tree to be cut, and the length of the excess fencing (accurate to two fractional digits). 
> 
> Display a blank line between test cases. 

## Sample Input

```markdown
6
 0  0  8  3
 1  4  3  2
 2  1  7  1
 4  1  2  3
 3  5  4  6
 2  3  9  8
3
 3  0 10  2
 5  5 20 25
 7 -3 30 32
0
```

## Sample Output

```markdown
Forest 1
Cut these trees: 2 4 5 
Extra wood: 3.16

Forest 2
Cut these trees: 2 
Extra wood: 15.00
```

## Solution

### 题意

有 $n$ 颗树，每颗树的坐标为 $x, y$ ，价值为 $v_i$ 长度为 $l_i$。现在要用篱笆将其中一些树围起来，但篱笆制作来源于这些树，即要求砍掉的树能构成篱笆的长度 $>=$ 剩余树的凸包周长。现在要使得砍掉树的价值之和最小，问需要砍掉哪些树(如果有价值相同的解，就输出砍的树最少的解)。

### 题解

**凸包周长 状态压缩枚举**

树的规模比较小，用二进制枚举所有情况即可。

## Code

```cpp
#include <iostream>
#include <cstdio>
#include <vector>
#include <algorithm>
#include <cmath>
using namespace std;
const double eps = 1e-10;
const int inf = 0x3f3f3f3f;
const int maxn = 30;

int n;
struct Point {
    double x, y;
    int v, l;
    int id;
    Point() {}
    Point(double a, double b) : x(a), y(b) {}
    bool operator<(const Point &b) const {
        if (x < b.x) return 1;
        if (x > b.x) return 0;
        return y < b.y;
    }
    bool operator==(const Point &b) const {
        if (x == b.x && y == b.y) return 1;
        return 0;
    }
    Point operator-(const Point &b) {
        return Point(x - b.x, y - b.y);
    }
} p[maxn], stk[maxn], tmp[maxn];
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

int Andrew(int l) {
    sort(tmp + 1, tmp + 1 + l);
    int len = 0;
    for (int i = 1; i <= l; ++i) {
        if(i > 1 && tmp[i] == tmp[i - 1]) continue; // 会有重复的点, WA了好几次
        while (len > 1 && sgn(cross(stk[len] - stk[len - 1], tmp[i] - stk[len - 1])) == -1) {
            len--;
        }
        stk[++len] = tmp[i];
    }
    int k = len;
    for (int i = l - 1; i >= 1; --i) {
        if(i > 1 && tmp[i] == tmp[i - 1]) continue;
        while (len > k && sgn(cross(stk[len] - stk[len - 1], tmp[i] - stk[len - 1])) == -1) {
            len--;
        }
        stk[++len] = tmp[i];
    }
    return len;
}

void solve(int &min_val, int &cur_num, double &re_len, int &ans) {
    int size = 1 << n;
    for(int bit = 0; bit < size; ++bit) {
        int t = 0, cur_val = 0;
        double cur_len = 0 ;
        for(int i = 0; i < n; ++i) {
            if(bit & (1 << i)) {
                cur_len += p[i].l;
                cur_val += p[i].v;
            } else {
                tmp[++t] = p[i];
            }
        }
        if(cur_val > min_val) continue;
        int cnt = Andrew(t);
        double c = 0;
        for(int i = 1; i < cnt; ++i) {
            c += dist(stk[i], stk[i + 1]);
        }

        if(cur_len >= c) {
            if(cur_val < min_val || (cur_val == min_val &&  n - t < cur_num)) {
                min_val = cur_val;
                cur_num = n - t;
                re_len = cur_len - c;
                ans = bit;
            }
        }
    }
}
int main() {
    int kase = 0;
    while(scanf("%d",&n) && n) {
        if(kase) printf("\n");
        for(int i = 0; i < n; ++i) {
            scanf("%lf%lf%d%d", &p[i].x, &p[i].y, &p[i].v, &p[i].l);
        }
        int min_val = inf;
        int cur_num = inf;
        double re_len = 0;
        int ans = 0;
        solve(min_val, cur_num, re_len, ans);
        printf("Forest %d\n", ++kase);
        printf("Cut these trees:");
        for(int i = 0 ; i < n; ++i) {
            if(ans & (1 << i)) {
                printf(" %d", i + 1);
            }
        }
        printf("\nExtra wood: %.2lf\n", re_len);
    }
    return 0;
}
```