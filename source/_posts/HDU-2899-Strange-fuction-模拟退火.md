---
title: HDU 2899 Strange fuction (模拟退火)
date: 2019-10-31 23:20:34
tags:
- ACM刷题
- 模拟退火
- 数值计算
- 随机化算法
categories: 竞赛
mathjax: true
---

题目链接：[HDU 2899](http://acm.hdu.edu.cn/showproblem.php?pid=2899)

## Problem Description

> Now, here is a fuction:
> 
>  F(x) = 6x^7+8x^6+7x^3+5x^2-yx (0 <= x <=100)
> 
> Can you find the minimum value when x is between 0 and 100.

<!--more-->

## Input

> The first line of the input contains an integer T(1<=T<=100) which means the number of test cases. Then T lines follow, each line has only one real numbers Y.(0 < Y <1e10)

## Output

> Just the minimum value (accurate up to 4 decimal places),when x is between 0 and 100.


## Sample Input

```markdown
2
100
200
```

## Sample Output

```markdown
-74.4291
-178.8534
```

## Solution

### 题意

给定 $y$，求函数 $F(x) = 6x^7 + 8x^6 + 7x^3 + 5x^2 - yx$ 的最小值，其中 $x$ 的范围是 $[0, 100]$。

### 思路

**模拟退火**

模拟退火算法是一种随机化算法，在数学建模中比较常见，在 ACM 中不太常用。主要用于求解函数 (不是单峰函数的时候) 的最值，在最小圆/球覆盖中也有应用。

模拟退火算法基于物理退火的原理，将固体加热至高温然后冷却，温度越高降温的概率越大 (降温更快)，温度越低降温的概率越小 (降温越慢)。模拟退火算法进行多次降温，直到找到一个可行解。

简单来说，如果新的状态比当前状态更优就接受该状态，否则以一定概率接受新状态。概率为：$P(\Delta E) = e^{\frac{-\Delta E}{T}}$，其中 $T$ 为当前温度，$\Delta E$ 新状态与当前状态的能量差。

模拟退火主要有三个参数：初始温度 $T_0$，降温系数 $d$，终止温度 $T_k$。

让当前温度 $T = T_0$，温度下降，尝试转移，如果转移 $T = d * T$。当 $T < T_k$ 时结束模拟退火算法。

伪代码如下：

```
T0 = 100000;  // 初始温度为高温，设置成一个大数
Tk = 1e-8;  // 终止温度为低温，设置为一个接近于0的数
d = 0.98;  // d是一个小于1但是非常接近于1的数
f(x);  // 评价函数，比如物理意义上的温度
T = T0;  // 当前温度
now, nxt;  // 当前状态与新状态
while(T > Tk) {
    dE = f(now) - f(nxt);  // 能量差
    if(dE >= 0) {  // 新状态更优就接受
        now = nxt;
    } else if(exp(-dE/T) > rand()) {  // 否则以一定概率接受
        now = nxt;
    }
    T *= d;  // 降温
}
```

模拟退火算法的缺点主要是精度不高，求得的是近似最优解而不是最优解。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef double db;
const db Tk = 1e-8;
const db T0 = 100000;
const db d = 0.98;

// 评估函数
db func(db x, db y) {
    return 6 * pow(x, 7.0) + 8 * pow(x, 6.0) + 7 * pow(x, 3.0) + 5 * pow(x, 2.0) - y * x;
}

// 模拟退火
db simulateAnneal(db y) {
    db T = T0;
    db x = 50.0;  // x的初始值
    db now = func(x, y);  // 当前状态
    db nxt;
    db ans = now;  // 最优解
    while(T > Tk) {
        int f[2] = {-1, 1};
        db newx = x + f[rand() % 2] * T;  // 按概率改变x，温度越低概率越小
        if(newx >= 0 && newx <= 100) {
            nxt = func(newx, y);  // 新状态
            ans = min(ans, nxt);  // 在退火过程中维护遇到的所有解的最优值
            db dE = now - nxt;  // 能量差
            if(dE >= 0) {
                now = nxt;
                x = newx;
            } else if(exp(dE / T) > rand()) {
                now = nxt;
                x = newx;
            }
        }
        T *= d;
    }
    return ans;
}

int main() {
    db y;
    int T;
    scanf("%d", &T);
    while(T--) {
        scanf("%lf", &y);
        printf("%.4lf\n", simulateAnneal(y));
    }
    return 0;
}
```

## Reference

> [OI-Wiki 模拟退火](https://oi-wiki.org/misc/simulated-annealing/)
> 
> 《算法竞赛 入门到进阶》罗勇军 郭卫斌 著