---
title: Google Kick Start Round A 2020
date: 2020-03-24 21:20:35
tags:
- ACM刷题
- 贪心
- 排序
- 动态规划
- 前缀和
- 二分
- Trie/字典树/前缀树
- DFS
categories: 竞赛
mathjax: true
---

比赛链接：[kick start Round A 2020](https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ffc7)

## A. Allocation

[题目链接](https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ffc7/00000000001d3f56)

### 题意

给出 $N$ 栋房子的价格，第 $i$ 栋房子的价格为 $A_i$，你有 $B$ 美元，问最多可以买多少栋房子？

### 思路

典型的贪心问题，将所有的房子按价格从低到高排序后选取即可。

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
    int T;
    cin >> T;
    int kase = 0;
    while(T--) {
        int n, b;
        cin >> n >> b;
        for(int i = 1; i <= n; ++i) {
            cin >> a[i];
        }
        ll ans = 0;
        sort(a + 1, a + 1 + n);
        for(int i = 1; i <= n; ++i) {
            if(b < a[i]) {
                break;
            } else {
                ++ans;
                b -= a[i];
            }
        }
        cout << "Case #" << ++kase << ": ";
        cout << ans << endl;
    }
    return 0;
}
```

## B. Plates

[题目链接](https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ffc7/00000000001d40bb)

### 题意

有 $N$ 叠盘子，每叠有 $K$ 个盘子，每个盘子有个 *beauty value*，现在要拿 $P$ 个盘子，使得 *beauty value* 之和最大。拿盘子的条件：如果一个盘子的上面的盘子都被拿走了，才能拿到这个盘子。

### 思路

看完题目就想到是动态规划的题了，但是还是忍不住先暴搜试试能不能水过小数据，结果 WA 了，于是只好老老实实写 DP。

本题有一点点像多重背包。首先计算每叠盘子各自的前缀和 $sum[N][K]$，设 $dp[i][j]$ 表示前 $i$ 堆盘子中取 $j$ 个盘子的 *beauty value* 的最大值，转移方程为：
$$dp[i][j]=max\{dp[i][j], dp[i - 1][j - l] + sum[i][l]|for\ l\ \in[0, min(j, k)]\}$$

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1510;

int n, k, p;
ll a[maxn][maxn];
ll sum[maxn][maxn];
ll dp[maxn][maxn];


int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    int kase = 0;
    while(T--) {
        for(int i = 0; i < maxn; ++i) {
            for(int j = 0; j < maxn; ++j) {
                sum[i][j] = 0;
                a[i][j] = 0;
                dp[i][j] = 0;
            }
        }
        cin >> n >> k >> p;
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= k; ++j) {
                cin >> a[i][j];
            }
        }
        cout << "Case #" << ++kase << ": ";
        for(int i = 1; i <= n; ++i) {
            for(int j = 1; j <= k; ++j) {
                sum[i][j] = sum[i][j - 1] + a[i][j];
            }
        }
        for(int i = 0; i <= k; ++i) {
            dp[1][i] = sum[1][i];
        }
        for(int i = 2; i <= n; ++i) {
            for(int j = 1; j <= i * k; ++j) {  // 我这里多算了，实际上到 p 就行了
                for(int l = 0; l <= min(j, k); ++l) {
                    dp[i][j] = max(dp[i][j], dp[i - 1][j - l] + sum[i][l]);
                }
            }
        }
        cout << dp[n][p] << endl;
    }
    return 0;
}
```

## C. Workout

[题目链接](https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ffc7/00000000001d3f5b)

### 题意

给定 $N$ 个严格递增的数，往这 $N$ 个数中间插 $K$ 个数，插入后要使所有的数仍然保持严格递增，而且要保证相邻两数的最大绝对值之差最小，求最小的绝对值之差。

### 思路

