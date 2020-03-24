---
title: POJ 2001 Shortest Prefixes (Trie)
date: 2019-10-03 23:31:57
tags:
- ACM刷题
- Trie/字典树/前缀树
- 字符串
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2001](http://poj.org/problem?id=2001)

## Description
> A prefix of a string is a substring starting at the beginning of the given string. The prefixes of "carbon" are: "c", "ca", "car", "carb", "carbo", and "carbon". Note that the empty string is not considered a prefix in this problem, but every non-empty string is considered to be a prefix of itself. In everyday language, we tend to abbreviate words by prefixes. For example, "carbohydrate" is commonly abbreviated by "carb". In this problem, given a set of words, you will find for each word the shortest prefix that uniquely identifies the word it represents.
> 
> In the sample input below, "carbohydrate" can be abbreviated to "carboh", but it cannot be abbreviated to "carbo" (or anything shorter) because there are other words in the list that begin with "carbo".
> 
> An exact match will override a prefix match. For example, the prefix "car" matches the given word "car" exactly. Therefore, it is understood without ambiguity that "car" is an abbreviation for "car" , not for "carriage" or any of the other words in the list that begins with "car".


## Input
> The input contains at least two, but no more than 1000 lines. Each line contains one word consisting of 1 to 20 lower case letters.
 
## Output
> The output contains the same number of lines as the input. Each line of the output contains the word from the corresponding line of the input, followed by one blank space, and the shortest prefix that uniquely (without ambiguity) identifies this word.
 

## Sample Input

```markdown
carbohydrate
cart
carburetor
caramel
caribou
carbonic
cartilage
carbon
carriage
carton
car
carbonate
```

## Sample Output

```markdown
carbohydrate carboh
cart cart
carburetor carbu
caramel cara
caribou cari
carbonic carboni
cartilage carti
carbon carbon
carriage carr
carton carto
car car
carbonate carbona
```

## Source

[Rocky Mountain 2004](http://poj.org/searchproblem?field=source&key=Rocky+Mountain+2004)

## Solution

### 题意

给出若干个由小写字母组成的单词，对每个单词找出最短的前缀，使得该前缀不是其他任何字符串的前缀。

如果整个单词都是其他单词的前缀就输出整个单词。

### 题解

**Trie**

标记每个结点结束的前缀是多少个单词的前缀，查找时只要找到某个唯一的前缀就输出。

## Code

```cpp
#include <cstdio>
#include <iostream>
#include <string>
#include <cstring>
using namespace std;
const int maxn = 1e4 + 10;

int nxt[maxn][30];
int val[maxn];
int tot = 1;

int index(char c) {
    return c - 'a';
}

void insert(string s) {
    int len = s.size();
    int p = 0;
    for (int i = 0; i < len; ++i) {
        int c = index(s[i]);
        if (!nxt[p][c]) {
            nxt[p][c] = tot++;
        }
        p = nxt[p][c];
        ++val[p];
    }
}
void query(string s) {
    int len = s.size();
    int p = 0;
    for (int i = 0; i < len; ++i) {
        int c = index(s[i]);
        p = nxt[p][c];
        cout << s[i];
        if(val[p] == 1) return;
    }
}

string s[maxn];
int n = 1;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    while(cin >> s[n]) {
        insert(s[n]);
        n++;
    }
    for(int i = 1; i < n; ++i) {
        cout << s[i] << " ";
        query(s[i]);
        cout << endl;
    }
    return 0;
}
```
