---
title: HDU 1686 Oulipo (KMP)
date: 2019-10-30 22:39:11
tags:
- ACM刷题
- KMP
- 字符串
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[HDU 1686](http://acm.hdu.edu.cn/showproblem.php?pid=1686)

## Problem Description

> The French author Georges Perec (1936–1982) once wrote a book, La disparition, without the letter 'e'. He was a member of the Oulipo group. A quote from the book:
> 
> Tout avait Pair normal, mais tout s’affirmait faux. Tout avait Fair normal, d’abord, puis surgissait l’inhumain, l’affolant. Il aurait voulu savoir où s’articulait l’association qui l’unissait au roman : stir son tapis, assaillant à tout instant son imagination, l’intuition d’un tabou, la vision d’un mal obscur, d’un quoi vacant, d’un non-dit : la vision, l’avision d’un oubli commandant tout, où s’abolissait la raison : tout avait l’air normal mais…
> 
> Perec would probably have scored high (or rather, low) in the following contest. People are asked to write a perhaps even meaningful text on some subject with as few occurrences of a given “word” as possible. Our task is to provide the jury with a program that counts these occurrences, in order to obtain a ranking of the competitors. These competitors often write very long texts with nonsense meaning; a sequence of 500,000 consecutive 'T's is not unusual. And they never use spaces.
> 
> So we want to quickly find out how often a word, i.e., a given string, occurs in a text. More formally: given the alphabet {'A', 'B', 'C', …, 'Z'} and two finite strings over that alphabet, a word W and a text T, count the number of occurrences of W in T. All the consecutive characters of W must exactly match consecutive characters of T. Occurrences may overlap.

<!--more-->

## Input

> The first line of the input file contains a single number: the number of test cases to follow. Each test case has the following format:
> 
> One line with the word W, a string over {'A', 'B', 'C', …, 'Z'}, with 1 ≤ |W| ≤ 10,000 (here |W| denotes the length of the string W).
One line with the text T, a string over {'A', 'B', 'C', …, 'Z'}, with |W| ≤ |T| ≤ 1,000,000.

## Output

> For every test case in the input file, the output should contain a single number, on a single line: the number of occurrences of the word W in the text T.


## Sample Input

```markdown
3
BAPC
BAPC
AZA
AZAZAZA
VERDI
AVERDXIVYERDIAN
```

## Sample Output

```markdown
1
3
0
```

## Source

> [华东区大学生程序设计邀请赛_热身赛](http://acm.hdu.edu.cn/search.php?field=problem&key=%BB%AA%B6%AB%C7%F8%B4%F3%D1%A7%C9%FA%B3%CC%D0%F2%C9%E8%BC%C6%D1%FB%C7%EB%C8%FC_%C8%C8%C9%ED%C8%FC&source=1&searchmode=source)

## Solution

### 题意

给定两个字符串，求第一个字符串在第二个字符串中出现的次数。

### 思路

KMP 模板题。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> prefix_function(string s) {
    int n = s.size();
    vector<int> pi(n);
    for(int i = 1; i < n; ++i) {
        int j = pi[i - 1];
        while(j > 0 && s[i] != s[j]) j = pi[j - 1];
        if(s[i] == s[j]) ++j;
        pi[i] = j;
    }
    return pi;
}

int kmp(string p, string t) {
    vector<int> pi = prefix_function(p);
    int ans = 0;
    int i = 0, j = 0;
    while(i < t.size()) {
        if(t[i] == p[j]) {
            ++i;
            ++j;
        } else {
            if(j == 0) ++i;
            else j = pi[j - 1];
        }
        if(j == p.size()) {
            ++ans;
        }
    }
    return ans;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    while(T--) {
        string p, t;
        cin >> p >> t;
        cout << kmp(p, t) << endl;
    }
    return 0;
}
```
