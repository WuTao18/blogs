---
title: HDU 6628 permutation 1 (暴力)
date: 2019-08-14 20:06:34
tags:
- ACM刷题
- 暴力
- STL
categories: 竞赛
mathjax: true
---

2019 杭电多校 5 1005

题目链接：[HDU 6628](http://acm.hdu.edu.cn/showproblem.php?pid=6628)

比赛链接：[2019 Multi-University Training Contest 5](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+5&source=1&searchmode=source)


## Problem Description
> A sequence of length $n$ is called a permutation if and only if it's composed of the first $n$ positive integers and each number appears exactly once.
>
> Here we define the "difference sequence" of a permutation $p_1, p_2,...,p_n$ as $p_2−p_1,p_3−p_2,...,p_n−p_{n−1}$. In other words, the length of the difference sequence is $n−1$ and the $i$-th term is $p_{i+1}−p_i$
> 
> Now, you are given two integers $N,K$. Please find the permutation with length $N$ such that the difference sequence of which is the $K$-th lexicographically smallest among all difference sequences of all permutations of length $N$.

<!--more-->

## Input
> The first line contains one integer $T$ indicating that there are $T$ tests.
> 
> Each test consists of two integers $N,K$ in a single line.
> 
> $*\ 1≤T≤40$ 
> 
> $*\ 2≤N≤20$ 
> 
> $*\ 1\le K\le min(10^4, N!)$

 

## Output
> For each test, please output $N$ integers in a single line. Those $N$ integers represent a permutation of $1$ to $N$, and its difference sequence is the $K$-th lexicographically smallest.
 

## Sample Input
```markdown
7
3 1
3 2
3 3
3 4
3 5
3 6
20 10000
```

## Sample Output
```markdown
3 1 2
3 2 1
2 1 3
2 3 1
1 2 3
1 3 2
20 1 2 3 4 5 6 7 8 9 10 11 13 19 18 14 16 15 17 12
```

## Solution

### 题意：

定义排列 $p_1, p_2, ... , p_n$ 的 "difference sequence" 为 $p_2-p_1, p_3-p_2,...,p_n-p_{n-1}$。现在给定 $N$ 和 $K$，求长度为 $N$ 的所有排列中 "difference sequence" 的字典序第 $K$ 小的排列。

### 题解：

**暴力 STL 全排列**

题目给定 $K$ 的范围不超过 $10^4$，而 $8! = 40320 > K$，因此可以预处理 $N <= 8$ 的情况，当 $N > 8$ 时暴力求 $a[1] = n$ 的全排列。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

const int maxn = 1e5 + 10;

struct P {
    int num[10];
    string str;
} ans[10][maxn];

int cmp(P p1, P p2) {
    return p1.str < p2.str;
}

int main() {
    int a[10] = {0, 1, 2, 3, 4, 5, 6, 7, 8};
    for(int i = 2; i <= 8; ++i) {
        int cnt = 1;
        do {
            // for(int j = 0; j < i; ++j) {
            //     cout << a[j] << " ";
            // }
            // cout << endl;
            for(int j = 1; j <= i; ++j) {
                ans[i][cnt].num[j] = a[j];
                if(j < i) ans[i][cnt].str += a[j + 1] - a[j] + 'A';
            }
            ++cnt;
        } while(next_permutation(a + 1, a + 1 + i));
        sort(ans[i] + 1, ans[i] + cnt, cmp);
        // for(int j = 1; j < cnt; ++j) { for(int k = 1; k <= i; ++k) cout << ans[i][j].num[k] << ""; cout << endl;}
    }
    int T;
    cin >> T;
    while(T--) {
        int n, k;
        scanf("%d%d", &n, &k);
        if(n <= 8) {
            for(int j = 1; j <= n; ++j) {
                printf("%d", ans[n][k].num[j]);
                printf("%s", j == n? "\n": " ");
            }
        } else {
            int a[30];
            a[1] = n;
            for(int i = 2; i <= n; ++i) {
                a[i] = i - 1;
            }
            for(int i = 0; i < k - 1; ++i) {
                next_permutation(a + 1, a + 1 + n);
            }
            for(int i = 1; i <= n; ++i) {
                printf("%d", a[i]);
                printf("%s", i == n? "\n": " ");
            }
        }
    }
    return 0;
}
```