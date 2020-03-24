---
title: 洛谷 P3369 【模板】普通平衡树 (Treap)
date: 2019-09-25 22:34:41
tags:
- ACM刷题
- 平衡树
- Treap
- BST
- 树
- 模板题
categories: 竞赛
mathjax: true
---

题目链接：[P3369 【模板】普通平衡树](https://www.luogu.org/problem/P3369)

## 题意

构造一种数据结构满足给出的 6 种操作。

## 思路

**平衡树**

平衡树的模板题。

先学习了一下 Treap。

Treap 在插入结点时给该结点随机生成一个额外的权值，然后用该权值维护一个大根堆，如果某个结点不满足大根堆的性质，就通过旋转与父节点交换。

对于删除某个结点，通过不断向下旋转直至变成叶子结点，然后直接删除即可。

模板来自[《算法竞赛进阶指南》](https://book.douban.com/subject/30136932/)。

## 代码

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 10;
const int inf = 0x3f3f3f3f;

struct Treap {
	int l, r;
	int val, dat;
	int cnt, size;
} a[maxn];
int tot, root;
int n;

int new_node(int val) {
	a[++tot].val = val;
	a[tot].dat = rand();
	a[tot].cnt = a[tot].size = 1;
	return tot;
}

void update(int p) {
	a[p].size = a[a[p].l].size + a[a[p].r].size + a[p].cnt;
}

void build() {
	new_node(-inf), new_node(inf);
	root = 1, a[1].r = 2;
	update(root);
}

int get_rank(int p, int val) {
	if (p == 0) return 0;
	if (val == a[p].val) return a[a[p].l].size + 1;
	if (val < a[p].val) return get_rank(a[p].l, val);
	return get_rank(a[p].r, val) + a[a[p].l].size + a[p].cnt;
}

int get_val(int p, int rank) {
	if (p == 0) return inf;
	if (a[a[p].l].size >= rank) return get_val(a[p].l, rank);
	if (a[a[p].l].size + a[p].cnt >= rank) return a[p].val;
	return get_val(a[p].r, rank - a[a[p].l].size - a[p].cnt);
}

// 右旋
void zig(int &p) {
	int q = a[p].l;
	a[p].l = a[q].r, a[q].r = p, p = q;
	update(a[p].r), update(p);
}

// 左旋
void zag(int &p) {
	int q = a[p].r;
	a[p].r = a[q].l, a[q].l = p, p = q;
	update(a[p].l), update(p);
}

void add(int &p, int val) {
	if (p == 0) {
		p = new_node(val);
		return;
	}
	if (val == a[p].val) {
		a[p].cnt++;
	} else if (val < a[p].val) {
		add(a[p].l, val);
		if (a[p].dat < a[a[p].l].dat) zig(p);
	} else {
		add(a[p].r, val);
		if (a[p].dat < a[a[p].r].dat) zag(p);
	}
	update(p);
}

int get_pre(int val) {
	int ans = 1; // a[1].val==-inf
	int p = root;
	while (p) {
		if (val == a[p].val) {
			if (a[p].l > 0) {
				p = a[p].l;
				while (a[p].r > 0) p = a[p].r;
				ans = p;
			}
			break;
		}
		if (a[p].val < val && a[p].val > a[ans].val) ans = p;
		p = val < a[p].val ? a[p].l : a[p].r;
	}
	return a[ans].val;
}

int get_next(int val) {
	int ans = 2; // a[2].val==inf
	int p = root;
	while (p) {
		if (val == a[p].val) {
			if (a[p].r > 0) {
				p = a[p].r;
				while (a[p].l > 0) p = a[p].l;
				ans = p;
			}
			break;
		}
		if (a[p].val > val && a[p].val < a[ans].val) ans = p;
		p = val < a[p].val ? a[p].l : a[p].r;
	}
	return a[ans].val;
}

// 把非叶子结点旋转至叶子结点后直接删除
void remove(int &p, int val) {
	if (p == 0) return;
	if (val == a[p].val) {
		if (a[p].cnt > 1) {
			a[p].cnt--, update(p);
			return;
		}
		if (a[p].l || a[p].r) { // 不是叶子节点，向下旋转
			if (a[p].r == 0 || a[a[p].l].dat > a[a[p].r].dat)
				zig(p), remove(a[p].r, val);
			else
				zag(p), remove(a[p].l, val);
			update(p);
		}
		else p = 0; // 叶子节点，删除
		return;
	}
	val < a[p].val ? remove(a[p].l, val) : remove(a[p].r, val);
	update(p);
}

int main() {
	build();
	cin >> n;
	while (n--) {
		int op, x;
        cin >> op >> x;

		if(op == 1) {
			add(root, x);
        } else if(op == 2) {
			remove(root, x);
        } else if(op == 3) {
            cout << get_rank(root, x) - 1 << endl;
        } else if(op == 4) {
            cout << get_val(root, x + 1) << endl;
        } else if(op == 5) {
            cout << get_pre(x) << endl;
        } else {
            cout << get_next(x) << endl;
        }
	}
    return 0;
}
```