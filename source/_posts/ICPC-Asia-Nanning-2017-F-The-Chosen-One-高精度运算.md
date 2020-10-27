---
title: ICPC Asia Nanning 2017 F. The Chosen One (高精度运算)
date: 2019-08-15 20:39:32
tags:
- ACM刷题
- Java
- 高精度
categories: 竞赛
mathjax: true
---

题目链接：[The Chosen One](https://nanti.jisuanke.com/t/A1535)

比赛链接：[ICPC Asia Nanning 2017](https://www.jisuanke.com/contest/3107?view=challenges)

## 题意

$t$ 组样例，每组给出一个整数 $n(2\le n\le 10^{50})$，求不大于 $n$ 的最大的 $2$ 的整数次幂。

<!--more-->

## 题解

**高精度运算**

`Java BigInteger` 中的 `bitLength()` 方法可以直接计算某个大数二进制表示下的位数。

更多关于 `Java BigInteger` 的操作参见我的另一篇文章 [大数运算之 Java BigInteger 的基本用法](https://wutao18.github.io/2019/08/15/%E5%A4%A7%E6%95%B0%E8%BF%90%E7%AE%97%E4%B9%8B-Java-BigInteger-%E7%9A%84%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95/#more)

```java
import java.util.Scanner;
import java.math.BigInteger;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int t = in.nextInt();
        while (t-->0){
            BigInteger n = in.nextBigInteger();
            BigInteger ans = new BigInteger("2");
            System.out.println(ans = ans.pow(n.bitLength() - 1));
        }
    }
}
```