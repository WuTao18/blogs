---
title: HDU 3746 Cyclic Nacklace (KMP找循环节)
date: 2019-11-03 22:53:42
tags:
- ACM刷题
- KMP
- 字符串
categories: 竞赛
mathjax: true
---

题目链接：[HDU 3746](http://acm.hdu.edu.cn/showproblem.php?pid=3746)

![](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/11/03/1572791173434-1572791173438.png)

<!--more-->

## Sample Input

```markdown
3
aaa
abca
abcde
```

## Sample Output

```markdown
0
2
5
```

## Author

> possessor WC

## Source

> [HDU 3rd “Vegetable-Birds Cup” Programming Open Contest](http://acm.hdu.edu.cn/search.php?field=problem&key=HDU+3rd+%A1%B0Vegetable-Birds+Cup%A1%B1+Programming+Open+Contest&source=1&searchmode=source)

## Solution

### 题意

给定一个字符串，问至少需要在末尾添加多少个字符使得字符串循环。

### 思路

**KMP**

设前缀函数为 $\pi()$，字符串长度为 $n$，下标从 $1$ 开始。

最小循环节为 $k = n - \pi(n)$。

如果 $n \% k=0$，那么字符串已经循环。

否则还需要添加 $k - n \% k$ 个字符。

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 5;
int n, pi[maxn];
char str[maxn];

void prefix_function() {
	int j = 0;
	for (int i = 2; i <= n; ++i) {
		while (j > 0 && str[j+1] != str[i]) j = pi[j];
		if (str[j+1] == str[i]) j++;
		pi[i] = j;
	}
}

int main() {
	int T;
	scanf("%d", &T);
	while(T--) {
		scanf("%s", str + 1);
		n = strlen(str + 1);
		prefix_function();
		if(pi[n] == 0) {
            printf("%d\n", n);
		} else {
			int k = n - pi[n];
			if(n % k == 0) {
                printf("0\n");
            } else {
                printf("%d\n", k - n % k);
            }
		}
	}
	return 0;
}
```
