---
title: C++ bitset的使用
date: 2019-11-23 22:08:50
tags:
- 编程语言
- C++
- STL
categories: 竞赛
mathjax: true
---

bitset 一般代替 bool 数组使用，常用于优化空间，因为 bitset 中一个元素只占 1 bit。

<!--more-->

bitset 的大小在定义使就需要确定。如果想要不定长的 bitset，就需要使用 vector<bool>。

bitset 的定义：

```cpp
bitset<16> bt;  // 定义大小为16的bitset，每一位都是0
bitset<16> bt(string("11001"));  // 定义大小为16的bitset，并用string初始化，注意高位为0，也就是 0000000000011001
```

bitset 可以直接使用 cin 和 cout 输入输出

```cpp
bitset<16> bt;
cin >> bt;
cout << bt << endl;
```

bitset 可以像数组一样访问或修改某一位置的元素，注意0表示低位。

```cpp
bitset<8> bt;
bt[0] = 1; // 00000001
```

bitset 也可以像一个数一样进行位运算：与(&)、或(|)、异或(^)、取反(~)、左移(<<)、右移(>>)。

常用函数：

```cpp
bitset<8> bt;
bt.size(); // 返回大小
bt.count(); // 返回1的个数
bt.set(); // 全部置1
bt.set(pos);  // pos位置1
bt.reset();  // 全部置0
bt.reset(pos);  // pos位置0
bt.flip(); // 全部取反
bt.flip(pos); // pos位取反
```
