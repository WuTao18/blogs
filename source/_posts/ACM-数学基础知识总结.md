---
title: ACM 数学基础知识总结
date: 2019-08-08 22:58:02
tags:
- ACM学习
- 数论
- 数学
password: 123456
categories: 竞赛
mathjax: true
---

## 主要内容
- 同余
- 快速幂
- 快速乘
- 欧拉函数
- 欧拉定理
- 费马定理
- 乘法逆元
- 扩展欧几里得算法


## 同余的性质

$$(a + b)\ mod\ n = ((a\ mod\ n)\ + \ (b\ mod\ n))\ mod\ n$$
$$(a - b)\ mod\ n = ((a\ mod\ n)\ - \ (b\ mod\ n) + n)\ mod\ n$$
$$(a * b)\ mod\ n = ((a\ mod\ n)\ * \ (b\ mod\ n))\ mod\ n$$
$$a ^ b\ mod\ n = (a\ mod\ n)^{b}\ mod\ n$$

## 快速幂

快速幂用于求解 $a ^ n\ mod\ m$，时间复杂度 $O(logn)$。

$n$ 用二进制表示有 $\lceil log_2(n + 1) \rceil$ 位，令 $t = \lceil log_2(n + 1) \rceil$，则有：

$$n = n_{t-1}2^{t-1} + n_{t-2}2^{t-2} + ... + n_02^0\ (n_i \in \{0, 1\})$$

于是：

$$a ^ n \equiv a ^ {n_{t-1}2^{t-1} + n_{t-2}2^{t-2} + ... + n_02^0}(mod\ m)$$
$$a ^ n \equiv a ^ {n_{0}2^{0}} \times a ^ {n_{1}2^{1}} \times ... \times a ^ {n_{t-1}2^{t-1}}(mod\ m)$$

其中：

$$a ^ {2^i} = (a ^ {2 ^ {i - 1}}) ^ 2$$

```cpp
typedef long long ll;
ll quick_mod(ll a, ll n, ll m) {
    if(!n)
        return 1 % m;

    ll res = 1;
    while (n) {
        if(n & 1) {  // 二进制最后一位为 1是奇数
            res = (res * a) % m;
        }
        a = (a * a) % m; 
        n >>= 1;  // 右移一位就是整除 2
    }
    return res;
}
```

## 快速乘

快速幂用于求解 $(a * b)\ mod\ m$，思想与快速幂一致，时间复杂度 $O(logn)$。

$b$ 用二进制表示：

$$b = b_{t-1}2^{t-1} + b_{t-2}2^{t-2} + ... + b_02^0 \ (b_i \in \{0, 1\})$$


其中 $t = \lceil log_2(n + 1) \rceil$.


于是：

$$a * b \equiv n_{0}*a*{2^{0}} + n_{1}*a*{2^{1}} + ... + n_{t-1}*a*{2^{t-1}}(mod\ m)$$

其中：

$$a * {2^i} = (a * {2 ^ {i - 1}}) * 2$$

```cpp
typedef long long ll;
ll quick_mod(ll a, ll b, ll m) {
    ll res = 0;
    while (b) {
        if(b & 1) {  // 二进制最后一位为 1是奇数
            res = (res + a) % m;
        }
        a = (a + a) % m; 
        b >>= 1;  // 右移一位就是整除 2
    }
    return res;
}
```

## 质数

### 算术基本定理

