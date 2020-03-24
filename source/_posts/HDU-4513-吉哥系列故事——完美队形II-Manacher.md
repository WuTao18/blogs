---
title: HDU 4513 吉哥系列故事——完美队形II (Manacher)
date: 2019-11-15 23:05:08
tags:
- ACM刷题
- Manacher
categories: 竞赛
mathjax: true
---

题目链接：[HDU 4513](http://acm.hdu.edu.cn/showproblem.php?pid=4513)

## Problem Description

> 吉哥又想出了一个新的完美队形游戏！
> 
> 假设有n个人按顺序站在他的面前，他们的身高分别是h[1], h[2] ... h[n]，吉哥希望从中挑出一些人，让这些人形成一个新的队形，新的队形若满足以下三点要求，则就是新的完美队形：
> 
> 1、挑出的人保持原队形的相对顺序不变，且必须都是在原队形中连续的；
> 
> 2、左右对称，假设有m个人形成新的队形，则第1个人和第m个人身高相同，第2个人和第m-1个人身高相同，依此类推，当然如果m是奇数，中间那个人可以任意；
> 
> 3、从左到中间那个人，身高需保证不下降，如果用H表示新队形的高度，则H[1] <= H[2] <= H[3] .... <= H[mid]。
> 
> 现在吉哥想知道：最多能选出多少人组成新的完美队形呢？

## Input

> 输入数据第一行包含一个整数T，表示总共有T组测试数据(T <= 20)；
> 
> 每组数据首先是一个整数n(1 <= n <= 100000)，表示原先队形的人数，接下来一行输入n个整数，表示原队形从左到右站的人的身高（50 <= h <= 250，不排除特别矮小和高大的）。

## Output

> 请输出能组成完美队形的最多人数，每组输出占一行。

## Sample Input

```markdown
2
3
51 52 51
4
51 52 52 51
```

## Sample Output

```markdown
3
4
```

## Source

> [2013腾讯编程马拉松初赛第二场（3月22日）](http://acm.hdu.edu.cn/search.php?field=problem&key=2013%CC%DA%D1%B6%B1%E0%B3%CC%C2%ED%C0%AD%CB%C9%B3%F5%C8%FC%B5%DA%B6%FE%B3%A1%A3%A83%D4%C222%C8%D5%A3%A9&source=1&searchmode=source)

## Solution

### 题意

如题。

### 思路

在 Manacher 求最长回文子串时增加一个判断条件: h[i - len[i]] <= h[i - len[i] + 2]

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 10;
int n;
int h[maxn << 1], len[maxn << 1];

int Manacher() {
    int mx = 0, id = 0;
    int ans = 1;
    for(int i = 1; i < (n * 2 + 1); ++i) {
        if(mx > i) {
            len[i] = min(mx - i, len[2 * id - i]);
        } else {
            len[i] = 1;
        }
        while(h[i - len[i]] == h[i + len[i]] && h[i - len[i]] <= h[i - len[i] + 2]) {  // 这里要保持递增
            ++len[i];
        }
        if(i + len[i] > mx) {
            mx = i + len[i];
            id = i;
        }
        ans = max(ans, len[i]);
    }
    return ans - 1;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    while(T--) {
        cin >> n;
        h[0] = -1;
        for(int i = 1; i <= n; ++i) {
            h[i * 2 - 1] = 0;
            cin >> h[i * 2];
        }
        h[n * 2 + 1] = 0;
        cout << Manacher() << endl;
    }  
    return 0;
}
```