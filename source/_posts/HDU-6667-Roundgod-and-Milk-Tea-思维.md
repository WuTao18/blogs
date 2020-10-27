---
title: HDU 6667 Roundgod and Milk Tea (思维)
date: 2019-08-14 22:01:52
tags:
- ACM刷题
- 思维
categories: 竞赛
mathjax: true
---

2019 杭电多校 8 1011

题目链接：[HDU 6667](http://acm.hdu.edu.cn/showproblem.php?pid=6667)

比赛链接：[2019 Multi-University Training Contest 8](http://acm.hdu.edu.cn/search.php?field=problem&key=2019+Multi-University+Training+Contest+8&source=1&searchmode=source)


## Problem Description
> Roundgod is a famous milk tea lover at Nanjing University second to none. This year, he plans to conduct a milk tea festival. There will be $n$ classes participating in this festival, where the ith class has $a_i$ students and will make $b_i$ cups of milk tea.
> 
> Roundgod wants more students to savor milk tea, so he stipulates that every student can taste at most one cup of milk tea. Moreover, a student can't drink a cup of milk tea made by his class. The problem is, what is the maximum number of students who can drink milk tea?
 
<!--more-->

## Input
> The first line of input consists of a single integer $T (1\le T\le 25)$, denoting the number of test cases. 
> 
> Each test case starts with a line of a single integer $n (1\le n\le 10^6)$, the number of classes. For the next $n$ lines, each containing two integers $a,b (0\le a,b\le 10^9)$, denoting the number of students of the class and the number of cups of milk tea made by this class, respectively. 
> 
> It is guaranteed that the sum of $n$ over all test cases does not exceed $6\times 10^6$.
 

## Output
> For each test case, print the answer as a single integer in one line.

## Sample Input
```markdown
1
2
3 4
2 1
```

## Sample Output
```markdown
3
```

## Solution

### 题意：

有 $n$ 个班级，每个班有 $a_i$ 个人，做了 $b_i$ 杯奶茶，每个班的每个人最多喝一杯奶茶且不能和自己班做的奶茶，问最多共有多少人喝到奶茶。

### 思路

最初的想法是用一个 $sum$ 记录所有剩余的奶茶数，然后每个组能喝的奶茶数为 $sum\ -$ 该组的奶茶(自己不能喝自己的) $+$ 上一组做的奶茶 (上一组减掉的加回来)。后来发现有点问题，就是中间一步减掉自己的奶茶可能是减多的，也就是上一组喝掉的可能就是当前组的奶茶，那么当前组剩余的奶茶是比原来少的，于是就用 $tmp2$ 保存上一组喝掉的奶茶数，每次让上一组喝掉当前组的奶茶，如果不够喝再用 $tmp$ 保存还要喝掉的奶茶数，往下迭代。

```cpp
#include <bits/stdc++.h>
using namespace std;

typedef long long ll;

const int maxn = 1e6 + 10;

struct Team
{
    ll m, n;  // 人数 奶茶数
} t[maxn];

int cmp(Team t1, Team t2) {
    return t1.m > t2.m;
}

int main() {
    int T;
    cin >> T;
    while(T--) {
        int n;
        scanf("%d", &n);
        ll sum = 0;
        for(int i = 0; i < n; ++i) {
            scanf("%lld%lld", &t[i].m, &t[i].n);
            sum += t[i].n;
        }
        sort(t, t + n, cmp);
        ll ans = 0; 
        ll tmp = t[0].n; // tmp 保存喝掉的奶茶数 第一组一定要被喝
        ll tmp2 = 0; // tmp2 保存的是上一组喝掉的奶茶
        for(int i = 0; i < n; ++i) {
            if(i) {
                // 上一组喝掉的奶茶数+之前喝掉的奶茶数
                if(t[i].n < tmp2 + tmp) {
                    t[i].n = 0;
                    tmp = tmp2 + tmp - t[i].n;
                } else {
                    t[i].n = t[i].n - (tmp2 + tmp);
                    tmp = 0;
                }
            }
            sum -= t[i].n; // 自己不能喝自己的奶茶
            if(i) sum += t[i - 1].n; // 可以喝上一组的奶茶
            // 剩余的奶茶数与第 i 组人数比较
            if(sum >= t[i].m) {
                ans += t[i].m;
                sum -= t[i].m;
                tmp2 = t[i].m;
            } else {
                ans += sum;
                sum -= sum;
                tmp2 = sum;
            }
            // cout << ans << endl;
        }
        printf("%lld\n", ans);
    }
    return 0;
}
```

比赛中完全想复杂了，其实完全可以很快处理。把每个人能喝的奶茶加起来和所有的奶茶比较即可。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e6 + 10;

ll a[maxn], b[maxn];

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        int n;
        scanf("%d", &n);
        ll sum = 0;
        for(int i = 0; i < n; ++i) {
            scanf("%lld%lld", &a[i], &b[i]);
            sum += b[i];
        }
        ll ans = 0;
        for(int i = 0; i < n; ++i) {
            ans += min(a[i], sum - b[i]);
        }
        printf("%lld\n", min(ans, sum));
    }
    return 0;
}
```