---
title: 设置 Quartus II 的仿真时间大于 1us
date: 2019-10-22 23:39:50
tags:
- Quartus
- 数字电路
categories: 工具
mathjax: true
---

Quartus II 仿真的默认时长是 1us。

设置时钟时看到 End time 想修改时长，把默认的 1us 改成 10us。

![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/10/22/1571757694971-1571757695212.png)

然后提示 End time 不合法。（只能设置为 0 到 1us）

![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/10/22/1571757707255-1571757707258.png)

正确的做法是在菜单栏选择 Edit -> End time，然后将 time 设置为 10us 就可以了。

![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/10/22/1571757716320-1571757716325.png)

![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/10/22/1571757728121-1571757728124.png)

![title](https://raw.githubusercontent.com/WuTao18/images/master/gitnote/2019/10/22/1571757736360-1571757736364.png)