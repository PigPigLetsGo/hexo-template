---
title: 配置JDK
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

# 配置JDK

==重要提示!==:下载JDK前先创建一个属于JDK的文件地址,因为这样好管理也好找到,虽然linux配置JDK没有Windows那么复杂,创建文件指令

```bash
mkdir /opt/jdk
```

下载前可以看下浏览器默认下载位置或者下载的时候弹出提示框进行选择下载到的位置我们就选择新创建的jdk目录

![image-20240208121140196](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208121140196.png)

```bash
cd /opt/jdk
```

切换到这个自己创建的jdk目录下再下载或者传输jdk压缩包

首先去官网下载一个JDK(注意:一定要下载对应自己linux系统的JDK不然可能会出现在执行JDK的时候报错)

Arm 64 Compressed Archive

x64 Compressed Archive

这两个linux版本的看系统下载,我当前下载的是64为的 x64 Compressed Archive

报错信息:`无法执行二进制文件`!

在虚拟机中下载或者在Windows中下载都可以在Windows中可以使用==Xftp==工具将其传输到linux中,当然如果出现传输失败就切换下权限切换到root试试,应该会成功的

下载完成后:

使用Xshell或者linux中打开终端,在当前压缩包的目录下

输入指令:

```bash
tar -xvzf jdk-n.n.n_x64_tar.gz[如果只有这一个压缩包其实直接tab就好了,写这么多就是示个意]
```

完成后终端输入指令:

切换到跟目录下:

```bash
cd /
vim /etc/profile
```

打开配置文件后,在配置文件的末尾输入:

```bash
export JAVA_HOME=/usr/local/jdk/jdk-17.0.4.1#这里根据自己的jdk位置来进行配置即可
export PATH=$JAVA_HOME/bin:$PATH
```

输入好配置文件后执行指令让这个目录的jdk生效,不然还是不能使用的

执行指令:

```bash
source /etc/profile
```

然后就可以输入指令来查看jdk是否安装成功了

`javac` , `java -version` , `java` 

