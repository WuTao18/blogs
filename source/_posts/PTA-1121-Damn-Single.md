---
title: PTA 1121 Damn Single
date: 2019-11-22 23:34:17
tags:
- ACM刷题
- STL
categories: 竞赛
mathjax: true
---

题目链接：[1121 Damn Single (25 分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805352359378944)

> "Damn Single (单身狗)" is the Chinese nickname for someone who is being single. You are supposed to find those who are alone in a big party, so they can be taken care of.

<!--more-->

## Input Specification:
> Each input file contains one test case. For each case, the first line gives a positive integer N (≤ 50,000), the total number of couples. Then N lines of the couples follow, each gives a couple of ID's which are 5-digit numbers (i.e. from 00000 to 99999). After the list of couples, there is a positive integer M (≤ 10,000) followed by M ID's of the party guests. The numbers are separated by spaces. It is guaranteed that nobody is having bigamous marriage (重婚) or dangling with more than one companion.

## Output Specification:
> First print in a line the total number of lonely guests. Then in the next line, print their ID's in increasing order. The numbers must be separated by exactly 1 space, and there must be no extra space at the end of the line.

## Sample Input:
```markdown
3
11111 22222
33333 44444
55555 66666
7
55555 44444 10000 88888 22222 11111 23333
```

## Sample Output:
```markdown
5
10000 23333 44444 55555 88888
```

## 题意

给定情侣关系，然后给出一次宴会上出席的人，问这些人中有多少单身狗 (伴侣没来的也算)。

## 思路

记录情侣关系，然后找出出席的人中不在情侣关系中的人以及伴侣没有来的人即可。

注意输出要补零，最后一行不用换行。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 10;

int couple[maxn] = {-1};

int main() {
    int n, m;
    scanf("%d", &n);
    for(int i = 0; i < n; ++i) {
        int x, y;
        scanf("%d%d", &x, &y);
        couple[x] = y;
        couple[y] = x;
    }
    scanf("%d", &m);
    vector<int> party;
    for(int i = 0; i < m; ++i) {
        int x;
        scanf("%d", &x);
        party.push_back(x);
    }
    set<int> ans;
    for(int i = 0; i < m; ++i) {
        int tmp = couple[party[i]];
        if(tmp == -1) {
            ans.insert(party[i]);
        } else {
            if(find(party.begin(), party.end(), tmp) == party.end()) {
                ans.insert(party[i]);
            }
        }
    }
    cout << ans.size() << endl;
    for (auto it = ans.begin(); it != ans.end(); ++it) {
        if(it != ans.begin()) {
            printf(" ");
        }
        printf("%05d", *it);
    }
    printf("\n");
    return 0;
}
```