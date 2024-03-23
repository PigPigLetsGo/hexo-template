---
title: Linux分区
categories: 
      - [计算机学科,linux,磁盘]
tags:
      - 计算机学科
      - linux
---

## Linux分区

原理介绍

1. 对Linux来说无论有几个分区, 分给哪一目录使用,它归根结底就只有一个根目录, 一个独立且唯一的文件结构, Linux中每个分区都是用来组成整个文件系统的一部分

2. Linux采用了一种叫`载入` 的处理方法,它的整个文件系统中包含了一整套的文件和目录, 且将一个分区和一个目录联系起来, 这时要载入的一个分区将使它的存储空间在一个目录下获得

![image_2023-01-05-18-11-08](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-05-18-11-08.png)

- 查看所有设备挂载情况

命令:`lsblk` 或者`lsblk -f` 

参数: -f :显示文件系统信息

​		 -m :显示权限信息

![image-20240208124153263](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124153263.png)

- 硬盘说明

1. Linux硬盘分IDE硬盘和SCSI硬盘, 目前基本上是SCSI硬盘

2. 对于<font color='red'>IDE硬盘</font>, 驱动器标识符为"hdx~", 其中"hd"表明分区所在设备的类型, 这里是指IDE硬盘了."x"为盘号(a为基本盘, b为基本从属盘, c为辅助主盘, d为辅助从属盘), "~"代表分区, 前四个分区用数字1到4表示, 它们是主分区或扩展分区, 从5开始就是逻辑分区.例:hda3表示为第一个IDE硬盘上的第三个主分区或扩展分区, hdb2表示为第二个IDE硬盘上的第二个主分区或扩展分区

3. 对于<font color='red'>SCSI硬盘</font>则标识为"sdx~", SCSI硬盘是用"sd"来表示分区所在设备的类型的, 其余则和IDE硬盘的表示方法一样

### 增加硬盘

虚拟机增加硬盘的步骤

在[虚拟机]菜单中, 选择[设置], 然后设备列表里添加硬盘, 然后一路[下一步], 中间只有选择磁盘大小的地方需要修改, 直到完成, 然后<font color='red'>重启系统</font>(才能识别!)

![image_2023-01-05-18-18-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-05-18-18-02.png)

![image-20240208124211316](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124211316.png)

![image_2023-01-05-18-19-22](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-05-18-19-22.png)

![image_2023-01-05-18-20-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-05-18-20-05.png)

![image_2023-01-05-18-22-53](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-05-18-22-53.png)

> 分区

分区命令:`fdisk /dev/sdb`

/dev:字符设备文件

开始对/sdb分区

- m 显示命令列表

- p 显示磁盘分区 同 fdisk -l

- n 新增分区

- d 删除分区

- w 写入并退出

说明:开始分区后输入n, 新增分区, 然后选择p, 分区类型为主分区, 两次回车默认剩余全部空间, 最后输入w写入分区并退出, 若不保存退出输入q

![image_2023-01-05-18-33-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-05-18-33-34.png)

### 格式化磁盘

分区命令:`mkfs -t ext4 /dev/sdb1` 

![img](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/1286512-20190206141059483-251392832.png)

其中ext4是分区类型

![image-20240208124231127](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124231127.png)

挂载:将一个分区与一个目录联系起来

mount   设备名称  挂载目录

- 挂载分区:`mount /dev/sdb1 /newdisk` 

![image-20240208124240563](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124240563.png)

![image-20240208124255145](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124255145.png)

- 注意:<font color='red'>用命令挂载重启后会生效</font>

> 断开挂载点:注意切出挂载点目录

- 断开挂载点:`umount /dev/sdb1` 

![image-20240208124305347](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124305347.png)

![image-20240208124323320](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124323320.png)

> 永久挂载

通过修改`/etc/fstab` 实现挂载

添加完成后 执行mount -a 立刻生效

第一个数组:0/1 = 备份/不备份, 第二个数字:2/1/0 = 根目录/其它目录文件检查/不检查

![image_2023-01-05-19-07-32](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-05-19-07-32.png)

重启之后还是挂载的状态说明成功了

![image_2023-01-05-19-09-52](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-05-19-09-52.png)
