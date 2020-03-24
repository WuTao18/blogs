---
title: HDU 2553 N皇后问题 (DFS)
date: 2019-08-14 20:21:40
tags:
- ACM刷题
- DFS
categories: 竞赛
mathjax: true
---

题目链接：[HDU 2553](http://acm.hdu.edu.cn/showproblem.php?pid=2553)

## Problem Description
> 在 $N*N$ 的方格棋盘放置了 $N$ 个皇后，使得它们不相互攻击（即任意 $2$ 个皇后不允许处在同一排，同一列，也不允许处在与棋盘边框成 $45$ 角的斜线上。 
> 
> 你的任务是，对于给定的 $N$，求出有多少种合法的放置方法。 


## Input
> 共有若干行，每行一个正整数 $N\le 10$，表示棋盘和皇后的数量；如果 $N=0$，表示结束。


## Output
> 共有若干行，每行一个正整数，表示对应输入行的皇后的不同放置数量。
 

## Sample Input
```markdown
1
8
5
0
```

## Sample Output
```markdown
1
92
10
```

## Solution

**DFS**

每行放置一个，位置用 $pos[i]$ 表示，这样就不用处理行冲突的问题；保证每行的 $pos$ 不同，就可以解决列冲突的问题；最后要保证不在同一斜线上。

注意此题要打表。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int cnt = 0, n;
int pos[110];

// k表示第 k行 也表示放第 k个皇后
void nqueen(int k) {
    if(k == n) {
        ++cnt;
        return;
    }

    for(int i = 0; i < n; ++i) {
        int j;
        // 判断当前位置是否和之前的所有棋子有冲突
        for(j = 0; j < k; ++j) {
            if(pos[j] == i || abs(pos[j] - i) == abs(j - k)) {
                break;
            }
        }
        // for循环跑完表示没有冲突
        if(j == k) {
            pos[k] = i;  // 记录合法的位置
            nqueen(k + 1);  // 搜索下一行
        }
    }
}

int main() {
    int a[11] = {0};  // 打表
    for(n = 1; n <= 10; ++n) {
        cnt = 0;
        nqueen(0);
        a[n] = cnt;
    }
    while(~scanf("%d",&n) && n) {
        printf("%d\n", a[n]);
    }
    return 0;
}

```