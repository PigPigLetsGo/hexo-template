---
title: 设置/初始化root用户
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

# 设置/初始化root用户

【设置 root 用户密码】
默认安装好的linux系统是没有设置root用户密码的，下面介绍如何设置root用户的密码。

由于Linux系统默认是没有激活 root 用户的，需要我们手动进行操作，步骤也非常简单，在命令行界面（终端）中输入如下命令：

```bash
sudo passwd 或者 sudo passwd root
 
Password： 你当前用户的密码
 
Enter new UNIX password： 设置是 root 用户的密码
 
Retype new UNIX password：重复以上 root 用户的密码
```

