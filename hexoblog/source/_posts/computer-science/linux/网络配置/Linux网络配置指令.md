---
title: linux网络配置指令
categories: 
      - [计算机学科,linux,网络配置]
tags:
      - 计算机学科
      - linux
---

> 指定ip

>  我们每次开机有可能ip地址分配不同如果想要固定ip则如下设置操作

说明:

直接修改配置文件来指定ip, 并可以连接到外网(程序员推荐)

编辑:`vi /etc/sysconfig/network-scripts/ifcfg-ens33` 

- ifcfg-ens33 文件说明

DEVICE=eth9     #接口名(设备,网卡)

HWADDR=00:0C:2x:6x:0x:xx        #MAC地址

TYPE=Ethernet       #网络类型(通常是Ethemet)

UUID=926a57ba-92c6-4231-bacb-f27e5e6a9f44       #随机

#系统启动的时候网络接口是否有效(yes/no)

NOBOOT=yes      #默认为no则不使用该配置启动网络接口,如果使用则改为yes

#IP的配置方法[none|static|bootp|dhcp(动态分配主机)] (引导时不使用协议|静态分配IP|BOOTP协议|DHCP协议)

BOOTPROTO=static

#IP地址

IPADDR=192.168.200.130

#网关

GATEWAY=192.168.200.2

#域名解析器

DNS1=192.168.200.2

> 重启网络服务器或者重启系统生效

```bash
service network restart 或者 reboot
```

再将虚拟机中的网络适配器更改对应的子网掩码和网关

![image-20240208124822023](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124822023.png)

![image-20240208124833571](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124833571.png)