[算术基本定理](https://zh.wikipedia.org/wiki/%E7%AE%97%E6%9C%AF%E5%9F%BA%E6%9C%AC%E5%AE%9A%E7%90%86)，又称正整数唯一分解定理，即每一个正整数都可以被分解成若干质数的乘积，且分解的方式唯一。

$\forall A \in \mathbb{N}, A > 1,\ ∃ ∏_{i=1}^{n} p_{i}^{a_i} = A$，其中 $p_1 < p_2 < ... < p_n$ 且 $p_i$ 是质数，$a_i \in \mathbb{Z^+}.$

### 质数的判定 —— 试除法

```cpp
bool is_prime(int n) {
    for(int i = 2; i <= n / i; ++i) {
        if(n % i == 0) return false;
    }
    return true;
}
```

## 约数

### 求一个数的所有约数 —— 试除法

如果 $d | n$，那么 $\frac{n}{d} | n$。令 $d≤\frac{n}{d}$，则 $d\le √n$ 。总体时间复杂度 $O(\sqrt n)$

```cpp
vector<int> get_divisors(int n) {
    vector<int> res;

    for(int i = 1; i <= n / i; ++i) {
        if(n % i == 0) {
            res.push_back(i);
            if(i != n / i) res.push_back(n / i); // 如果 i 是 n 的约数，那么 n / i 也是 n 的约数
        }
    }
    sort(res.begin(), res.end());
    return res;
}
```

### 约数个数

[约数个数定理](https://baike.baidu.com/item/%E7%BA%A6%E6%95%B0%E4%B8%AA%E6%95%B0%E5%AE%9A%E7%90%86)

一个大于 $1$ 的正整数 $n=p_1 ^ {a_1}\cdot p_2 ^ {a_2}\cdot ... \cdot p_k ^ {a_k}$，
则 $n$ 的正约数的个数为 $f(n) = (a_1 + 1)(a_2 + 1) ... (a_k + 1)$

### 约数之和

[约数和定理](https://baike.baidu.com/item/%E7%BA%A6%E6%95%B0%E5%92%8C%E5%AE%9A%E7%90%86/3808428)

一个大于 $1$ 的正整数 $n=p_1 ^ {a_1}\cdot p_2 ^ {a_2}\cdot ... \cdot p_k ^ {a_k}$，
则 $n$ 的正约数的个数为 $f(n) = (p_1^0 + p_1^1 + ... + p_1^{a_1})(p_2^0 + p_2^1 + ... + p_2^{a_2}) ... (p_k^0 + p_k^1 + ... + p_k^{a_k})$

## 欧拉函数

### 定义

$\varphi(n)$  $1 \sim n$ 中于 $n$ 互质的个数

**例：** $\varphi(6) = 2$

**求法：**

$$N = p_1^{\alpha_1} \cdot p_2^{\alpha_2} \cdot... \cdot p_k^{\alpha_k}$$

$$\varphi(N) = N(1-\frac1{p_1})(1-\frac1{p_2})...(1-\frac1{p_k}) \tag{1.1}$$

**例：** 

$N = 6 = 2 * 3$

$\varphi(6) = 6(1-\frac1{2})(1-\frac1{3}) = 2$

**证明：**

容斥原理

$1 \sim N$ 中和 $N$ 互质的个数

1. 从 $1 \sim N$ 中去掉 $p_1, p_2, ... , p_k$ 的倍数。 

$$N-\frac{N}{p_1}-\frac{N}{p_2}-...-\frac{N}{p_k}$$

2. 加上所有 $p_i \times p_j$ 的倍数 (所有 $p_i \times p_j$ 的倍数被去掉了两次) 

$$N-\frac{N}{p_1}-\frac{N}{p_2}-...-\frac{N}{p_k} + \frac{N}{p_1 * p_2} + \frac{N}{p_1 * p_3} + ...$$

3. 减去所有 $p_i \times p_j \times p_k$ 的倍数 

$$N-\frac{N}{p_1}-\frac{N}{p_2}-...-\frac{N}{p_k} \\ + \frac{N}{p_1 * p_2} + \frac{N}{p_1 * p_3} + ... \\ - \frac{N}{p_1 * p_2 * p_3} - \frac{N}{p_1 * p_2 * p_4} - ...  \tag{1.2}$$

4. 重复上述步骤

把公式 $(1.1)$ 展开就是公式 $(1.2)$

### 用定义求欧拉函数

时间复杂度 $O(\sqrt{N})$

```cpp
// 用定义求欧拉函数
// O(sqrt(N))
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    while(n--) {
        int a;
        cin >> a;
        int res = a;
        for(int i = 2; i <= a / i; i++) {
            if(a % i == 0) {
                // res = res * (1 - 1 / i);
                res = res / i * (i - 1);
                while(a % i == 0) a /= i;
            }
        }
        if(a > 1) res = res / a * (a - 1);
    }
}
```

### 筛法求欧拉函数

$1 \sim N$ 中每一个数的欧拉函数

用定义求的时间复杂度 $O(N\sqrt {(N)})$

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

typedef long long ll;

const int N = 1000000 + 10;

int prime[N], cnt;
int phi[N];
bool st[N];

ll get_eulers(int n) {
    for(int i = 2; i <= n; ++i) {
        if(!st[i]) {
            primes[cnt++] = i;
            phi[i] = i - 1;  // 小于质数 p 的所有数都与 p 互质
        }
        for(int j = 0; primes[j] <= n / i; ++j) {
            st[primes[j] * i] = 1;
            if(i % primes[j] == 0) {  // pj 是 i 的质因子
                // phi(pj * i)  phi(i)
                // pj * i 的所有质因子与 i 的所有质因子相同
                // phi(i) = i * (1 - 1 / p1) ... (1 - 1 / pk)
                // phi(pj * i) = pj * i * (1 - 1 / p1) ... (1 - 1 / pk)
                // phi(pj * i) = pj * phi(i)
                phi[primes[j] * i] = primes[j] * phi(i);
                break;
            }
            // i mod pj != 0
            // phi(i) = i * (1 - 1 / p1) ... (1 - 1 / pk)
            // phi(pj * i) = pj * i * (1 - 1 / p1) ... (1 - 1 / pk) * (1 - 1 / pj)
            // phi(pj * i) = phi(i) * pj * (1 - 1 / pj) = phi(i) * (pj - 1)
            phi[primes[j] * i] = phi[i] * (primes[j] - 1)
`        }
    }

    ll res = 0;  // phi(1) = 1
    for(int i = 1; i <= n; ++i) {
        res += phi[i];
    }
    return res;
}

int main() {
    int n;
    cin >> n;

    cout << get_eulers(n) << endl;

    return 0;
}
```


## 欧拉定理

若 $a$ 与 $n$ 互质，则 

$$a^{\varphi(n)} \equiv 1 (mod \ n) \tag{2.1}$$

**例：**

$a = 5, \ n = 6, \ 5 ^ {\varphi(6)}\ mod\ 6 = 5 ^ 2\ mod\ 6 = 1$ 

**证明：**

$1 \sim n$ 中，所有与 $n$ 互质的数为：$a_1,a_2,...,a_{\varphi(n)}$

$aa_1,aa_2,...,aa_{\varphi(n)}$ 也和 $n$ 互质，且互不相同。(若相同，设 $aa_i \equiv aa_j(mod\ n)$，则 $a(a_i - a_j)\equiv0(mod\ n)$，则 $a_i\equiv a_j$，矛盾)

$\{a_1, a_2, ... a_n\}$ 为 $1 \sim n$ 中所有与 $n$ 互质的数，且互不相同。

$\{aa_1, aa_2, ... aa_n\}$ 也为 $1 \sim n$ 中所有与 $n$ 互质的数 (模 $n$)，且互不相同。

两个集合都包含 $\varphi(n)$ 个与 $n$ 互质的数，只不过顺序不同，因此两个集合相等。

则有

$$\prod_{i=1}^{\varphi(n)} (aa_i) \equiv \prod_{i=1}^{\varphi(n)} (a_i) (mod\ n)$$

由于

$$\prod_{i=1}^{\varphi(n)} (aa_i) = a^{\varphi(n)} \prod_{i=1}^{\varphi(n)} a_i$$

因此

$$a^{\varphi(n)} \prod_{i=1}^{\varphi(n)} a_i \equiv \prod_{i=1}^{\varphi(n)} (a_i) (mod\ n)$$

即可得出

$$a^{\varphi(n)} \equiv 1 (mod\ n)$$

证毕


## 费马定理

若 $p$ 为质数，$gcd(a, p) = 1$，则 

$$a ^ {p - 1} \equiv 1(mod\ p) \tag{3.1}$$

费马定理是欧拉定理的简单推论

**证明：**

若 $p$ 为质数，则 $\varphi(p) = p - 1$，根据欧拉定理即可得证。


## 乘法逆元

### 定义

对于某一个 $b$，能找到一个 $x$，使得 $\frac{a}{b} \equiv a\cdot x\ (mod\ m)$，则 $x$ 称为 $b$ 模 $m$ 的逆元，记作 $b ^ {-1}$ (不是 $b$ 的负一次方，是一个整数)。

$$\frac{a}{b} \equiv a\cdot b^{-1}\ (mod\ m) \tag{5.1}$$

### 逆元的性质

$$\frac{a}{b} \equiv a\cdot b^{-1}\ (mod\ m)$$

$$b\cdot {\frac{a}{b}} \equiv b\cdot a\cdot b^{-1}\ (mod\ m)$$

$$a \equiv b\cdot a\cdot b^{-1}\ (mod\ m)$$

$$ b\cdot b^{-1}\equiv 1\ (mod\ m) \tag{5.2}$$

问题即转化为：

对于某一个 $b$，求一个 $x$，使得 $b\cdot x\equiv 1\ (mod\ p)$，其中 $p$ 是质数。

### 逆元的求法

由费马定理：

$$b^{p-1}\equiv 1\ (mod\ p)$$

$$b\cdot b^{p-2}\equiv 1\ (mod\ p)$$

因此，如果 $p$ 是质数，则 $b$ 模 $p$ 的逆元是 $b^{p - 2}$。因此可以用快速幂求逆元。

### 注意

$b$ 和 $p$ 的不互质时不存在逆元

```cpp
#include <iostream>
#include <cstdio>
using namespace std;

typedef long long ll;
// a ^ k % p
int qmi(int a, int k, int p) {
    int res = 1;
    while(k) {
        if(k & 1) res = (ll)res * a % p;
        k >>= 1;
        a = (ll)a * a % p;
    }
    return res;
}

int main() {
    int n;
    scanf("%d", &n);
    while(n--) {
        int a, k, p;
        scanf("%d%d%d", &a, &k, &p);

        int res = qmi(a, p - 2, p);
        if(a % p) printf("%d\n", res);
        else printf("impossible\n");
    }
    return 0;
}
```

## 裴蜀定理

有一对正整数 $a,b$ ，那么一定存在非零整数 $x,y$ ，使得 $ax + by = gcd(a, b)$

$a$ 和 $b$ 的最大公约数是 $a$ 和 $b$ 能凑出来的最小的正整数

假设 $x$ 和 $y$ ，使得 $ax + by = d$ ，那么 $d$ 一定是 $a$ 和 $b$ 的最大公约数的倍数。因为 $a$ 是 $gcd(a, b)$ 的倍数，$b$ 也是 $gcd(a, b)$ 的倍数，所以 $ax + by = d$ ，也一定是 $gcd(a, b)$ 的倍数。

**证明**

构造法

构造的方法就是扩展欧几里得算法


## 扩展欧几里得算法

### 欧几里得算法

$$gcd(a, b) = gcd(b, a\ mod\ b) \tag{7.1}$$

```cpp
#include <iostream>
#include <cstdio>
using namespace std;

int gcd(int a, int b) {
    if(!b) {
        return a;
    }
    return gcd(b, a % b);
}

int main() {
    int n;
    scanf("%d", &n);
    while(n--) {
        int a, b;
        scanf("%d%d", &a, &b);
    }
    return 0;
}
```

### 扩展欧几里得算法

由于 $gcd(a, 0) = a$，因此 $a\cdot x + 0\cdot y = a$ 的一组解为 $x = 1, y = 0$。

$$a\ mod\ b = a - \lfloor \frac{a}{b} \rfloor \cdot b \tag{7.2}$$

$$by + (a\ mod\ b)x = gcd(a, b) = d$$

$$by + (a - \lfloor \frac{a}{b} \rfloor \cdot b)x = d$$

$$ax + b(y - \lfloor \frac{a}{b} \rfloor \cdot x) = d$$

```cpp
#include <iostream>
#include <cstdio>
using namespace std;

int exgcd(int a, int b, int &x, int &y) {
    if(!b) {
        x = 1, y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return d;
}

int main() {
    int n;
    scanf("%d", &n);
    while(n--) {
        int a, b;
        scanf("%d%d", &a, &b);
        exgcd(a, b, x, y);
        printf("%d %d\n", x, y);
    }
    return 0;
}
```

$x$ $y$ 不唯一

$ax + by = d$

$a(x - \frac{b}{d}) + b(y + \frac{a}{d}) = d$

$$ 
\left\{
\begin{array}{rcl}
&x = x_0 - \frac{b}{d} \cdot k \\
&y = y_0 + \frac{b}{d} \cdot k \\
\end{array} \right. 
(k \in \mathbb{Z})
$$

### 求解线性同余方程

$$ax≡b(mod\ m)$$

$$∃y∈ \mathbb{Z},\ s.t.\ ax = m⋅y + b$$

$$ax - my = b$$

$$y' = -y$$

$$ax + my' = b$$

$$gcd(a, m) | b$$

$$ax + my' = gcd(a, m)$$

$$\frac{b}{gcd(a, m)}(ax + my) = gcd(a, m) \cdot \frac{b}{gcd(a, m)}$$

```cpp
#include <iostream>
#include <cstdio>
using namespace std;
typedef long long ll;

int exgcd(int a, int b, int &x, int &y) {
    if(!b) {
        x = 1, y = 0;
        return a;
    }
    int d = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return d;
}

int main() {
    int n;
    scanf("%d", &n);
    while(n--) {
        int a, b, m;
        scanf("%d%d%d", &a, &b, &m);
        int x, y;
        int d = exgcd(a, m, x, y);
        if(b % d) printf("impossible\n");
        else printf("%d\n", (ll)x * (b / d) % m);
    }
    return 0;
}
```

(更新中)
