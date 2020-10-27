---
title: POJ 2287 Tian Ji -- The Horse Racing (贪心)
date: 2019-11-06 23:11:28
tags:
- ACM刷题
- 贪心
- 思维
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2287](http://poj.org/problem?id=2287)

## Description
> Here is a famous story in Chinese history.
> 
> That was about 2300 years ago. General Tian Ji was a high official in the country Qi. He likes to play horse racing with the king and others.
> 
> Both of Tian and the king have three horses in different classes, namely, regular, plus, and super. The rule is to have three rounds in a match; each of the horses must be used in one round. The winner of a single round takes two hundred silver dollars from the loser.
> 
> Being the most powerful man in the country, the king has so nice horses that in each class his horse is better than Tian's. As a result, each time the king takes six hundred silver dollars from Tian.
> 
> Tian Ji was not happy about that, until he met Sun Bin, one of the most famous generals in Chinese history. Using a little trick due to Sun, Tian Ji brought home two hundred silver dollars and such a grace in the next match.
> 
> It was a rather simple trick. Using his regular class horse race against the super class from the king, they will certainly lose that round. But then his plus beat the king's regular, and his super beat the king's plus. What a simple trick. And how do you think of Tian Ji, the high ranked official in China?
> 
> ![](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/11/06/1573050687823-1573050688065.png)
> 
> Were Tian Ji lives in nowadays, he will certainly laugh at himself. Even more, were he sitting in the ACM contest right now, he may discover that the horse racing problem can be simply viewed as finding the maximum matching in a bipartite graph. Draw Tian's horses on one side, and the king's horses on the other. Whenever one of Tian's horses can beat one from the king, we draw an edge between them, meaning we wish to establish this pair. Then, the problem of winning as many rounds as possible is just to find the maximum matching in this graph. If there are ties, the problem becomes more complicated, he needs to assign weights 0, 1, or -1 to all the possible edges, and find a maximum weighted perfect matching...
> 
> However, the horse racing problem is a very special case of bipartite matching. The graph is decided by the speed of the horses -- a vertex of higher speed always beat a vertex of lower speed. In this case, the weighted bipartite matching algorithm is a too advanced tool to deal with the problem.
> 
> In this problem, you are asked to write a program to solve this special case of matching problem.

<!--more-->

## Input
> The input consists of up to 50 test cases. Each case starts with a positive integer n ( n<=1000) on the first line, which is the number of horses on each side. The next n integers on the second line are the speeds of Tian's horses. Then the next n integers on the third line are the speeds of the king's horses. The input ends with a line that has a single `0' after the last test case.
 
## Output
> For each input case, output a line containing a single number, which is the maximum money Tian Ji will get, in silver dollars.


## Sample Input

```markdown
3
92 83 71
95 87 74
2
20 20
20 20
2
20 19
22 18
0
```

## Sample Output

```markdown
200
0
0
```

## Source

[Shanghai 2004](http://poj.org/searchproblem?field=source&key=Shanghai+2004)

## Solution

### 题意

田忌赛马，田忌和齐王都有 $n$ 匹马，赢一局得 $200$，输一局失 $200$，平局财产不动。求田忌最多能赢多少钱。

### 思路

**贪心**

首先拿田忌最快的马与齐王最快的马比。

如果田忌的马快就赢一场，否则用最慢的马去比，输一场。

如果田忌最快的马与齐王最快的马一样快，则拿田忌最慢的马与齐王最慢的马比。

## Code

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
int a[1010], b[1010];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    while(cin >> n) {
        if(n == 0) break;
        for(int i = 0; i < n; ++i) {
            cin >> a[i];
        }
        for(int i = 0; i < n; ++i) {
            cin >> b[i];
        }
        sort(a, a + n);
        sort(b, b + n);
        int min1 = 0, min2 = 0;
        int max1 = n - 1, max2 = n - 1;
        int ans = 0;
        for(int i = 0; i < n; ++i) {
            if(a[max1] > b[max2]) {  // 田忌最快的马比齐王最快的马快
                ++ans;
                max1--;
                max2--;
            } else if(a[max1] < b[max2]) {  // 田忌最快的马比齐王最快的马慢
                --ans;
                min1++;
                max2--;
            } else {  // 田忌最快的马与齐王最快的马一样快
                if(a[min1] > b[min2]) {  // 田忌最慢的马比齐王最慢的马快
                    ++ans;
                    min1++;
                    min2++;
                } else {
                    if(a[min1] < b[max2]) {  // 田忌最慢的马比齐王最慢的马慢
                        --ans;
                    }
                    min1++;
                    max2--;
                }
            }
        }
        cout << ans * 200 << endl;
    }
    return 0;
}
```
