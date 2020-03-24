---
title: HDU 1548 A strange lift (BFS)
date: 2019-08-06 21:03:54
tags: 
- BFS
- ACM刷题
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1548](http://acm.hdu.edu.cn/showproblem.php?pid=1548)

## Description
> There is a strange lift.The lift can stop can at every floor as you want, and there is a number Ki(0 <= Ki <= N) on every floor.The lift have just two buttons: up and down.When you at floor i,if you press the button "UP" , you will go up Ki floor,i.e,you will go to the i+Ki th floor,as the same, if you press the button "DOWN" , you will go down Ki floor,i.e,you will go to the i-Ki th floor. Of course, the lift can't go up high than N,and can't go down lower than 1. For example, there is a buliding with 5 floors, and k1 = 3, k2 = 3,k3 = 1,k4 = 2, k5 = 5.Begining from the 1 st floor,you can press the button "UP", and you'll go up to the 4 th floor,and if you press the button "DOWN", the lift can't do it, because it can't go down to the -2 th floor,as you know ,the -2 th floor isn't exist. 
> 
> Here comes the problem: when you are on floor A,and you want to go to floor B,how many times at least he has to press the button "UP" or "DOWN"? 

## Input
> The input consists of several test cases.,Each test case contains two lines. 
> The first line contains three integers N ,A,B( 1 <= N,A,B <= 200) which describe above,The second line consist N integers k1,k2,....kn. 
> A single 0 indicate the end of the input.

## Output
> For each case of the input output a interger, the least times you have to press the button when you on floor A,and you want to go to floor B.If you can't reach floor B,printf "-1".

## Sample Input
```markdown
5 1 5
3 3 1 2 5
0
```

## Sample Output
```markdown
3
```

## Solution

### 题意

一幢楼有 n 层，电梯在每一层都可以上或者下 k 层，问能不能从 A 层到达 B 层，如果能输出最少坐几次电梯，如果不能输出 -1。

### 题解

BFS 水题

使用 BFS 求最少次数，每次搜索两种状态：上或下，如果能到达 B 层，就输出最先到达的步数，如果不能输出-1。

注：此题还可用 Dijkstra 求解。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll inf = 0x3f3f3f3f;
const int maxn = 1010;

int n, A, B;
int k[maxn], dis[maxn], vis[maxn];

int bfs(int s) {
    queue<int> que;
    que.push(s);
    while(!que.empty()) {
        int x = que.front();
        que.pop();
        if(x == B) return dis[x];  // 如果到达 B 层就输出
        for(int i = -1; i <= 1; i += 2) {
            int tmp = x + i * k[x];  // 往上或者往下
            // 判断楼层是否合法
            if(tmp > 0 && tmp <= n && !vis[tmp]) {
                dis[tmp] = dis[x] + 1;
                vis[tmp] = 1;
                que.push(tmp);
            }
        }
    }
    return -1;  // 无法到达 B 层
}

int main() {
    while(~scanf("%d",&n) && n) {
        // 输入
        scanf("%d%d", &A, &B);
        for(int i = 1; i <= n; ++i) {
            scanf("%d", &k[i]);
        }
        // 初始化
        for(int i = 1; i <= n; ++i) {
            dis[i] = inf;
            vis[i] = 0;
        }
        dis[A] = 0;
        vis[A] = 1;
        // 搜索并输出
        printf("%d\n", bfs(A));
    }
    return 0;
}

```