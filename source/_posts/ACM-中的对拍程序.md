---
title: ACM 中的对拍程序
date: 2019-10-26 20:58:42
tags:
- ACM指南
- 对拍
categories: 工具
mathjax: true
---

所谓对拍，就是随机生成数据，然后用一个肯定正确的暴力算法的程序，去测试一个要提交的程序。

由于比赛中一般使用 Linux 系统，所以本篇博客的代码都是 Linux 下的程序代码。

~~其实最简单的方式是写脚本。~~

这里介绍的是用选手最熟悉的 C++ 语言写对拍程序。

<!--more-->

假设要提交的程序为 sol.cpp，暴力的程序为 bf.cpp，随机数据生成器 random.cpp。

首先编译这三个程序，得到 sol，bf，random。

一种简单的方法就是先生成数据，然后暴力程序和待测试程序分别运行，比较结果。

生成数据，输出到 data.in

```
./random > ./data.in
```

先运行暴力程序，结果输出到 data.out

```
./bf < ./data.in > ./data.out
```

然后运行待测试程序，结果输出到 data.ans

```
./sol < ./data.in > ./data.ans
```

然后比较两个结果是否相同

```
diff ./data.out ./data.out
```

上述方法的缺点是无法统计时间。

于是就有了以下的对拍程序。

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    for(int T = 0; T < 10000; ++T) {
        system("./random > ./data.in");
        double st = clock();
        system("./sol < ./data.in > ./data.ans");
        double et = clock();
        system("./bf < ./data.in > ./data.out");
        if(system("diff ./data.out ./ data.ans")) {
            puts("Wrong Answer\n");
            return 0;
        } else {
            printf("Accepted, 测试点 #%d, 用时 %.0lfms\n", T, et - st);
        }
    }
    return 0;
}
```

将上述所有文件放在同一目录下，编译运行对拍程序就可以对拍。

## 参考

> [《算法竞赛进阶指南》](https://book.douban.com/subject/30136932/) 李煜东 著