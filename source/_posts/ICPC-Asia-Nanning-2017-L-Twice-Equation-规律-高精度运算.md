---
title: ICPC Asia Nanning 2017 L. Twice Equation (规律 高精度运算)
date: 2019-08-15 20:56:04
tags:
- ACM刷题
- Java
- 高精度
categories: 竞赛
mathjax: true
---

题目链接：[Twice Equation](https://nanti.jisuanke.com/t/A1541)

比赛链接：[ICPC Asia Nanning 2017](https://www.jisuanke.com/contest/3107?view=challenges)

## Description
> For given $L$, find the smallest $n$ no smaller than $L$ for which there exists an positive integer $m$ for which $2m(m + 1) = n(n + 1)$.  


## Input
> This problem contains multiple test cases. The first line of a multiple input is an integer $T (1 \le T < 1000)$ followed by $T$ input lines. Each line contains an integer $L (1 \le L < 10^{190})$.

## Output
> For each given $L$, output the smallest $n$. If available nn does not exist, output $−1$.
 

## Sample Input
```markdown
3
1
4
21
```

## Sample Output
```markdown
3
20
119
```

## Solution

### 题意

给出一个整数 $L$，求大于等于 $L$ 的最小整数 $n$ 满足存在一个整数 $m$ 使得 $2m(m + 1) = n(n + 1)$。

### 题解

**找规律 高精度运算**

打表找规律

$$f(n) = f(n - 1) * 6 - f(n - 2) + 2$$

然后用 Java 大数求解即可。

## Code

```java
import java.util.Scanner;
import java.math.BigInteger;

public class Main {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        BigInteger[] a = new BigInteger[1000];
        // 打表
        a[0] = BigInteger.ZERO;
        a[1] = BigInteger.valueOf(3);
        BigInteger six = new BigInteger("6");
        BigInteger two = new BigInteger("2");
        for(int i = 2; i < 300; ++i) {
            a[i] = ((a[i - 1].multiply(six)).subtract(a[i - 2])).add(two);
        }

        int t = in.nextInt();
        while (t-->0){
            boolean flag = false;
            BigInteger l = in.nextBigInteger();
            for(int i = 0; i < 1000; ++i) {
                if(a[i].compareTo(l) >= 0) {
                    System.out.println(a[i]);
                    flag = true;
                    break;
                }
            }
            if(!flag) System.out.println(-1);
        }
    }
}
```