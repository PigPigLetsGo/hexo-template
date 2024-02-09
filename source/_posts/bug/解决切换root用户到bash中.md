---
title: 解决切换root用户到bash中
categories:
    - [问题总汇]
tags:
    - linux
    - 问题总汇
---

# 解决切换root用户到bash中

1.  打开终端输入:

```bash
切换到root此时root为bash不要紧
cp /etc/skel/.bash* .
.为当前路径直接复制到root下或者执行
cp /etc/skel/.bash* /root/
将缺失的配置文件复制到root用户下
```

2.  登出,或者重启一下