我一开始想的是贪心+优先队列，先计算所有相邻两数的差值并加入到优先队列中，然后每次取出队首 $x$，将它分成 $x / 2$ 和 $x - x / 2$ 两部分，并将这两部分加入到优先队列中，操作 $K$ 次后的队首就是答案。提交后小数据过了，大数据 $WA$ 了。仔细一看小数据是 $K=1$。然后找到了反例，如 $K=4$，原数组是 $[10, 20]$，那么正确结果是 $[10, 12, 14, 16, 18, 20]$，而采用本方法得到的结果是 $[10, 12, 13, 15, 17, 20]$。

最后用的是二分答案过的，典型的最大值最小的问题。判断函数的思路：传入参数 $x$，表示答案为 $x$，然后遍历每个数 $num[i]$，如果 $num[i + 1] - num[i] > x$，就插入 $num[i] + x$，如果插入的数的个数大于 $K$ 个就返回 $false$，否则返回 $true$。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e5 + 10;
const int inf = 0x3f3f3f3f;

int n, k;
ll m[maxn];
ll tmp[maxn];

bool judge(int x) {
    int cnt = 0;
    for(int i = 0; i < n - 1; ++i) {
        while(tmp[i + 1] - tmp[i] > x) {
            tmp[i] += x;
            ++cnt;
        }
    }
    if(cnt > k) return 0;
    else return 1;
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    int kase = 0;
    while(T--) {
        cin >> n >> k;
        for(int i = 0; i < n; ++i) {
            cin >> m[i];
        }
        cout << "Case #" << ++kase << ": ";
        int l = 1, r = inf;
        while(l < r) {
            int mid = (l + r) >> 1;
            for(int i = 0; i < n; ++i) {
                tmp[i] = m[i];
            }
            if(judge(mid)) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        cout << r << endl;
    }
    return 0;
}
```

## D. Bundling

[题目链接](https://codingcompetitions.withgoogle.com/kickstart/round/000000000019ffc7/00000000001d3ff3)

### 题意

给定 $N$ 个字符串，把它们分组，每组 $K$ 个。每组的分数是该组所有字符串的最长公共前缀。求最大的所有组的分数和。

### 思路

很容易想到前缀树 (字典树 / trie 树)，建完树后从根节点 $dfs$，同时记录深度 $d$，然后从叶子节点回溯，统计每个节点出现的个数 $cnt$，如果某个节点 $u$ 的 $cnt[u] ≥ K$，那么说明有 $K$ 个字符串的前缀是以该节点结尾，深度 $d$ 表示它们的前缀的长度，由于是从叶子节点回溯的，所以一定是最长公共前缀，所以 $ans = ans + d$，同时 $cnt[u]$ 减去 $k$，即这 $k$ 个字符串已经分完组，不再分到其他组。

### 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e5 + 10;
const int inf = 0x3f3f3f3f;

int trie[maxn << 2][30], tot = 1;
int cnt[maxn << 2];
int n, k;
ll ans;

void init() {
    memset(trie, 0, sizeof(trie));
    memset(cnt, 0, sizeof(cnt));
    tot = 1;
    ans = 0;
}

void insert(string s) {
    int p = 0;
    for(char ch: s) {
        int id = ch - 'A';
        if(!trie[p][id]) {
            trie[p][id] = tot++;
        }
        p = trie[p][id];
    }
    ++cnt[p];
}

void dfs(int u, int d) {
    for(int v = 0; v < 26; ++v) {
        if(trie[u][v]) {
            dfs(trie[u][v], d + 1);
            cnt[u] += cnt[trie[u][v]];
        }
    }
    while(cnt[u] >= k) {
        cnt[u] -= k;
        ans += d;
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    int kase = 0;
    while(T--) {
        init();
        cout << "Case #" << ++kase << ": ";
        cin >> n >> k;
        for(int i = 0; i < n; ++i) {
            string s;
            cin >> s;
            insert(s);
        }
        dfs(0, 0);
        cout << ans << endl;
    }
    return 0;
}
```