---
title: C++ STL rope 可持久化平衡树 (可持久化数组)
date: 2019-10-02 23:04:15
tags:
- ACM学习
- STL
- 数据结构
- 可持久化平衡树
- 可持久化数组
- 平衡树
categories: 
- [竞赛]
- [笔记]
mathjax: true
---

官方文档好像 GG 了。

rope 不属于标准 STL，属于扩展 STL，来自 pb_ds 库 (Policy-Based Data Structures)。

基本操作：

```cpp
#include <ext/rope>  // 头文件
using namespace __gnu_cxx;  // 注意名称空间

rope<int> rp;

int main() {
    rp.push_back(x); // 在末尾插入 x
    rp.insert(pos, x); // 在 pos 处插入 x
    rp.erase(pos, x); // 在 pos 处删除 x 个元素
    rp.length(); // 返回 rp 的大小
    rp.size(); // 同上
    rp.replace(pos, x); // 将 pos 处的元素替换成 x
    rp.substr(pos, x); // 从 pos 处开始提取 x 个元素
    rp.copy(pos, x, s); // 从 pos 处开始复制 x 个元素到 s 中
    rp[x]; // 访问第 x 个元素
    rp.at(x); // 同上
    return 0;
}

```

rope 内部是块状链表实现的，黑科技是支持 $O(1)$ 复制，而且不会空间爆炸 (rope 是平衡树，拷贝时只拷贝根节点就行)。因此可以用来做可持久化数组。

拷贝历史版本的方式：

```cpp
rope<int> *his[100000];
his[i] = new rope<int> (*his[i - 1]);
```

缺点是常数大 (C++ STL 的通病)。

还有一个叫 crope 的东西，crope 即 rope<char>，可以用 cin/cout 直接输入输出，常用于字符串操作。
