---
title: 'Codeforces Round #572 (Div. 2)'
date: 2019-08-31 21:15:14
tags:
- ACM刷题
- 思维
- 数学
- 数论
- 树
- 数据结构
categories: 竞赛
mathjax: true
---

比赛链接：[Codeforces Round #572 (Div. 2)](https://codeforces.com/contest/1189)

官方题解：[Editorial of Codeforces Round #572](https://codeforces.com/blog/entry/68079)

<!--more-->

## A. Keanu Reeves

### 题意

如果一个 $01$ 序列中 $0$ 的数量和 $1$ 的数量不同，就认为这个序列是“好”的。

给定一个长度为 $n$ 的 $01$ 序列，将这个序列分割成多个“好”的序列，求最少分割次数和分割后的所有序列。

### 思路

如果原序列 $0$ 和 $1$ 不同就不用分割。

否则分割成两个串，第一串为第一个数，剩下的为第二串。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const double eps = 1e-8;
const int inf = 0x3f3f3f3f;
const int maxn = 100000 + 5;

string s;

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin >> n;
    cin >> s;
    if(n % 2) {
        cout << 1 << endl;
        cout << s << endl;
        return 0;
    }
    int one = 0;
    for(int i = 0; i < n; ++i) {
        if(s[i] == '1') ++one;
    }
    if(one * 2 != n) {
        cout << 1 << endl;
        cout << s << endl; 
    } else {
        cout << 2 << endl;
        cout << s.substr(0, n - 1) << " " << s[n - 1] << endl;
    }
    return 0;
}
```

## B. Number Circle

### 题意

给定 $n$ 个数字，将所有数字构成一个环，放置顺序任意，问是否存在一种放置顺序，使得每个数字小于左右两个数字的和。

### 思路

对所有数字排序，如果 $a_n \ge a_{n - 1} + a_{n - 2}$，那么答案是 $NO$，因为这样 $a_n$ 大于等于任意两个数的和。

否则答案一定存在，只要交换最后两个数即可，因为 $a_i < a_{i + 1} < a_{i - 1} + a_{i + 1}\ (i < n)$ 。(构造方法不唯一)

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e5 + 10;

int a[maxn];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin >> n;
    for(int i = 0; i < n; ++i) {
        cin >> a[i];
    }
    sort(a, a + n);
    if(a[n - 1] >= a[n - 2] + a[n - 3]) {
        cout << "NO" << endl;
    } else {
        cout << "YES" << endl;
        swap(a[n - 1], a[n - 2]);
        for(int i = 0; i < n; ++i) {
            cout << a[i] << " ";
        }
        cout << endl;
    }
    return 0;
}
```

## C. Candies!

### 题意

定义 $f([a_1, a_2, \ldots, a_{2^k}])$ 为每次取两个相邻的数 $a_i$ 和 $a_{i + 1}$，将这两个数变成一个数 $(a_i + a_{i + 1})\ mod\ 10$，经过若干次同样操作直到序列长度为 $1$ 为止。其中每次操作时如果 $a_i + a_{i + 1} \ge 10$ 就能获得一颗糖果。

现在给定一个长度为 $n$ 的序列 $s$，以及 $q$ 个询问。每个询问包含 $l$ 和 $r$，求 $f([s_{l}, s_{l + 1}, \ldots, s_{r}])$。

### 思路

每次合并是相加取模，所以每次序列都不会超过 $10$，$f([a_1, a_2, \ldots, a_{2^k}]) = \lfloor \frac{a_1 + a_2 + \ldots + a_{2^k}}{10} \rfloor$。

用前缀和处理即可。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e5 + 5;

int s[maxn];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin >> n;
    for(int i = 1; i <= n; ++i) {
        cin >> s[i];
        s[i] += s[i - 1];
    }
    int q;
    cin >> q;
    for(int i = 0; i < q; ++i) {
        int l, r;
        cin >> l >> r;
        cout << (s[r] - s[l - 1]) / 10 << endl;
    }
    return 0;
}
```

## D1. Add on a Tree

### 题意

给定一颗树，初始每条边的权值为 $0$。可以任选两个叶子结点，给这两个结点的简单路径的所有边加上任意实数，问是否可以通过有限次操作使得树上的所有边达到任意值。

### 思路

如果有度为 $2$ 的结点就不能。因为度为 $2$ 的结点的两条边经过任意次操作后权值都相等。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 100000 + 5;

int degree[maxn];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int n;
    cin >> n;
    for(int i = 0; i < n - 1; ++i) {
        int u, v;
        cin >> u >> v;
        degree[u]++; degree[v]++;
    }
    for(int i = 1; i <= n; ++i) {
        if(degree[i] == 2) {
            cout << "NO" << endl;
            return 0;
        }
    }
    cout << "YES" << endl;
    return 0;
}
```

## D2. Add on a Tree: Revolution

待补

## E.Count Pairs

### 题意

给定 $n$ 个数 $a_1, a_2, ..., a_n$ ，求满足 $(a_i+a_j)(a_i^2+a_j^2)\equiv k\ mod\ p$ 的 $(i, j)(1 \le i < j \le n)$ 的数量。其中 $p$ 是素数，且 $n$ 个数都不同。

### 思路

$$(a_i+a_j)(a_i^2+a_j^2)\equiv k\ mod\ p$$

$$(a_i-a_j)(a_i+a_j)(a_i^2+a_j^2)\equiv (a_i-a_j)k\ mod\ p$$

$$(a_i^2-a_j^2)(a_i^2+a_j^2)\equiv (a_i-a_j)k\ mod\ p$$

$$(a_i^4-a_j^4)\equiv (a_i-a_j)k\ mod\ p$$

$$a_i^4-ka_i\equiv a_j^4-ka_j\ (mod\ p)$$

统计一下有多少对 $(a_i^4-ka_i)\ mod\ p$ 是相同的。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const double eps = 1e-8;
const int inf = 0x3f3f3f3f;
const int maxn = 3e5 + 5;

ll a[maxn];

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
    int n;
    ll p, k;
    cin >> n >> p >> k;
    map<ll, int> b;
    for(int i = 0; i < n; ++i) {
        cin >> a[i];
        ll tmp = (qmod(a[i], 4, p) - k * a[i]);
        tmp += p * (abs(tmp) / p + 1);
        b[tmp % p]++;
        // cout << tmp << endl;
    }
    ll sum = 0;
    for(auto it = b.begin(); it != b.end(); ++it) {
        if(it->second > 1) sum += (it->second) * (it->second - 1) / 2;
    }
    cout << sum << endl;
    return 0;
}
```

## F.Array Beauty

待补