---
title: PTA 1155 Heap Paths (DFS)
date: 2019-11-19 21:16:29
tags:
- ACM刷题
- DFS
- 堆
categories: 竞赛
mathjax: true
---

题目链接：[1155 Heap Paths (30 分)](https://pintia.cn/problem-sets/994805342720868352/problems/1071785408849047552)

> In computer science, a heap is a specialized tree-based data structure that satisfies the heap property: if P is a parent node of C, then the key (the value) of P is either greater than or equal to (in a max heap) or less than or equal to (in a min heap) the key of C. A common implementation of a heap is the binary heap, in which the tree is a complete binary tree. (Quoted from Wikipedia at https://en.wikipedia.org/wiki/Heap_(data_structure))
> 
> One thing for sure is that all the keys along any path from the root to a leaf in a max/min heap must be in non-increasing/non-decreasing order.
> 
> Your job is to check every path in a given complete binary tree, in order to tell if it is a heap or not.

## Input Specification:
> Each input file contains one test case. For each case, the first line gives a positive integer $N (1<N≤1,000)$, the number of keys in the tree. Then the next line contains $N$ distinct integer keys (all in the range of int), which gives the level order traversal sequence of a complete binary tree.

## Output Specification:

> For each given tree, first print all the paths from the root to the leaves. Each path occupies a line, with all the numbers separated by a space, and no extra space at the beginning or the end of the line. The paths must be printed in the following order: for each node in the tree, all the paths in its right subtree must be printed before those in its left subtree.
> 
> Finally print in a line `Max Heap` if it is a max heap, or `Min Heap` for a min heap, or `Not Heap` if it is not a heap at all.

## Sample Input 1:
```markdown
8
98 72 86 60 65 12 23 50
```

## Sample Output 1:
```markdown
98 86 23
98 86 12
98 72 65
98 72 60 50
Max Heap
```

## Sample Input 2:
```markdown
8
8 38 25 58 52 82 70 60
```

## Sample Output 2:
```markdown
8 25 70
8 25 82
8 38 52
8 38 58 60
Min Heap
```

## Sample Input 3:
```markdown
8
10 28 15 12 34 9 8 56
```

## Sample Output 3:
```markdown
10 15 8
10 15 9
10 28 34
10 28 12 56
Not Heap
```

## 题意

给定一个长度为 $N$ 的数组，输出从根节点到叶子结点的每一条路径，并且判断是否是堆。

## 思路

直接 dfs 输出路径。设立两个变量 Max 和 Min 统计父节点比子节点大的个数和父节点比子节点小的个数。如果两者都不为 0，说明不是堆；如果 Max 为 0，说明是小顶堆，如果 Min 为 0，说明是大顶堆。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e3 + 10;

int arr[maxn];
int n;
int path[11];  // 保存路径
int Max = 0, Min = 0;

void dfs(int step, int id) {
    path[step] = arr[id];
    if(id * 2 > n) {
        for(int i = 1; i < step; ++i) {
            cout << path[i] << " ";
        }
        cout << path[step] << endl;
        return;
    }
    int l = id * 2, r = id * 2 + 1;  // 左右儿子结点
    if(r <= n) {
        if(path[step] < arr[r]) ++Min;
        if(path[step] > arr[r]) ++Max;
        dfs(step + 1, r);
    }
    if(l <= n) {
        if(path[step] < arr[l]) ++Min;
        if(path[step] > arr[l]) ++Max;
        dfs(step + 1, l);
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n;
    for(int i = 1; i <= n; ++i) {
        cin >> arr[i];
    }
    dfs(1, 1);
    if(Min == 0) {
        cout << "Max Heap" << endl;
    } else if(Max == 0) {
        cout << "Min Heap" << endl;
    } else {
        cout << "Not Heap" << endl;
    }
    return 0;
}
```