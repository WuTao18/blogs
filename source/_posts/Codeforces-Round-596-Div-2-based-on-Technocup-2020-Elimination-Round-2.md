---
title: 'Codeforces Round #596 (Div. 2, based on Technocup 2020 Elimination Round 2)'
date: 2019-11-02 22:40:00
tags:
- ACM刷题
- 枚举
- 构造
- 思维
- 素数
- STL
categories: 竞赛
mathjax: true
---

比赛链接：[Codeforces Round #596 (Div. 2, based on Technocup 2020 Elimination Round 2)](https://codeforces.com/contest/1247)

官方题解：[Technocup 2020 — Elimination Round 2 + Codeforces Round 596: analysis](https://codeforces.com/blog/entry/70898)

<!--more-->

## A. Forgetting Things

### 题意

有两个数 $a$ 和 $b$，给定 $a$ 的首位 $d_a$ 和 $b$ 的首位 $d_b$，问能否构造出 $a$ 和 $b$，满足 $a + 1 = b$

### 思路

如果 $a = b$，构造 $a1$，$b2$。

如果 $a + 1 = b$，构造 $a9$，$b0$。

如果 $a = 9$，$b = 1$，构造 $9$，$10$。

否则无法构造。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main() {
    int n, m;
    cin >> n >> m;
    if(n == m) {
        printf("%d1 %d2\n", n, m);
    } else if(n == 9 && m == 1) {
        printf("9 10\n");
    } else if(n + 1 == m) {
        printf("%d9 %d0\n", n, m);
    } else {
        printf("-1\n");
    }
    return 0;
}

```

## B1. TV Subscriptions (Easy Version)

### 题意

某电视节目有 $k$ 集，告诉你 $n$ 天每天播放哪一集，现在要看连续 $d$ 天，最少需要买几集。

### 思路

暴力。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
 
int a[110];
 
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    while(T--) {
        int n, k, d;
        cin >> n >> k >> d;
        for(int i = 0; i < n; ++i) {
            cin >> a[i];
        }
        int ans = 0x3f3f3f3f;
        map<int, int> mp;
        for(int i = 0; i < n - d + 1; ++i) {
            mp.clear();
            for(int j = 0; j < d; ++j) {
                mp[a[i + j]]++;  
            }
            int tmp = mp.size();
            ans = min(ans, tmp);
        }
        int tmp = mp.size();
        ans = min(ans, tmp);
        cout << ans << endl;
    }
    return 0;
}
```

## B2. TV Subscriptions (Hard Version)

### 题意

与上一题相同，数据范围变大。

### 思路

维护一个长度为 $d$ 的滑动窗口即可。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 2e5 + 10;
 
int a[maxn];
int mp[maxn * 5];
 
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    while(T--) {
        memset(mp, 0, sizeof(mp));
        int n, k, d;
        cin >> n >> k >> d;
        for(int i = 0; i < n; ++i) {
            cin >> a[i];
        }
        int ans = 0x3f3f3f3f;
        int tmp = 0;
        for(int i = 0; i < n; ++i) {
            if(i < d) {
                if(mp[a[i]] == 0) ++tmp;
                mp[a[i]]++;
            } else {
                ans = min(ans, tmp);
                mp[a[i - d]]--;
                if(mp[a[i - d]] == 0) --tmp;
                if(mp[a[i]] == 0) ++tmp;
                mp[a[i]]++;
            }
        }
        ans = min(ans, tmp);
        cout << ans << endl;
    }
    return 0;
}
```

## C. p-binary

### 题意

给定两个整数 $n$ 和 $p$，将 $n$ 分解成若干个 $2^x + p$ 的和，求最少需要几项。如 $n=24,p=-1$，则 $24 = (2^4 - 1) + (2^2 - 1) + (2^2 - 1) + (2^2 - 1)$，所以答案为 $4$。

### 思路

枚举项数 $i$，$2^{31}>1e9$，所以不超过 $31$ 项。然后将 $n$ 减去 $p*i$，$n - p\times i$ 最多有 $n - p\times i$ 项 (每项都为 $2^0$)，最少项数为二进制分解后二进制中为 $1$ 的个数，判断 $i$ 是否在这个范围内即可。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main() {
    int n, p;
    int ans = -1, tmp = 0;
    scanf("%d%d", &n, &p);
    for(int i = 1; i <= 31; ++i) {
        int cnt = 0;
        tmp = n - p * i;
        while(tmp > 0) {
            if(tmp & 1) {
                cnt++;
            }
            tmp >>= 1;
        }
        if(cnt <= i && i <= n - p * i){
            ans = i;
            break;
        }
    }
    printf("%d\n", ans);
    return 0;
}
```


## D. Power Products

### 题意

给定 $n$ 个正整数 $a_1, a_2, ..., a_n$，和一个整数 $k\ge 2$，问有多少对 $a_i, a_j$ 满足 $a_i\cdot a_j = x^k$，$x$ 是整数。

### 思路

将每个数质因数分解，将每个因子和模 $k$ 后的数量存入 $map$，然后找所需的剩余因子个数满足的是否存在。

比如 $24 = 2^3 * 3$，设 $k = 2$，则 $map$ 中存 $((2,\ 3\mod k),\ (3,\ 1 \mod k))$，$((2,\ 1),\ (3,\ 1))$。然后在 $map$ 中找 $((2,k - 1),\ (3,\ k - 1))$ 是否存在即可。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e5 + 10;
 
ll a[maxn];
 
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    ll n, k;
    cin >> n >> k;
    map<vector<pair<ll, ll> >, ll> mp;
    ll ans = 0;
    for(int i = 0; i < n; ++i) {
        cin >> a[i];
        vector<pair<ll, ll> > vt;
        for(int j = 2; j <= a[i] / j; ++j) {
            int num = 0;
            if(a[i] % j == 0) {
                while(a[i] % j == 0) {
                    ++num;
                    a[i] /= j;
                }
            }
            if(num % k) {
                vt.push_back({j, num % k});
            }
        }
        if(a[i] > 1) vt.push_back({a[i], 1 % k});
        vector<pair<ll, ll> > vt2;
        for(int j = 0; j < vt.size(); ++j) {
            vt2.push_back({vt[j].first, k - vt[j].second});
        }
        ans += mp[vt2];
        mp[vt]++;
    }
    cout << ans << endl;
    return 0;
}
```