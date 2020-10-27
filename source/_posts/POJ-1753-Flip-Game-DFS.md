---
title: POJ 1753 Flip Game (DFS)
date: 2019-08-14 20:38:43
tags:
- ACM刷题
- DFS
categories: 竞赛
mathjax: true
---

题目链接：[POJ 1753](http://poj.org/problem?id=1753)

## Problem Description
> Flip game is played on a rectangular 4x4 field with two-sided pieces placed on each of its 16 squares. One side of each piece is white and the other one is black and each piece is lying either it's black or white side up. Each round you flip 3 to 5 pieces, thus changing the color of their upper side from black to white and vice versa. The pieces to be flipped are chosen every round according to the following rules: 
> 
> Choose any one of the 16 pieces. 
> 
> Flip the chosen piece and also all adjacent pieces to the left, to the right, to the top, and to the bottom of the chosen piece (if there are any).
> 
> Consider the following position as an example: 
> 
> bwbw
>  
> wwww 
> 
> bbwb
>  
> bwwb
>  
> Here "b" denotes pieces lying their black side up and "w" denotes pieces lying their white side up. If we choose to flip the 1st piece from the 3rd row (this choice is shown at the picture), then the field will become: 
> 
> bwbw
>  
> bwww
>  
> wwwb
>  
> wwwb 
> 
> The goal of the game is to flip either all pieces white side up or all pieces black side up. You are to write a program that will search for the minimum number of rounds needed to achieve this goal. 
> 
> ![](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/08/14/1565786207845-1565786207847.png)

<!--more-->

## Input
> The input consists of 4 lines with 4 characters "w" or "b" each that denote game field position.


## Output
> Write to the output file a single integer number - the minimum number of rounds needed to achieve the goal of the game from the given position. If the goal is initially achieved, then write 0. If it's impossible to achieve the goal, then write the word "Impossible" (without quotes).
 

## Sample Input
```markdown
bwwb
bbwb
bwwb
bwww
```

## Sample Output
```markdown
4
```

## Solution

### 题意

> 给定 4 * 4 的正方形，每个格子要么是黑色，要么是白色，可以改变任意的格子的颜色，其上下左右的格子也会改变，问至少改变几个格子可以使正方形变为全黑或者全白。

### 思路

**DFS**

1. 每个格子要么不翻，要么翻 1 次。（翻奇数次和翻 1 次效果相同，翻偶数次和不翻效果一样）
2. 可以选择翻转 0 个、1 个、2 个、3 个 ... 16 个
3. 每次翻完后判断是否为纯色，如果为纯色就输出相应的格子个数，如果 16 个翻转了还不是纯色，就输出“impossible”。

## Code

```cpp
#include<bits/stdc++.h>
using namespace std;

bool mp[10][10]; //存储棋子
bool flag; //判断是否成功
int step; //步数

//判断当前状态是否全是白在上或是黑在上
int judge() {
    int i,j;
    for(i = 0; i < 4; ++i) {
        for(j = 0; j < 4; ++j) {
            if(mp[i][j] != mp[0][0]) return 0;
        }
    }
    return 1;
}

//翻转棋子，同时翻转四周相邻的棋子
void flip(int x,int y) {
    mp[x][y] =! mp[x][y];
    if(y <= 2) mp[x][y + 1] =! mp[x][y + 1];
    if(y != 0) mp[x][y - 1] =! mp[x][y - 1];
    if(x <= 2) mp[x + 1][y] =! mp[x + 1][y];
    if(x != 0) mp[x - 1][y] =! mp[x - 1][y];
}

//搜索
void dfs(int x, int y, int dep) {
	// 搜索深度与步数相同时就回溯
    if(step == dep) {
        if(judge()) flag = true;
        return;
    }
    // 如果已经满足条件就可以返回
    if(flag || x == 4) return;

    //翻转棋子
    flip(x, y);
    //进行下一状态的搜索
    if(y < 3) dfs(x, y + 1, dep + 1);
    else dfs(x + 1, 0, dep + 1);

	//将该棋子再翻回，即当前棋子不翻
    flip(x, y);
    //进行下一状态的搜索
    if(y < 3) dfs(x, y + 1, dep);
    else dfs(x + 1, 0, dep);
}

int main() {
    flag = false; //初始化flag为false
    //初始化棋盘每个位置为false
    for(int i = 0; i < 4; ++i) {
        for(int j = 0; j < 4; ++j) {
            mp[i][j] = false;
        }
    }

    //输入
    for(int i = 0; i < 4; ++i) {
        for(int j = 0; j < 4; ++j) {
            char c;
            scanf("%c", &c);
            if(c == 'b') mp[i][j] = true; //设定b为true
        }
        getchar();
    }

    //对0-16的每个步长进行搜索
    for(step = 0; step <= 16; step++) {
        dfs(0, 0, 0);
        if(flag) break; //如果成功了，该步长就是最小值
    }

    //成功就输出步长，失败就输出impossible
    if(flag) printf("%d\n", step);
    else printf("Impossible\n");

    return 0;
}

```