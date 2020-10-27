---
title: HDU 1847 Good Luck in CET-4 Everybody!  (巴什博弈)
date: 2019-10-07 22:36:36
tags:
- ACM刷题
- 巴什博弈
- 博弈论
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1847](http://acm.hdu.edu.cn/showproblem.php?pid=1847)

## Problem Description

> 大学英语四级考试就要来临了，你是不是在紧张的复习？也许紧张得连短学期的ACM都没工夫练习了，反正我知道的Kiki和Cici都是如此。当然，作为在考场浸润了十几载的当代大学生，Kiki和Cici更懂得考前的放松，所谓“张弛有道”就是这个意思。这不，Kiki和Cici在每天晚上休息之前都要玩一会儿扑克牌以放松神经。
> 
> “升级”？“双扣”？“红五”？还是“斗地主”？
> 
> 当然都不是！那多俗啊~
> 
> 作为计算机学院的学生，Kiki和Cici打牌的时候可没忘记专业，她们打牌的规则是这样的：
> 
> 1、  总共n张牌;
> 
> 2、  双方轮流抓牌；
> 
> 3、  每人每次抓牌的个数只能是2的幂次（即：1，2，4，8，16…）
> 
> 4、  抓完牌，胜负结果也出来了：最后抓完牌的人为胜者；
> 
> 假设Kiki和Cici都是足够聪明（其实不用假设，哪有不聪明的学生~），并且每次都是Kiki先抓牌，请问谁能赢呢？
> 
> 当然，打牌无论谁赢都问题不大，重要的是马上到来的CET-4能有好的状态。
> 
> Good luck in CET-4 everybody!

<!--more-->

## Input

> 输入数据包含多个测试用例，每个测试用例占一行，包含一个整数n（1<=n<=1000）。
 

## Output

> 如果Kiki能赢的话，请输出“Kiki”，否则请输出“Cici”，每个实例的输出占一行。

## Sample Input

```markdown
1
3
```

## Sample Output

```markdown
Kiki
Cici
```

## Author

> lcy
 

## Source

> [ACM Short Term Exam_2007/12/13](http://acm.hdu.edu.cn/search.php?field=problem&key=ACM+Short+Term+Exam_2007%2F12%2F13&source=1&searchmode=source)

## Solution

### 题意

有 $n$ 张牌，两个人轮流抓牌，每次可以取 $2^i$ 张，最后取完的人获胜，求获胜者。

### 思路

所有的数要么是 $3$ 的倍数，要么是 $3$ 的倍数余 $1$，要么是 $3$ 的倍数余 $2$。

如果轮到对手时且只剩下 $3$ 张牌，那么对手只能取 $1$ 张或 $2$ 张，对手必败。

如果轮到对手时且只剩下 $3i$ 张牌，那么不管对手取几张，剩下的牌数为 $3j + 1$ 或 $3j + 2$，然后你只要取走余数，又构造一个 $3$ 的倍数。

所以牌数为 $3$ 的倍数时先手必败，否则先手必胜。

## Code

```cpp
#include <iostream>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    while(cin >> n) {
        if(n % 3) {
            cout << "Kiki" << endl;
        } else {
            cout << "Cici" << endl;
        }
    }
}
```
