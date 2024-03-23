---
title: nvm-Node版本控制
categories:
    - [tools,nodeJs]
tags:
    - nodeJs
    - tools
---

附：[node](https://so.csdn.net/so/search?q=node&spm=1001.2101.3001.7020)更换版本（简单操作）
 安装nvm
 1 系统已经有node

2.  网址：https://[github](https://so.csdn.net/so/search?q=github&spm=1001.2101.3001.7020).com/coreybutler/nvm-windows/releases
3.  下载 nvm-setup.zip,解压之后会有个.exe安装程序，第一步是同意，剩下的无脑next即可安装成功。

##### 操作

1. 打开cmd，使用 nvm ls 命令查看本地已有版本
2. 使用 安装命令 nvm install v10.16.0 即可（别忘了写v）
3. 再次使用 nvm ls命令查看本地已有版本。发现12.15.0和10.16.0均有
4. 切换版本 nvm use 12.15.0

### 基础命令

```c
nvm ls               // 查看已安装node版本
nvm install vXX      // 安装对应vXX版本的node
nvm uninstall vXX    // 卸载对应vXX版本的node
nvm use xxx          // 选择使用XXX版本
```