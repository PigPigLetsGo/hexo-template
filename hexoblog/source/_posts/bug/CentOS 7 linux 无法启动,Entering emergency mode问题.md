---
title: vmware - CentOS 7 linux 无法启动,Entering emergency mode问题
categories:
    - [问题总汇]
tags:
    - linux
    - 问题总汇
---

# CentOS 7 linux 无法启动,Entering emergency mode问题

由于虚拟机强制关机,导致虚拟机CentOS 7启动时报错

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/20191128223811271.png)

解决办法

从报错信息中可以看到输入==`journalctl`==命令就可以查看本次启动的日志

日志最后如下图

![image-20240208121510029](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208121510029.png)

从日志中看到错误的原因是无法挂载到系统,即可 Failed to monut/sysroot

解决办法,输入命令

```bash
xfs_repair -v -L /dev/dm-0
```

然后出现下面结果

![image-20240208121523842](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208121523842.png)

重新启动客户端