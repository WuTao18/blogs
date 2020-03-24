---
title: ICPC Asia Nanning 2017 I. Rake It In (DFS+贪心 或 对抗搜索+Alpha-Beta剪枝)
date: 2019-08-15 21:27:40
tags:
- ACM刷题
- DFS
- 贪心
- 对抗搜索
- alpha-beta剪枝
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[Rake It In](https://nanti.jisuanke.com/t/A1538)

比赛链接：[ICPC Asia Nanning 2017](https://www.jisuanke.com/contest/3107?view=challenges)

## Description
> The designers have come up with a new simple game called “Rake It In”. Two players, Alice and Bob, initially select an integer k and initialize a score indicator. An $4 \times 4$ board is created with 16 values placed on the board. Starting with player Alice, each player in a round selects a $2 \times 2$ region of the board, adding the sum of values in the region to the score indicator, and then rotating these four values $90$ degrees counterclockwise.
> 
> After $2k$ rounds in total, each player has made decision in k times. The ultimate goal of Alice is to maximize the final score. However for Bob, his goal is to minimize the final score.
> 
> In order to test how good this game is, you are hired to write a program which can play the game. Specifically, given the starting configuration, they would like a program to determine the final score when both players are entirely rational. 


## Input
> The input contains several test cases and the first line provides an integer $t (1 \le t \le 200)$ which is the number of test cases.
> 
> Each case contains five lines. The first line provides the integer $k (1 \le k \le 3)$. Each of the following four lines contains four integers indicating the values on the board initially. All values are integers between $1$ to $10$.

## Output
> For each case, output an integer in a line which is the predicted final score.
 

## Sample Input
```markdown
4
1
1 1 2 2
1 1 2 2  
3 3 4 4  
3 3 4 4
2
1 2 3 4
1 2 3 4
1 2 3 4
1 2 3 4
3  
1 1 4 4
4 4 1 1
1 1 4 4
1 4 1 4
3  
1 2 3 4
5 1 2 3
4 5 1 2
3 4 5 1
```

## Sample Output
```markdown
20
40
63
71
```

## Solution

### 题意

有一块 $4\times 4$ 的板，Alice 和 Bob 每次选择 $2\times 2$ 的区域并逆时针旋转 $90$ 度，这个区域的和累加到总分上。现在 Alice 先手，有 $k$ 轮游戏，Alice 想要分数最大化，Bob 想要分数最小化，求最终的分数。 

### 题解

**DFS 贪心**

比较好的解法是对抗搜索 与 $Alpha-Beta$ 剪枝。

题解给出是上分支定界和启发式搜索。

但是用贪心 + 爆搜竟然过了。

~~关于对抗搜索和 $Alpha-Beta$ 剪枝以后再更新。~~

## Code

DFS + 贪心

```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf = 1000;

int k;
int mt[10][10];

// 交换两数
void swap(int &a, int &b) {
    a = a ^ b;
    b = a ^ b;
    a = a ^ b;
}

// 逆时针旋转
void rote(int x, int y) {
    swap(mt[x][y], mt[x + 1][y]);
    swap(mt[x][y + 1], mt[x + 1][y + 1]);
    swap(mt[x][y], mt[x + 1][y + 1]);
}

// 顺时针旋转
void rerote(int x, int y) {
    swap(mt[x][y], mt[x + 1][y + 1]);
    swap(mt[x][y + 1], mt[x + 1][y + 1]);
    swap(mt[x][y], mt[x + 1][y]);
}

// 求和
int sum(int x, int y) {
    return mt[x][y] + mt[x + 1][y] + mt[x][y + 1] + mt[x + 1][y + 1];
}

int dfs(int step) {
    if(step == 2 * k) { // 最后一步
        int ans = inf;
        for(int i = 0; i < 3; ++i) {
            for(int j = 0; j < 3; ++j) {
                ans = min(ans, sum(i, j));
            }
        }
        return ans;
    } else {
        // 奇数步选择最大 偶数步选择最小
        int ans = (step & 1)? 0: inf;
        for(int i = 0; i < 3; ++i) {
            for(int j = 0; j < 3; ++j) {
                rote(i, j); // 逆时针旋转
                if(step & 1) {
                    ans = max(ans, sum(i, j) + dfs(step + 1));
                } else {
                    ans = min(ans, sum(i, j) + dfs(step + 1));
                }
                rerote(i, j); // 回溯时转回来
            }
        }
        return ans;
    }
}

int main() {
    int T;
    cin >> T;
    while(T--) {
        scanf("%d", &k);
        for(int i = 0; i < 4; ++i) {
            for(int j = 0; j < 4; ++j) {
                scanf("%d", &mt[i][j]);
            }
        }
        int ans = dfs(1);
        printf("%d\n", ans);
    }
    return 0;
}
```

对抗搜索 + $Alpha-Beta$ 剪枝

```cpp
#include <bits/stdc++.h>
using namespace std;
const int inf = 1000;

int k;
int mt[10][10];

void swap(int &a, int &b) {
    a = a ^ b;
    b = a ^ b;
    a = a ^ b;
}

void rote(int x, int y) {
    swap(mt[x][y], mt[x + 1][y]);
    swap(mt[x][y + 1], mt[x + 1][y + 1]);
    swap(mt[x][y], mt[x + 1][y + 1]);
}

void rerote(int x, int y) {
    swap(mt[x][y], mt[x + 1][y + 1]);
    swap(mt[x][y + 1], mt[x + 1][y + 1]);
    swap(mt[x][y], mt[x + 1][y]);
}

int cnt(int x, int y) {
    return mt[x][y] + mt[x + 1][y] + mt[x][y + 1] + mt[x + 1][y + 1];
}

int dfs(int sum, int step, int alpha, int beta) {
    if(step == 2 * k + 1) {
        return sum;
    } else {
        for(int i = 0; i < 3; ++i) {
            for(int j = 0; j < 3; ++j) {
                rote(i, j);
                if(step & 1) {
                    alpha = max(alpha, dfs(sum + cnt(i, j), step + 1, alpha, beta));
                } else {
                    beta = min(beta, dfs(sum + cnt(i, j), step + 1, alpha, beta));
                }
                rerote(i, j);
                if(beta <= alpha) break;
            }
            if(beta <= alpha) break;
        }
        return (step & 1)? alpha: beta;
    }
}

int main() {
    int T;
    cin >> T;
    while(T--) {
        scanf("%d", &k);
        for(int i = 0; i < 4; ++i) {
            for(int j = 0; j < 4; ++j) {
                scanf("%d", &mt[i][j]);
            }
        }
        int ans = dfs(0, 1, -inf, inf);
        printf("%d\n", ans);
    }
    return 0;
}
```