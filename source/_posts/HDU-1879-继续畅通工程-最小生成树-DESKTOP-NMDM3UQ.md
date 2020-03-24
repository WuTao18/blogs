---
title: HDU 1879 继续畅通工程 (最小生成树)
date: 2019-09-20 23:29:47
tags:
- ACM刷题
- 最小生成树
- 并查集
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1879](http://acm.hdu.edu.cn/showproblem.php?pid=1879)

## Problem Description
> 省政府“畅通工程”的目标是使全省任何两个村庄间都可以实现公路交通（但不一定有直接的公路相连，只要能间接通过公路可达即可）。现得到城镇道路统计表，表中列出了任意两城镇间修建道路的费用，以及该道路是否已经修通的状态。现请你编写程序，计算出全省畅通需要的最低成本。
 

## Input
> 测试输入包含若干测试用例。每个测试用例的第1行给出村庄数目N ( 1< N < 100 )；随后的 N(N-1)/2 行对应村庄间道路的成本及修建状态，每行给4个正整数，分别是两个村庄的编号（从1编号到N），此两村庄间道路的成本，以及修建状态：1表示已建，0表示未建。
> 
> 当N为0时输入结束。
 

## Output
> 每个测试用例的输出占一行，输出全省畅通需要的最低成本。

## Sample Input

```markdown
3
1 2 1 0
1 3 2 0
2 3 4 0
3
1 2 1 0
1 3 2 0
2 3 4 1
3
1 2 1 0
1 3 2 1
2 3 4 1
0
```

## Sample Output

```markdown
3
1
0
```

## Author

> ZJU

## Source

[浙大计算机研究生复试上机考试-2008年](http://acm.hdu.edu.cn/search.php?field=problem&key=%D5%E3%B4%F3%BC%C6%CB%E3%BB%FA%D1%D0%BE%BF%C9%FA%B8%B4%CA%D4%C9%CF%BB%FA%BF%BC%CA%D4-2008%C4%EA&source=1&searchmode=source)

## Solution

### 题意

如题。

### 思路

**最小生成树**

最小生成树裸题。把已建的道路的成本设为 0，跑一遍最小生成树即可。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e4 + 10;

struct Edge {
    int u, v, w;
} edge[maxn];

int far[maxn];

void init(int n) {
    for(int i = 0; i <= n; ++i) far[i] = i;
}

int find(int x) {
    if(far[x] != x) far[x] = find(far[x]);
    return far[x];
}

int cmp(Edge a, Edge b) {
    return a.w < b.w;
}

int Kruskal(int n, int m) {
    init(n);
    sort(edge + 1, edge + 1 + m, cmp);
    int ans = 0;
    for(int i = 1; i <= m; ++i) {
        int x = find(edge[i].u);
        int y = find(edge[i].v);
        if(x != y) {
            ans += edge[i].w;
            far[x] = y;
        }
    }
    return ans;
}

int main() {
    ios::sync_with_stdio(false);
    int n;
    while(cin >> n && n) {
        int m = n * (n - 1) / 2;
        for(int i = 1; i <= m; ++i) {
            int f;
            cin >> edge[i].u >> edge[i].v >> edge[i].w >> f;
            if(f) {
                edge[i].w = 0;
            }
        }
        cout << Kruskal(n, m) << endl;
    }
    return 0;
}
```
