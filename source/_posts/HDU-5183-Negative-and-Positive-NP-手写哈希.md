---
title: HDU 5183 Negative and Positive (NP)  (手写哈希)
date: 2019-11-08 22:30:13
tags:
- ACM刷题
- 哈希/Hash
- 前缀和
categories: 竞赛
mathjax: true
---

题目链接：[HDU 5183](http://acm.hdu.edu.cn/showproblem.php?pid=5183)

## Problem Description

> When given an array $(a_0,a_1,a_2,⋯a_{n−1})$ and an integer $K$, you are expected to judge whether there is a pair $(i,j)(0≤i≤j<n)$ which makes that $NP−sum(i,j)$ equals to $K$ true. Here $NP−sum(i,j)=a_i−a_{i+1}+a_{i+2}+⋯+(−1)^{j−i}a_j$

<!--more-->

## Input

> Multi test cases. In the first line of the input file there is an integer $T$ indicates the number of test cases.
> 
> In the next $2∗T$ lines, it will list the data for each test case.
> 
> Each case occupies two lines, the first line contain two integers $n$ and $K$ which are mentioned above.
> 
> The second line contain $(a_0,a_1,a_2,⋯a_{n−1})$ separated by exact one space.
> 
> [Technical Specification]
> 
> All input items are integers.
> 
> $0<T≤25,1≤n≤1000000,−1000000000≤a_i≤1000000000,−1000000000≤K≤1000000000$

## Output

> For each case，the output should occupies exactly one line. The output format is Case #id: ans, here id is the data number starting from 1; ans is “Yes.” or “No.” (without quote) according to whether you can find $(i,j)$ which makes $PN−sum(i,j)$ equals to $K$.
> 
> See the sample for more details.


## Sample Input

```markdown
2
1 1
1
2 1
-1 0
```

## Sample Output

```markdown
Case #1: Yes.
Case #2: No.
```

## Hint

> If input is huge, fast IO method is recommended.
 
## Source

> [BestCoder Round #32](http://acm.hdu.edu.cn/search.php?field=problem&key=BestCoder+Round+%2332&source=1&searchmode=source)

## Solution

### 题意

给定长度为 $n$ 的数组，问是否存在一段区间其加减交错的和等于 $k$。

### 思路

对该数组求前缀和然后哈希就行。

不过这题不同的 set 过不了。

所以要手写哈希。

当然 unordered_set 也可以过。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e6 + 7;

struct HashMap {
    int head[maxn], next[maxn];
    ll num[maxn];
    int tot;
    inline void init() {
        tot = 0;
        memset(head, -1, sizeof(head));
    }
    inline void insert(ll val) {
        int h = abs(val) % maxn;
        num[tot] = val, next[tot] = head[h], head[h] = tot++;
    }
    inline bool find(ll val) {
        int h = abs(val) % maxn;
        for(int i = head[h]; ~i; i = next[i]) {
            if(num[i] == val) {
                return true;
            }
        }
        return false;
    }
} hashmap;

ll arr[maxn], sum[maxn];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    int T;
    cin >> T;
    int kase = 0;
    while(T--) {
        hashmap.init();
        ll n, k;
        cin >> n >> k;
        for(int i = 1; i <= n; ++i) {
            cin >> arr[i];
            sum[i] = sum[i - 1] + (i & 1? arr[i]: -arr[i]);
        }
        int flag = 0;
        for(int i = n; i; --i) {
            hashmap.insert(sum[i]);
            if(i & 1) {
                if(hashmap.find(sum[i - 1] + k)) {
                    flag = 1;
                    break;
                }
            } else {
                if(hashmap.find(sum[i - 1] - k)) {
                    flag = 1;
                    break;
                }
            }
        }
        cout << "Case #" << ++kase << ": " << (flag? "Yes." : "No.") << endl;
    }
    return 0;
}
```