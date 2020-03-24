---
title: 牛客 Fruit Ninja 2018 ACM 上海大都会赛 (随机化算法)
date: 2019-09-30 23:21:37
tags:
- ACM刷题
- 随机化算法
- 计算几何
categories: 竞赛
mathjax: true
---

题目链接：[Fruit Ninja](https://ac.nowcoder.com/acm/problem/17376)
比赛链接：[2018 ACM 国际大学生程序设计竞赛上海大都会赛重现赛](https://ac.nowcoder.com/acm/contest/163#question)


## 题目描述

> Fruit Ninja is a juicy action game enjoyed by millions of players around the world, with squishy,
> 
> splat and satisfying fruit carnage! Become the ultimate bringer of sweet, tasty destruction with every slash.
> 
> Fruit Ninja is a very popular game on cell phones where people can enjoy cutting the fruit by touching the screen.
> 
> In this problem, the screen is rectangular, and all the fruits can be considered as a point. A touch is a straight line cutting
> 
> thought the whole screen, all the fruits in the line will be cut.
> 
> A touch is EXCELLENT if $\frac{M}{N} \ge x$, ($N$ is total number of fruits in the screen, $M$ is the number of fruits that cut by the touch, $x$ is a real number.)
Now you are given $N$ fruits position in the screen, you want to know if exist a EXCELLENT touch.

## 输入描述:
The first line of the input is $T(1 \le T \le 100)$, which stands for the number of test cases you need to solve.
The first line of each case contains an integer $N (1 \le N \le 10^4)$ and a real number $x (0 < x < 1)$, as mentioned above.
The real number will have only 1 digit after the decimal point.
The next $N$ lines, each lines contains two integers $x_i$ and $y_i (-10^9 ≤ x_i,y_i ≤ 10^9)$, denotes the coordinates of a fruit.

## 输出描述:
For each test case, output "Yes" if there are at least one EXCELLENT touch. Otherwise, output "No".

## 示例1

> 输入
> 
>       2
>       5 0.6
>       -1 -1
>       20 1
>       1 20
>       5 5
>       9 9
>       5 0.5
>       -1 -1
>       20 1
>       1 20
>       2 5
>       9 9
> 
> 输出
> 
>       Yes
>       No

## 题意

给定 $N$ 个点以及一个实数 $x$，问是否存在一条直线，直线经过 $N$ 个点中的 $M$ 个点，且 $\frac{M}{N} \ge x$。

## 思路

**随机化算法**

随机取两个点构造直线，比较直线经过的点的个数（斜率相同）是否大于等于 $N * x$。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef double db;
const int maxn = 1e4 + 10;

class Point {
public:
    db x, y;
    Point(db x = 0, db y = 0) : x(x), y(y) {}
    void input() {
        scanf("%lf%lf", &x, &y);
    }
};
Point p[maxn];

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        int n;
        db x;
        scanf("%d%lf", &n, &x);
        x *= n;
        for(int i = 0; i < n; ++i) {
            p[i].input();
        }
        int flag = 0;
        for(int i = 0; i < 1000; ++i) {
            int a = rand() % n, b = rand() % n;
            while(a == b) a = rand() % n;
            db cnt = 2;
            for(int j = 0; j < n; ++j) {
                if(j == a || j == b) continue;
                if((p[j].y - p[a].y) * (p[b].x - p[j].x) == (p[j].x - p[a].x) * (p[b].y - p[j].y)) {
                    ++cnt;
                }
            }
            if(cnt >= x) {
                flag = 1;
                break;
            }
        }
        if(flag) printf("Yes\n");
        else printf("No\n");
    }
    return 0;
}
```