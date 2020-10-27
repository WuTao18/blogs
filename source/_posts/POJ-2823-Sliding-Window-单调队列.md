---
title: POJ 2823 Sliding Window (单调队列)
date: 2019-09-07 23:28:07
tags:
- ACM刷题
- 单调队列
- STL
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2823](http://poj.org/problem?id=2823)

## Problem Description
> An array of size n ≤ 10 6 is given to you. There is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves rightwards by one position. Following is an example:
> 
> The array is [1 3 -1 -3 5 3 6 7], and k is 3.
> 
> |Window position	|Minimum value	|Maximum value|
> |:--:|--:|--:|
> |[1  3  -1] -3  5  3  6  7 	|-1	|3|
> | 1 [3  -1  -3] 5  3  6  7 	|-3	|3|
> | 1  3 [-1  -3  5] 3  6  7 	|-3	|5|
> | 1  3  -1 [-3  5  3] 6  7 	|-3	|5|
> | 1  3  -1  -3 [5  3  6] 7 	|3	|6|
> | 1  3  -1  -3  5 [3  6  7]	|3	|7|
> 
> Your task is to determine the maximum and minimum values in the sliding window at each position.

<!--more-->

## Input
> The input consists of two lines. The first line contains two integers n and k which are the lengths of the array and the sliding window. There are n integers in the second line.
 
## Output
> There are two lines in the output. The first line gives the minimum values in the window at each position, from left to right, respectively. The second line gives the maximum values.    
 

## Sample Input

```markdown
8 3
1 3 -1 -3 5 3 6 7
```

## Sample Output

```markdown
-1 -3 -3 -3 3 3
3 3 5 5 6 7
```

## Source

[POJ Monthly--2006.04.28](http://poj.org/searchproblem?field=source&key=POJ+Monthly--2006.04.28), Ikki

## Solution

### 题意

给定 $n$ 个数和一个长度为 $k$ 的滑动窗口，求滑动窗口从左滑到右每次的最大值和最小值。

### 题解

**单调队列**

单调队列模板题。

首先记录每个数的下标。

求最大值和最小值分别维护一个单调队列，如果当前插入的数的下标减去队首元素的下标超过了窗口长度 $k$，就弹出队首。

## Code

```cpp
#include <iostream>
#include <deque>
using namespace std;
const int maxn = 1e6 + 10;

struct Node {
    int val, id;
} node[maxn];

int n, k;

void getMax() {
    deque<Node> d;
    for(int i = 0; i < k; ++i) {
        while(!d.empty() && node[i].val > d.back().val) d.pop_back();
        d.push_back(node[i]);
    }
    cout << d.front().val;
    for(int i = k; i < n; ++i) {
        while(!d.empty() && node[i].id - d.front().id >= k) {
            d.pop_front();
        }
        while(!d.empty() && node[i].val > d.back().val) d.pop_back();
        d.push_back(node[i]);
        cout << " " << d.front().val;
    }
    cout << endl;
}

void getMin() {
    deque<Node> d;
    for(int i = 0; i < k; ++i) {
        while(!d.empty() && node[i].val < d.back().val) d.pop_back();
        d.push_back(node[i]);
    }
    cout << d.front().val;
    for(int i = k; i < n; ++i) {
        while(!d.empty() && node[i].id - d.front().id >= k) {
            d.pop_front();
        }
        while(!d.empty() && node[i].val < d.back().val) d.pop_back();
        d.push_back(node[i]);
        cout << " " << d.front().val;
    }
    cout << endl;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n >> k;
    for(int i = 0; i < n; ++i) {
        cin >> node[i].val;
        node[i].id = i;
    }
    getMin();
    getMax();
    return 0;
}
```
