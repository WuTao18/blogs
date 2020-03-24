---
title: C++ 常见的cin的错误处理
date: 2019-11-07 22:50:01
tags:
- C++
- 编程语言
categories: 笔记
mathjax: true
---

写程序时想要用 cin 对输入的合法性检查，于是学习了一下 cin.fail() 函数，顺便学习了类似的一些函数。

cin 对象包含了一个描述流状态的数据成员。流状态有 3 个标志位：eofbit，badbit 和 failbit。

当 cin 操作到达文件末尾时，eofbit 置 1。

当流被破坏时，badbit 置 1。例如：试图读取不可访问的文件、写入写保护的磁盘、写入的设备剩余空间不足等。

cin 操作未能读取到预期的字符时 failbit 会置 1。例如：要输入到一个整型变量中，输入的却是字符时 failbit 置 1。I/O 失败时 failbit 也可能置 1。

如果 3 个状态位都为 0 表示一切顺利。

下面介绍一些检查或改变流状态的方法。

- eof()

如果 eofbit 为 1 返回 true。

所以可以用 `cin.eof()` 来判断是否读到文件尾。

- good()

如果流可以使用，也就是 3 个状态位都为 0，返回 true。

只有 `cin.good()` 为 true 时，`while(cin >> input)` 才返回 true。

- bad()

如果 badbit 为 1 返回 true。

- fail()

如果 failbit 或 badbit 为 1 返回 true。所以与 bad() 一样的情况会返回 true，输入一个类型不匹配的字符也是返回 true。

可用于检查输入是否合法。

```cpp
int a;
cin >> a;
if(cin.fail()) {
    cout << "Input error" << endl;
}
```

注意：如果输入不合法重新输入，failbit 是不会自动重置的。

- rdstate()

返回流状态。

- clear(iostate s)

状态位置位后不会改变。因此要重置状态位需使用 clear(iostate s)。该方法将所有的状态位都设置为 s，其中 s 默认为 0。也就是 clear() 将所有状态位清零。

## 参考

> [《C++ Primer Plus》 Stephen Prata](https://book.douban.com/subject/10789789/)
> 
> [C++之cin.eof, cin.bad, cin.good, cin.fail, cin.clear](https://blog.csdn.net/maoliran/article/details/51725396)