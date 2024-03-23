---
title: 使用Vagrant与VirtuBox快速安装Linux系统
categories: 
      - [计算机学科,linux,VirtuBox]
tags:
      - 计算机学科
      - linux
---

# 使用Vagrant与VirtuBox快速安装Linux系统

首先我们下载VirtuBox与Vagrant

Vagrant下载完成后需要重启一下电脑，开机后我们输入命令来查看是否安装成功了。

![image-20240316164625749](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316164625749.png)

vagrant的镜像仓库：https://app.vagrantup.com/boxes/search

vagrant的官方下载地址：https://www.vagrantup.com/

我们下面就可以通过Vagrant来安装一个Linux系统了

-  打开window cmd窗口，运行vagrant init centos/7，即可初始化一个centos7系统

   下载虚拟机时 centos/7 是看镜像仓库中的名字的

   运行后提示：

   ![image-20240316165630971](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316165630971.png)

   运行后在指定目录下就会创建一个文件

   ![image-20240316165621225](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316165621225.png)

-  运行vgrant up 即可启动虚拟机，系统root用户的密码是vagrant

   使用命令启动虚拟机

   过程比较漫长，因为他要从官方下载镜像然后启动系统

   ![image-20240316165755530](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316165755530.png)

   等待一会儿在VirtuBox中就可以看到虚拟机了

   ![image-20240316170809872](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316170809872.png)

   提示到如下消息后就直接ctrl + c停止

   ![image-20240316173641519](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316173641519.png)

   然后通过命令 vagrant ssh 连接上虚拟机就行了。

   ![image-20240316173718891](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316173718891.png)

-  vagrant其它常用命令

   -  vagrant ssh：自动使用vagrant用户连接虚拟机
      -  vagrant upload source [destination] [name|id]：上传文件
   -  https://www.vagrantup.com/docs/cli/init.html Vagrant 命令行

-  默认虚拟机的ip地址不是固定ip，开发不方便

   -  修改vagrantfile

   找到用户目录下的vagrantfile文件打开找到如下位置将注释解开

   ![image-20240316180659036](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316180659036.png)

   cmd中查看ip输入命令ipconfig如果没有找到关于VirtuBox的使用这个命令：ipconfig /all

   ![image-20240316181051000](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316181051000.png)

   我们可以看到ip地址是56.1那么我们就需要在vagrantfile的当前位置写上56.n的ip地址如下：

   比如说：56.10

   ![image-20240316181207356](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316181207356.png)

   改完配置后我们就需要重启下虚拟机 输入命令：vagrant reload

   cmd显示如下信息后直接ctrl + c退出就行了，然后重新连接虚拟机vagrant ssh

   ![image-20240316181422136](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316181422136.png)

   使用命令ip addr

   ![image-20240316182254038](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316182254038.png)

   此时我们可以看到ip地址就变成我们设置的ip了

   下面我们互相ping一下，先在windows ping 下 linux的ip

   ![image-20240316182409325](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316182409325.png)

   ping得通

   下面linux ping windows的ip

   ![image-20240316182449309](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316182449309.png)

   也ping得通

-  在命令窗口中退出虚拟机的连接状态执行如下命令：

   -  exit

-  下次使用虚拟机我们启动时可以使用命令：vagrant up 它依靠用户目录下的Vagrantfile启动，启动之后使用vagrant ssh连接上就可以了。

注意：VirtuBox会与括号带不限于如下软件冲突，需要卸载这些软件，然后重启电脑

冲突的软件：红蜘蛛，360，净网大师(有可能)等.

## 环境-配置docker阿里云镜像加速

到阿里云的控制台首页中找到 "容器镜像服务" 里面得到镜像

https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

![image-20240316185154088](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316185154088.png)

依次执行图中的命令即可。

![image-20240316185319343](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316185319343.png)