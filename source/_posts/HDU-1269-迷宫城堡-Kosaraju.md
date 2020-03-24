---
title: HDU 1269 迷宫城堡 (Kosaraju)
date: 2019-11-21 22:30:56
tags:
- ACM刷题
- Kosaraju
- 强连通分量
- 图论
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1269](http://acm.hdu.edu.cn/showproblem.php?pid=1269)

## Problem Description

> 为了训练小希的方向感，Gardon建立了一座大城堡，里面有N个房间(N<=10000)和M条通道(M<=100000)，每个通道都是单向的，就是说若称某通道连通了A房间和B房间，只说明可以通过这个通道由A房间到达B房间，但并不说明通过它可以由B房间到达A房间。Gardon需要请你写个程序确认一下是否任意两个房间都是相互连通的，即：对于任意的i和j，至少存在一条路径可以从房间i到房间j，也存在一条路径可以从房间j到房间i。

## Input

> 输入包含多组数据，输入的第一行有两个数：N和M，接下来的M行每行有两个数a和b，表示了一条通道可以从A房间来到B房间。文件最后以两个0结束。

## Output

> 对于输入的每组数据，如果任意两个房间都是相互连接的，输出"Yes"，否则输出"No"。

## Sample Input

```markdown
3 3
1 2
2 3
3 1
3 3
1 2
2 3
3 2
0 0
```

## Sample Output

```markdown
Yes
No
```

## Source

> [HDU 2006-4 Programming Contest](http://acm.hdu.edu.cn/search.php?field=problem&key=HDU+2006-4+Programming+Contest+&source=1&searchmode=source)

## Solution

### 题意

如题。

### 思路

求强连通分量的模板题。

这里使用的是 Kosaraju 算法。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e4 + 5;

vector<int> g[maxn], rg[maxn];  // 原图和反图
vector<int> s;  // 时间戳
int vis[maxn];
int sccno[maxn];
int cnt;  // 强连通分量的个数

void init(int n) {
    for(int i = 0; i < n; ++i) {
        g[i].clear();
        rg[i].clear();
    }
    cnt = 0;
    s.clear();
    memset(sccno, 0, sizeof(sccno));
    memset(vis, 0, sizeof(vis));
}

void dfs1(int u) {
    if(vis[u]) return;
    vis[u] = 1;
    for(int i = 0; i < g[u].size(); ++i) {
        dfs1(g[u][i]);
    }
    s.push_back(u);
}

void dfs2(int u) {
    if(sccno[u]) return;
    sccno[u] = cnt;
    for(int i = 0; i < rg[u].size(); ++i) {
        dfs2(rg[u][i]);
    }
}

void Kosaraju(int n) {
    for(int i = 1; i <= n; ++i) {
        dfs1(i);
    }
    for(int i = n - 1; ~i; --i) {
        if(!sccno[s[i]]) {
            ++cnt;
            dfs2(s[i]);
        }
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n, m;
    while(cin >> n >> m) {
        if(n == 0 && m == 0) break;
        init(n);
        for(int i = 0; i < m; ++i) {
            int u, v;
            cin >> u >> v;
            g[u].push_back(v);
            rg[v].push_back(u);
        }
        Kosaraju(n);
        cout << (cnt == 1? "Yes": "No") << endl;
    }
    return 0;
}
```