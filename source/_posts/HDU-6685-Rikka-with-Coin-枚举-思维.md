---
title: HDU 6685 Rikka with Coin (枚举 思维)
date: 2019-08-20 18:11:37
tags:
- ACM刷题
- 枚举
- 思维
categories: 竞赛
mathjax: true
---

2019 杭电多校 9 1006

题目链接：[HDU 6685](http://acm.hdu.edu.cn/showproblem.php?pid=6685)

比赛链接：[2019 Multi-University Training Contest 9](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+9&source=1&searchmode=source)

## Problem Description
> Rikka hates coins, and she used to never carry any coins with her. These days, Rikka is doing her summer internship abroad. Without mobile payment, Rikka has to face strange prices of commodities, and as a result of always using paper currency, she has to face mountainous coins on here table. 
> 
> In the local currency system, there are $4$ kinds of coins: $10$ cents, $20$ cents, $50$ cents and $1$ dollar. Up to now, Rikka has gained at least $10^{100}$ coins for each kind. 
> 
> Now, Rikka is going to have dinner in the canteen, and she decides to pay the bill only with coins. There are $n$ different combos in the canteen and the price of the $i$th is $w_i$ cents. Rikka would like to choose one combo as dinner but she has not decided to choose which one yet. Therefore, she wants to take some coins so that whichever she chooses, she can always pay the bill without receiving any change. 
> 
> Since Rikka hates coins, she wants to carry as few coins as possible with her. As it is generally known that Rikka is not good at math, she wants you to help her make the decision.
 

## Input
> The first line of the input contains a single integer $T(1\le T\le 500)$, the number of test cases. 
> 
> For each test case, the first line contains a single integer $n(1\le n\le 100)$, the number of combos sold in the canteen. 
> 
> The second line contains $n$ positive integers $w_1,…,w_n(1\le w_i\le 10^9)$, which represents the prices.
 

## Output
> For each test case, output a single line with a single integer which represents the minimum number of coins. If there is no valid solution, output $−1$. 
>> **Hint**
>> 
>> In the first test case, one optimal solution is to bring one coin of $10$ cents and two coins of $20$ cents.
>>
>> In the second test case, one optimal solution is to bring $5$ coins of one dollar.
 

## Sample Input

```markdown
3
5
10 20 30 40 50
5
100 200 300 400 500
1
1
```

## Sample Output

```markdown
3
5
-1
```

## Solution

### 题意

给出 $n$ 种物品的价格，现在要从无限枚 $10$元，$20$元，$50$元，$100$元的硬币中选出最少的硬币，满足能购买任何一种物品都不用找零。

### 题解

显然如果个位不为零时没有可行方案。

接下来考虑可行方案的求解。

$10$ 分的硬币多只会用一个，如果用了两个，直接替换成一个 $10$ 分一个 $20$ 分一定不亏。

$20$ 分的硬币多只会用三个，如果用了四个，直接替换成一个 $10$ 分两个 $20$ 分一个 $50$ 分一定不亏。

$50$ 分的硬币多只会用一个，如果用了两个，直接替换成一个 $50$ 分和一个一元一定不亏。 

因此，直接暴力枚举 $10$, $20$, $50$ 的硬币的数量即可，整百的部分用一元硬币填充。



## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const double eps = 1e-8;
const int inf = 0x3f3f3f3f;
const int maxn = 100 + 5;

int w[maxn];

bool judge(int n, int a, int b, int c) {
    for(int i = 0; i <= a; ++i) {
        for(int j = 0; j <= b; ++j) {
            for(int k = 0; k <= c; ++k) {
                if(i * 50 + j * 20 + k * 10 == n) {
                    return true;
                }
            }
        }
    }
    return false;
}

int main() {
    std::ios::sync_with_stdio(false);
    cin.tie(0);
    cout.tie(0);
    int T;
    cin >> T;
    while (T--) {
        int n;
        cin >> n;
        int flag = 0;
        for(int i = 0; i < n; ++i) {
            cin >> w[i];
            if(w[i] % 10) {
                flag = 1;
            }
        }
        if(flag) {
            cout << -1 << endl;
            continue;
        }
        int ans = inf;
        for(int j = 0; j <= 1; ++j) {
            for(int k = 0; k <= 3; ++k) {
                for(int l = 0; l <= 1; ++l) {
                    int flag = 1;
                    int cnt = 0;
                    for(int i = 0; i < n; ++i) {
                        if(w[i] < 100) {
                            if(judge(w[i], j, k, l)) {
                                continue;
                            } else {
                                flag = 0;
                                break;
                            }
                        } else {
                            if(judge(w[i] % 100 + 100, j, k, l)) {
                                cnt = max(cnt, (w[i] - 100) / 100);
                            } else if(judge(w[i] % 100, j, k, l)) {
                                cnt = max(cnt, w[i] / 100);
                            } else {
                                flag = 0;
                                break;
                            }
                        }
                    }
                    if(flag) {
                        ans = min(ans, cnt + j + k + l);
                    }
                }
            }
        }
        cout << ans << endl;
    }
    return 0;
}
```
