---
title: POJ 2251 Dungeon Master (BFS)
date: 2019-08-14 20:29:40
tags:
- ACM刷题
- BFS
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2251](http://poj.org/problem?id=2251)

## Description
> You are trapped in a 3D dungeon and need to find the quickest way out! The dungeon is composed of unit cubes which may or may not be filled with rock. It takes one minute to move one unit north, south, east, west, up or down. You cannot move diagonally and the maze is surrounded by solid rock on all sides. 
> 
> Is an escape possible? If yes, how long will it take?  

<!--more-->

## Input
> The input consists of a number of dungeons. Each dungeon description starts with a line containing three integers $L$, $R$ and $C$ (all limited to $30$ in size). 
> 
> $L$$ is the number of levels making up the dungeon. 
> 
> $R$ and $C$ are the number of rows and columns making up the plan of each level. 
> 
> Then there will follow $L$ blocks of $R$ lines each containing $C$ characters. Each character describes one cell of the dungeon. A cell full of rock is indicated by a '$\#$' and empty cells are represented by a '$.$'. Your starting position is indicated by '$S$' and the exit by the letter '$E$'. There's a single blank line after each level. Input is terminated by three zeroes for $L$, $R$ and $C$.

## Output
> Each maze generates one line of output. If it is possible to reach the exit, print a line of the form
>  
> Escaped in x minute(s).
> 
> where x is replaced by the shortest time it takes to escape. 
> 
> If it is not possible to escape, print the line 
> 
> Trapped!
 

## Sample Input
```markdown
3 4 5
S....
.###.
.##..
###.#

#####
#####
##.##
##...

#####
#####
#.###
####E

1 3 3
S##
#E#
###

0 0 0
```

## Sample Output
```markdown
Escaped in 11 minute(s).
Trapped!
```

## Solution

### 题意

> 给定三维的迷宫，求 S 到 E 的最近距离

### 思路

**BFS**

三维迷宫的 $BFS$，与二维迷宫的 $BFS$ 类似，每次往 $6$ 个方向搜索，第一次到达 $E$ 点的距离就是最短距离。

## Code

```cpp
#include <cstdio>
#include <iostream>
#include <queue>
#include <algorithm>
#include <cstring>
#include <string>
using namespace std;

int r, n, m;
string s[50][50];

struct node {
    int x, y, z;  // 坐标
    int t;  // 时间
};

node Begin, End;  // 起点和终点
queue <node> que;
int dx[6] = {0, 0, 0, 0, 1, -1};
int dy[6] = {0, 0, -1, 1, 0, 0};
int dz[6] = {1, -1, 0, 0, 0, 0};
int vis[50][50][50];

int bfs() {
    que.push(Begin);  // 加入起点
    vis[Begin.x][Begin.y][Begin.z] = 1;  // 起点已访问
    while(!que.empty()) {
        // 取出队首
        node front = que.front();
        que.pop();
        // 判断是否到达终点
        if (front.x == End.x && front.y == End.y && front.z == End.z) {
            return front.t;
        }
        // 搜索六个方向
        for (int i = 0; i < 6; ++i) {
            node tmp;
            tmp.x = front.x + dx[i];
            tmp.y = front.y + dy[i];
            tmp.z = front.z + dz[i];
            tmp.t = front.t + 1;
            if (tmp.x >= 0 && tmp.x < r && tmp.y >= 0 && tmp.y < n && tmp.z >= 0 && tmp.z < m && !vis[tmp.x][tmp.y][tmp.z] && s[tmp.x][tmp.y][tmp.z] != '#') {
                que.push(tmp);
                vis[tmp.x][tmp.y][tmp.z] = 1;
            }
        }
    }
    return -1;  // 无法到达
}

int main() {
    while(cin >> r >> n >> m) {
        if (r == 0 && n == 0 && m == 0) break;
        // 初始化
        memset(vis, 0, sizeof(vis));
        while(!que.empty()) {
            que.pop();
        }
        // 输入
        for (int i = 0; i < r; ++i) {
            for (int j = 0; j < n; ++j) {
                cin >> s[i][j];
                // 寻找起点和终点
                for (int k = 0; k < m; ++k) {
                    if (s[i][j][k] == 'S') {
                        Begin.x = i;
                        Begin.y = j;
                        Begin.z = k;
                        Begin.t = 0;
                    } else if (s[i][j][k] == 'E') {
                        End.x = i;
                        End.y = j;
                        End.z = k;
                        End.t = 0;
                    }
                }
            }
        }

        int ans = bfs(); // 搜索
        // 输出
        if (ans == -1) {
            printf("Trapped!\n");
        } else {
            printf("Escaped in %d minute(s).\n", ans);
        }
    }

    return 0;
}
```