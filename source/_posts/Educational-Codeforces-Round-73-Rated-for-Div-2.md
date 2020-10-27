---
title: Educational Codeforces Round 73 (Rated for Div. 2)
date: 2019-09-26 20:26:08
tags:
- ACM刷题
- 模拟
- 思维
- 动态规划
categories: 竞赛
mathjax: true
---

比赛链接：[Educational Codeforces Round 73 (Rated for Div. 2)](https://codeforces.com/contest/1221)

官方题解：[Educational Codeforces Round 73 Editorial](https://codeforces.com/blog/entry/69925)

<!--more-->

## A. 2048 Game

### 题意

如果一个只包含 $2$ 的幂次的集合，问能否从中选择一些数使得和为 $2048$。

### 思路

不断合并直到凑到 $2048$。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int q;
    cin >> q;
    while(q--) {
        map<int, int> mp;
        int n;
        cin >> n;
        int flag = 0;
        for(int i = 0; i < n; ++i) {
            int x;
            cin >> x;
            if(x == 2048) {
                flag = 1;
            }
            mp[x]++;
        }
        int tmp = 2048;
        for(int i = 1; i <= 2048; i *= 2) {
            if(mp[i] >= tmp) {
                flag = 1;
                break;
            } else {
                mp[i * 2] += mp[i] / 2;
            }
            tmp /= 2;
        }
        if(flag) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
    return 0;
}
```

## B. Knights

### 题意

给定 $n * n$ 的棋盘，放置黑白两种马，问怎么放这些马，使得相互攻击的棋子最多。 

### 思路

隔着放就行。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin >> n;
    for(int i = 1; i <= n; ++i) {
        for(int j = 1; j <= n; ++j) {
            if((i + j) % 2) {
                cout << "B";
            } else {
                cout << "W";
            }
        }
        cout << endl;
    }
    return 0;
}
```

## C. Perfect Team

### 题意

要组队参加 ICPC 比赛，有三种学生：coder，mathematician，普通人。每个队伍至少一名 coder，至少一名 mathematician，并且必须是三个人。现在给定每种学生的人数，求最多能组多少支队伍。

### 思路

设总人数为 $x$。则队伍数最多为 $\lfloor x / 3 \rfloor$。然后分别看一下 coder 的人数和 mathematician 的人数是否多于 $\lfloor x / 3 \rfloor$，取三者最少的就是答案。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int q;
    cin >> q;
    while(q--) {
        int c, m, x;
        cin >> c >> m >> x;
        int sum = c + m + x;
        sum /= 3;
        if(c < sum) {
            sum = c;
        }
        if(m < sum) {
            sum = m;
        }
        cout << sum << endl;
    }
    return 0;
}
```

## D. Make The Fence Great Again

### 题意

给定 $n$ 块板，第 $i$ 块板的高度为 $a_i$，要使相邻两块板的高度不同，可以增加板的高度，增加的代价为 $b_i$，求最少的代价。

### 思路

每块板要么不增加，要么增加 $1$，要么增加 $2$。每次从上一块板的三种状态转移过来即可。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 3e5 + 10;
const ll inf = 1e18;

ll a[maxn], b[maxn], dp[maxn][3];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int q;
    cin >> q;
    while(q--) {
        int n;
        cin >> n;
        for(int i = 1; i <= n; ++i) {
            cin >> a[i] >> b[i];
            for(int j = 0; j < 3; ++j) {
                dp[i][j] = inf;
            } 
        }
        dp[1][0] = 0;
        dp[1][1] = b[1];
        dp[1][2] = b[1] * 2;
        for(int i = 2; i <= n; ++i) {
            for(int j = 0; j <= 2; ++j) {
                for(int k = 0; k <= 2; ++k) {
                    if(a[i - 1] + k != a[i] + j) {
                        dp[i][j] = min(dp[i][j], dp[i - 1][k] + b[i] * j);
                    }
                }
            }
        }
        ll ans = min({dp[n][0], dp[n][1], dp[n][2]});
        cout << ans << endl;
    }
    return 0;
}
```
