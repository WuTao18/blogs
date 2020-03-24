---
title: HDU 3949 XOR (线性基)
date: 2019-08-28 14:06:58
tags:
- ACM刷题
- 线性基
categories: 竞赛
mathjax: true
---

题目链接：[HDU 3949](http://acm.hdu.edu.cn/showproblem.php?pid=3949)

## Problem Description
> XOR is a kind of bit operator, we define that as follow: for two binary base number A and B, let C=A XOR B, then for each bit of C, we can get its value by check the digit of corresponding position in A and B. And for each digit, 1 XOR 1 = 0, 1 XOR 0 = 1, 0 XOR 1 = 1, 0 XOR 0 = 0. And we simply write this operator as ^, like 3 ^ 1 = 2,4 ^ 3 = 7. XOR is an amazing operator and this is a question about XOR. We can choose several numbers and do XOR operatorion to them one by one, then we get another number. For example, if we choose 2,3 and 4, we can get 2^3^4=5. Now, you are given N numbers, and you can choose some of them(even a single number) to do XOR on them, and you can get many different numbers. Now I want you tell me which number is the K-th smallest number among them.  
 

## Input
> First line of the input is a single integer T(T<=30), indicates there are T test cases. 
> 
> For each test case, the first line is an integer N(1<=N<=10000), the number of numbers below. The second line contains N integers (each number is between 1 and 10^18). The third line is a number Q(1<=Q<=10000), the number of queries. The fourth line contains Q numbers(each number is between 1 and 10^18) K1,K2,......KQ.  
 

## Output
> For each test case,first output Case #C: in a single line,C means the number of the test case which is from 1 to T. Then for each query, you should output a single line contains the Ki-th smallest number in them, if there are less than Ki different numbers, output -1.
 

## Sample Input

```markdown
2
2
1 2
4
1 2 3 4
3
1 2 3
5
1 2 3 4 5
```

## Sample Output

```markdown
Case #1:
1
2
3
-1
Case #2:
0
1
2
3
-1
```

## Hint

> If you choose a single number, the result you get is the number you choose.
> 
> Using long long instead of int because of the result may exceed 2^31-1.

## Solution

### 题意

给定 $n$ 个数，$q$ 个询问。

每个询问包含一个正整数 $k$。在 $n$ 个数中选择若干个数求异或和。求所有异或和中第 $k$ 小的异或和。

### 题解

**线性基**

首先，原数集 $S$ 的所有异或和与线性基 $P$ 的所有异或和是相同的。所有的异或和就是线性基中所有的基的线性组合。

构造一个特殊的线性基，使得线性基中每一个行向量都是最小的。也就是每个行向量的最高位的 $1$ 所在的列向量只有一个 $1$。此时所有异或和不变。

求第 $k$ 小的数，把 $k$ 转成二进制，第 $i$ 位为 $1$ 表示取第 $i$ 小的行向量。

**注**：异或和的个数有两种情况。异或和包含 $0$ 和不包含 $0$。如果两个向量线性相关，则一定可以通过异或得到 $0$。而线性基是极大线性无关组，若 $|S| = |P|$，则不能得到 $0$，否则一定可以。若可以得到 $0$，则有 $2^{|P|}$ 个异或和，否则有 $2^{|P|} - 1$ 个异或和。

**举个例子**

假设 $S = {1, 5, 8, 9, 20}$

线性基为

```
1 0 1 0 0
0 1 0 0 0
0 0 1 0 1
0 0 0 0 1
```

构造上述的特殊线性基

```
1 0 0 0 0
0 1 0 0 0
0 0 1 0 0
0 0 0 0 1
```

共有 $2^{4} = 16$ 个异或和 (包含 $0$)。

当 $k = 1$ 时，二进制为 $(00001)_2$，取第一小的行向量，即最后一行，因此答案为 $(00001)_2$。

当 $k = 2$ 时，二进制为 $(00010)_2$，取第二小的行向量，即第三行，因此答案为 $(00100)_2$。

当 $k = 3$ 时，二进制为 $(00011)_2$，取第一小的和第二小的行向量，即最后两行，因此答案为 $(00001)_2\ xor\ (00100)_2 = (00101)_2$。

...

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
using ll = long long;
const int maxn = 5e5 + 5;
const int maxbit = 63;

ll p[maxbit];
ll tmp[maxbit];

void add(ll x) {
    for(int i = maxbit - 1; i >= 0; --i) {
        if((x >> i) & 1) {
            if(p[i] == 0) {
                p[i] = x;
                break;
            }
            x ^= p[i];
        }
    }
}

int main() {
    std::ios::sync_with_stdio(false);
    int T;
    cin >> T;
    for(int _ = 1; _ <= T; ++_) {
        cout << "Case #" << _ << ":" << endl;
        memset(p, 0, sizeof(p));
        int n;
        cin >> n;
        for(int i = 1; i <= n; ++i) {
            ll x;
            cin >> x;
            add(x);
        }

        for(int i = maxbit - 1; i >= 0; --i) {
            for(int j = i + 1; j < maxbit; ++j) {
                if((p[j] >> i) & 1) {
                    p[j] ^= p[i];
                }
            }
        }

        int cnt = 0;
        for(int i = 0; i < maxbit; ++i) {
            if(p[i]) {
                tmp[cnt++] = p[i];
            }
        }

        int m;
        cin >> m;
        while(m--) {
            ll k;
            cin >> k;
            // 判断异或和能否为 0 
            if(cnt != n) {
                --k;
            }
            // 判断 k 是否超过异或和能表示的数量
            if(k >= (1LL << cnt)) {
                cout << -1 << endl;
                continue;
            }
            ll ans = 0;
            for(int i = 0; i < maxbit; ++i) {
                if((k>>i) & 1) {
                    ans ^= tmp[i];
                }
            }
            cout << ans << endl;
        }
    }
    
    return 0;
}
```
