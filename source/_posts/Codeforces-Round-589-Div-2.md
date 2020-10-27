---
title: 'Codeforces Round #589 (Div. 2)'
date: 2019-10-09 23:02:30
tags:
- ACM刷题
- 暴力
- 模拟
- 素数
- 数论
- 哈希/Hash
- 思维
categories: 竞赛
mathjax: true
---

比赛链接：[Codeforces Round #589 (Div. 2)](https://codeforces.com/contest/1228)

官方题解：[Codeforces Round #589 (Div. 2) Editorial](https://codeforces.com/blog/entry/70162)

<!--more-->

## A. Distinct Digits

### 题意

给定两个整数 $l$ 和 $r$，求一个 $x$ 满足 $l \le x \le r$ 且 $x$ 的每一位都不同。

### 思路

直接暴力。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;

bool judge(int x) {
    int m[100] = {0};
    while(x) {
        if(m[x % 10]) {
            return false;
        }
        m[x % 10]++;
        x /= 10;
    }
    return true;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int l, r;
    while(cin >> l >> r) {
        int f = 0;
        for(int i = l; i <= r; ++i) {
            if(judge(i)) {
                f = 1;
                cout << i << endl;
                break;
            }
        }
        if(f == 0) cout << -1 << endl;
    }
    return 0;
}
```

## B. Filling the Grid

### 题意

给定一个 $h \times w$ 的矩形，以及 $h$ 个 $r$ 和 $w$ 个 $c$。

$r_i$ 表示第 $i$ 行左边界从左往右连续的黑色格子的是 $r_i$ 个。

$c_i$ 表示第 $i$ 列上边界从上往下连续的黑色格子的是 $c_i$ 个。

给出 $h, w, r[], c[]$，求可以构造出多少种矩形满足条件。

### 思路

**模拟**

对每行每列模拟，填充黑色格子和白色格子（黑色格子旁边一个格子一定是白色），如果行列有冲突就输出零，否则找出所有的没填充的格子的个数 $cnt$，答案为 $2 ^ {cnt} \mod 10^9 + 7$。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll mod = 1e9 + 7;
const ll maxn = 1e3 + 10;

ll c[maxn], r[maxn];
ll g[maxn][maxn];

ll qmod(ll a, ll b, ll m) {
    a %= m;
    ll res = 1;
    while (b > 0) {
        if (b & 1) res = res * a % m;
        a = a * a % m;
        b >>= 1;
    }
    return res;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    ll h, w;
    cin >> h >> w;
    for(ll i = 1; i <= h; ++i) {
        cin >> r[i];
        for(ll j = 1; j <= r[i]; ++j) {
            g[i][j] = 1;
        }
        g[i][r[i] + 1] = 2;
    }
    ll f = 1;
    for(ll i = 1; i <= w; ++i) {
        cin >> c[i];
        for(ll j = 1; j <= c[i]; ++j) {
            if(g[j][i] == 2) {
                f = 0;
            }
            g[j][i] = 1;
        }
        if(g[c[i] + 1][i] == 1) {
            f = 0;
        }
        g[c[i] + 1][i] = 2;
    }
    if(f == 0) {
        cout << 0 << endl;
    } else {
        ll ans = 0;
        for(ll i = 1; i <= h; ++i) {
            for(ll j = 1; j <= w; ++j) {
                if(!g[i][j]) {
                    ++ans;
                }
            }
        }
        cout << qmod(2, ans, mod) << endl;
    }
    return 0;
}
```

## C. Primes and Multiplication

### 题意

$g(x, p)$ 定义为最大的 $p ^ k$ 满足 $p^k$ 整除 $x$。

$f(x, y)$ 定义为所有 $g(y, p)$ 的乘积，其中 $p$ 是 $x$ 的质因数。

给定 $x$ 和 $n$，求 $\prod_{i=1}^n f(x,i) \mod \ (10^9 + 7)$。

### 思路

理解了 $g(x, p)$ 的含义就容易做了。

$g(x, p)$ 其实为 $x$ 分解质因数后 $p$ 出现的次数。

先求出 $x$ 的所有质因数。然后对于每个质因数，求出 $1$ 到 $n$ 中这个质因数出现的次数。也就是求 $n!$ 的这个质因数的个数。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll mod = 1e9 + 7;

vector<ll> divide(ll x) {
    vector<ll> ans;
    for(int i = 2; i <= x / i; ++i) {
        if (x % i == 0) {
            ans.push_back(i);
            while (x % i == 0) x /= i;
        }
    }
    if (x > 1) ans.push_back(x);
    return ans;
}

ll qmod(ll a, ll b, ll m) {
    if(!b) return 1 % m;
    ll ans = 1;
    while(b) {
        if(b & 1) ans = (ans * a) % m;
        a = (a * a) % m;
        b >>= 1;
    }
    return ans;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    ll x, n;
    cin >> x >> n;
    vector<ll> p = divide(x);
    ll ans = 1;
    for(int i = 0; i < p.size(); ++i) {
        ll tmp = n;
        while(tmp >= p[i]) {
            ans = ans * qmod(p[i], tmp / p[i], mod) % mod;
            tmp /= p[i];
        }
    }
    cout << ans % mod << endl;
    return 0;
}
```

## D. Complete Tripartite

### 题意

给定一个无向无环图。问能否将点集分成 $3$ 个，使得每一个点集内的任意两点没有边，每一个点集内的点与其他所有点集内的所有点都有一条边。

### 思路

**哈希**

特殊的三分图。

考虑到一个集合内的所有点都与其他两个集合内的所有点有边，也就是一个集合内的每个点的所有边连出去的点都是相同的，根据这个哈希，如果哈希值有 $3$ 个，就按哈希值分类即可。

我的做法是把所有的点赋一个互不相同的正整数值。每个点的哈希值 $h[i]$ 为它的所有边的另外一个点的值的和。

注意判断一下哈希值为 $0$ 的情况。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 3e5 + 10;
map <ll, vector<ll>> mp;
ll e[maxn], ans[maxn], h[maxn];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    mt19937 rnd(time(0));
    ll n, m;
    cin >> n >> m;
    unordered_map<ll, int> ump;
    for(int i = 1; i <= n; ++i) {
        h[i] = rnd();
        while(ump[h[i]] || h[i] == 0) {
            h[i] = rnd();
        }
        ump[h[i]]++;
    }
    for(int i = 0; i < m; ++i) {
        ll a, b;
        cin >> a >> b;
        e[a] += h[b];
        e[b] += h[a];
    }
    int f = 1;
    for(int i = 1; i <= n; ++i) {
        if(e[i]) {
            mp[e[i]].push_back(i);
        } else {
            f = 0;
        }
    }
    if(mp.size() != 3 || f == 0) {
        cout << -1 << endl;
    } else {
        ll x = 0;
        for(auto it = mp.begin(); it != mp.end(); ++it) {
            ++x;
            for(int i = 0; i < (it->second).size(); ++i) {
                ans[(it->second)[i]] = x;
            }
        }
        for(int i = 1; i <= n; ++i) {
            cout << ans[i] << " ";
        }
        cout << endl;
    }
    return 0;
}
```
