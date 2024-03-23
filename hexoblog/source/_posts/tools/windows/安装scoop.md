---
title: 安装和使用scoop
categories:
   - [tools,windows]
tags: 
   - tools
   - windows
---

## 常用命令

-  寻找软件：`scoop search 软件名` 
-  安装软件：`scoop install 软件名` 
-  删除软件：`scoop uninstall 软件名` 
-  查看已安装的软件： `sccop list` 
-  清理缓存：`scoop cache rm 软件名` 或者 `scoop cache rm *` 
-  查看可添加仓库：`scoop bucket known` 
-  添加额外仓库：`scoop bucket add 仓库名` 

1 打开PowerShell输入，执行策略更改为 RemoteSigned

```shell
set-executionpolicy RemoteSigned
```

2 输入命令进行安装

```shell
iex "& {$(irm get.scoop.sh)} -RunAsAdmin"
```

### 中国用户专享

如果访问Github有问题，下载资源失败可以尝试一下方式：

1.  设置Scoop代理。在命令行中输入 (PowerShell或CMD中都行)，：`scoop config proxy 127.0.0.1:7890` 让scoop网络连接走代理，后面的ip地址和端口号根据自己的代理设置。

### 更改scoop的默认安装路径

在终端输入：

```sh
[environment]::setEnvironmentVariable('SCOOP_GLOBAL','F:\GlobalScoopApps','Machine') $env:SCOOP_GLOBAL='F:\GlobalScoopApps'
```

-  路径自己创建存放的文件夹自己改路径
-  当使用scoop 命令进行安装时，注意加 -g ， scoop install <app> -g

## 建议安装程序aria2

```sh
PS C:\Users\Administrator> scoop list
Installed apps:

Name           Version  Source Updated             Info
----           -------  ------ -------             ----
7zip           23.01    main   2023-07-14 00:16:05
git            2.41.0.2 main   2023-07-14 00:16:42
neofetch       7.1.0    main   2023-07-14 00:17:26
vim            9.0      main   2023-07-14 00:24:45
aria2          1.36.0-1 main   2023-08-26 11:06:50 Global install
quicklook      3.7.3    extras 2023-08-26 13:38:35 Global install
youtube-dl-gui 1.8.5    extras 2023-08-26 11:52:11 Global install


PS C:\Users\Administrator> scoop install aria2
```

## 使用bucket让windows软件包管理器变得更加强大

#### 什么是bucket?

在 Scoop 里面，bucket 就是一个软件仓库。Scoop 将一个个仓库缓存至本地，当我们想要安装一个软件的时候，Scoop 就从本地的仓库中挑选出我们想要安装的软件的安装配置文件，并依照这个配置文件进行软件的安装工作，包括：

-  从哪里下载软件
-  如何安装软件、安装到哪里、需要修改更新什么环境变量
-  安装之前、之后都要做什么准备（善后）工作
-  ……

#### 添加一个bucket

首先，Scoop 给我们提供了很多可以直接使用的 bucket，就是为了方便我们安装更为常见的带有 GUI 的软件。一个最为常见，也是我推荐大家添加的 bucket 是 `extras`，这里面基本涵盖了大部分不符合 main bucket 收录条件的常用软件，包括我们熟悉的：各个版本的 Firefox、福昕阅读器、Geek Uninstaller、Inkscape、Snipaste 等等。（甚至有 Steam，但是不推荐用这样的方式安装。）

我们可以通过下面这个命令来添加 `extras` 这个 bucket：

```sh
scoop bucket add extras
```

![image-20230826112430785](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308261124794.png)

之后，我们就可以下载更多更多常见的软件了。比方说，我们要下载 [ScreenToGif](https://sspai.com/post/40556) 这个极为好用的 Gif 屏幕录制软件：

```sh
scoop install screentogif
```

### 官方维护的 bucket

`extras` 这个 bucket 是最有用，也是我们大部分人肯定会用到的仓库。除此之外，我们可以通过这个命令查看 Scoop 还能直接识别哪些 bucket：

```sh
scoop bucket known
```

![image-20230826112605419](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308261126217.png)

下面列出的这几个仓库就是 Scoop 可以直接识别并添加的 bucket，即我们可以通过下面这个命令直接添加相应的 bucket：

```sh
scoop bucket add <仓库名>
```

比如：

```sh
PS C:\Users\Administrator> scoop bucket known
main
extras
versions
nirsoft
sysinternals
php
nerd-fonts
nonportable
java
games
PS C:\Users\Administrator> scoop bucket add mages
Unknown bucket 'mages'. Try specifying <repo>.
usage: scoop bucket add <name> [<repo>]
PS C:\Users\Administrator> scoop bucket add games
Checking repo... OK
The games bucket was added successfully.
PS C:\Users\Administrator>
```

这里面，我来介绍一下和开发环境的安装没有太大关系的几个仓库：

-  `extras`：就是我刚刚介绍的，Scoop 官方维护的一个仓库，涵盖了大部分因为种种原因不能被收录进主仓库的常用软件。地址：[lukesampson/scoop-extras](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Flukesampson%2Fscoop-extras%2Ftree%2Fmaster%2Fbucket)

-  `nirsoft`：是一个 NirSoft 开发的小工具的安装合集。NirSoft 制作了大量的（dozens and dozens）小工具，包括系统工具、网络工具、密码恢复等等，孜孜不倦、持续更新。

-  -  Bucket 地址：[kodybrown/scoop-nirsoft](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2Fkodybrown%2Fscoop-nirsoft)
   -  NirSoft 官网地址：[NirSoft](https://sspai.com/link?target=http%3A%2F%2Fwww.nirsoft.net%2F)

-  `games`：顾名思义，是游戏（和与游戏相关的工具）合集。包含了大量免费/开源的小游戏，地址：[Calinou/scoop-games](https://sspai.com/link?target=https%3A%2F%2Fgithub.com%2FCalinou%2Fscoop-games)

剩下的几个 bucket 都是和开发环境相关的，比如 `java` 这个 bucket 就是为了安装 JDK 用的 bucket，这些我在这里就不过多赘述了。

## 备份与恢复配置文件

```sh
#[备份] 导出 scoop 的 bucket，已安装 apps 和 自定义配置信息到文件 scoopfile.json中
scoop export > scoopfile.json
#[恢复] 从 scoopfile.json 文件中恢复信息
scoop import scoopfile.json
```

