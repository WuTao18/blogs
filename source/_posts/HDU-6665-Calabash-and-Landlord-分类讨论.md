---
title: HDU 6665 Calabash and Landlord (分类讨论)
date: 2019-08-14 22:27:47
tags:
- ACM刷题
- 计算几何
- 分类讨论
categories: 竞赛
mathjax: true
---

2019 杭电多校 8 1009

题目链接：[HDU 6665](http://acm.hdu.edu.cn/showproblem.php?pid=6665)

比赛链接：[2019 Multi-University Training Contest 8](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+8&source=1&searchmode=source)


## Problem Description
> Calabash is the servant of a landlord. The landlord owns a piece of land, which can be regarded as an infinite 2D plane. 
> 
> One day the landlord set up two orthogonal rectangular-shaped fences on his land. He asked Calabash a simple problem: how many nonempty connected components is my land divided into by these two fences, both finite and infinite? Calabash couldn't answer this simple question. Please help him! 
> 
> Recall that a connected component is a maximal set of points not occupied by the fences, and every two points in the set are reachable without crossing the fence.
 

## Input
> The first line of input consists of a single integer $T (1\le T\le 10000)$, the number of test cases. 
> 
> Each test case contains two lines, specifying the two rectangles. Each line contains four integers $x_1,y_1,x_2,y_2 (0\le x_1,y_1,x_2,y_2\le 10^9,x_1<x_2,y_1<y_2)$, where $(x_1,y_1),(x_2,y_2)$ are the Cartesian coordinates of two opposite vertices of the rectangular fence. The edges of the rectangles are parallel to the coordinate axes. The edges of the two rectangles may intersect, overlap, or even coincide.
 

## Output
> For each test case, print the answer as an integer in one line.

## Sample Input
```markdown
3
0 0 1 1
2 2 3 4
1 0 3 2
0 1 2 3
0 0 1 1
0 0 1 1
```

## Sample Output
```markdown
3
4
2
```

## Solution

### 题意：

给出两个矩形，求矩形把平面分割成几块。

### 思路

**分类讨论**

![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/08/14/1565792338325-1565792338743.png)

听说只要离散化到 $5*5$ 的格子里然后 $DFS$ 就可以了，有空再补。

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

const ll inf = 0x3f3f3f3f;

struct Point
{
    ll x, y;
}p[10];


int main() {
    int T;
    cin >> T;
    while (T--) {
        ll maxx = 0, maxy = 0, minx = inf, miny = inf;
        for(int i = 1; i <= 2; ++i) {
            scanf("%lld%lld", &p[i].x, &p[i].y);
        }
        p[3].x = p[1].x;
        p[3].y = p[2].y;
        p[4].x = p[2].x;
        p[4].y = p[1].y;
        ll s1 = (p[2].x - p[1].x) * (p[2].y - p[1].y);

        for(int i = 5; i <= 6; ++i) {
            scanf("%lld%lld", &p[i].x, &p[i].y);
        }
        p[7].x = p[5].x;
        p[7].y = p[6].y;
        p[8].x = p[6].x;
        p[8].y = p[5].y;
        ll s2 = (p[6].x - p[5].x) * (p[6].y - p[5].y);

        for(int i = 1; i <= 8; ++i) {
            maxx = max(maxx, p[i].x);
            maxy = max(maxy, p[i].y);
            minx = min(minx, p[i].x);
            miny = min(miny, p[i].y);
        }

        // for(int i = 1; i <= 8; ++i) {
        //     cout << p[i].x << " " << p[i].y << endl;
        // }

        if(p[1].x == p[5].x && p[1].y == p[5].y && p[3].x == p[7].x && p[3].y == p[7].y && p[6].x == p[2].x && p[6].y == p[2].y && p[4].x == p[8].x && p[4].y == p[8].y) {
            printf("2\n");
            continue;
        }
        ll s = (maxx - minx) * (maxy - miny); // cout << s << endl;
        if(s1 == s || s2 == s) {
            if((p[1].x == p[5].x && p[2].x == p[6].x)) {
                if(p[5].y == p[1].y || p[6].y == p[2].y) printf("3\n");
                else printf("4\n");
            } else if((p[1].y == p[5].y && p[2].y == p[6].y)) {
                if(p[5].x == p[1].x || p[6].x == p[2].x) printf("3\n");
                else printf("4\n");
            }
            else printf("3\n");
        } else if(p[4].y >= p[7].y || p[8].y >= p[3].y || p[5].x >= p[2].x || p[1].x >= p[6].x) {
            printf("3\n");
        } else if((p[2].x - p[1].x) * (p[6].y - p[5].y) == s) {
            if(p[6].y > p[2].y && p[5].y < p[1].y && p[1].x < p[5].x && p[2].x > p[6].x) {
                printf("6\n");
            } else if(p[1].x == p[5].x && p[1].y == p[5].y) {
                printf("4\n");
            } else if(p[3].x == p[7].x && p[3].y == p[7].y) {
                printf("4\n");
            } else if(p[2].x == p[6].x && p[2].y == p[6].y) {
                printf("4\n");
            } else if(p[4].x == p[8].x && p[4].y == p[8].y) {
                printf("4\n");
            } else {
                printf("5\n");
            }
        } else if((p[6].x - p[5].x) * (p[2].y - p[1].y) == s) {
            if(p[2].y > p[6].y && p[1].y < p[5].y && p[5].x < p[1].x && p[6].x > p[2].x) {
                printf("6\n");
            } else if(p[1].x == p[5].x && p[1].y == p[5].y) {
                printf("4\n");
            } else if(p[3].x == p[7].x && p[3].y == p[7].y) {
                // cout << 1 << endl;
                printf("4\n");
            } else if(p[2].x == p[6].x && p[2].y == p[6].y) {
                printf("4\n");
            } else if(p[4].x == p[8].x && p[4].y == p[8].y) {
                printf("4\n");
            } else {
                printf("5\n");
            }
        } else {
            printf("4\n");
        }
    }
    
    return 0;
}
```
