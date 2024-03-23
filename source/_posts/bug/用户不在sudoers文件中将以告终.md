---
title: 解决提示：用户不在sudoers文件中将以告终
categories:
    - [问题总汇]
tags:
    - linux
    - 问题总汇
---

1.  切换到root权限下,查看/etc/sudoers文件的权限

2.  更改权限为777为所有用户可读写
3.  vim进去编译sudoers文件在100行的下一行写:

```bash
dkx(用户名)   ALL=(ALL)   ALL
```

4.  保存并退出
5.  再将sudoers文件的权限改为400 如果执行sudo报错将权限改为440即可
