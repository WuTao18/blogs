---
title: POJ 3974 Palindrome (Manacher)
date: 2019-11-14 21:59:48
tags:
- ACM刷题
- Manacher
- 字符串
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[POJ 3974](http://poj.org/problem?id=3974)

## Description
> Andy the smart computer science student was attending an algorithms class when the professor asked the students a simple question, "Can you propose an efficient algorithm to find the length of the largest palindrome in a string?"
> 
> A string is said to be a palindrome if it reads the same both forwards and backwards, for example "madam" is a palindrome while "acm" is not.
> 
> The students recognized that this is a classical problem but couldn't come up with a solution better than iterating over all substrings and checking whether they are palindrome or not, obviously this algorithm is not efficient at all, after a while Andy raised his hand and said "Okay, I've a better algorithm" and before he starts to explain his idea he stopped for a moment and then said "Well, I've an even better algorithm!".
> 
> If you think you know Andy's final solution then prove it! Given a string of at most 1000000 characters find and print the length of the largest palindrome inside this string.

<!--more-->

## Input
> Your program will be tested on at most 30 test cases, each test case is given as a string of at most 1000000 lowercase characters on a line by itself. The input is terminated by a line that starts with the string "END" (quotes for clarity).
 
## Output
> For each test case in the input print the test case number and the length of the largest palindrome.

## Sample Input

```markdown
abcbabcbabcba
abacacbaaaab
END
```

## Sample Output

```markdown
Case 1: 13
Case 2: 6
```

## Source

[Seventh ACM Egyptian National Programming Contest](http://poj.org/searchproblem?field=source&key=Seventh+ACM+Egyptian+National+Programming+Contest)

## Solution

### 题意

给定一个字符串，求最长回文子串。

### 思路

Manacher 模板题。

有关 Manacher 算法的讲解见[这里](https://wutao18.github.io/2019/11/13/%E6%9C%80%E9%95%BF%E5%9B%9E%E6%96%87%E5%AD%90%E4%B8%B2-%E2%80%94%E2%80%94-Manacher-%E9%A9%AC%E6%8B%89%E8%BD%A6-%E7%AE%97%E6%B3%95/)

## Code

```cpp
#include <iostream>
#include <cstdio>
#include <string>
#include <cstring>
#include <cmath>
#include <algorithm>
#include <cstdlib>
using namespace std;
const int maxn = 2000010;

string init(string s) {
    string res;
    res += '@';
    for(int i = 0; i < s.size(); ++i) {
        res += '#';
        res += s[i];
    }
    res += '#';
    res += '$';
    return res;
}

int len[maxn];

int Manacher(string s) {
    memset(len, 0, sizeof(len));
    int mx = 0, id = 0;
    int ans = 0;
    for(int i = 1; i < s.size() - 1; ++i) {
        if(mx > i) {
            len[i] = min(mx - i, len[2 * id - i]);
        } else {
            len[i] = 1;
        }
        while(s[i - len[i]] == s[i + len[i]]) {
            ++len[i];
        }
        if(i + len[i] > mx) {
            mx = i + len[i];
            id = i;
        }
        ans = max(ans, len[i]);
    }
    return ans - 1;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    string s;
    int kase = 0;
    while (cin >> s) {
        if(s == "END") break;
        string tmp = init(s);
        cout << "Case " << (++kase) << ": ";
        cout << Manacher(tmp) << endl;
    }
    return 0;
}
```
