---
title: linux服务管理
categories: 
      - [计算机学科,linux,服务管理]
tags:
      - 计算机学科
      - linux
---

> 介绍

服务(service)本质就是进程,但是是运行在后台的,通常会监听某个端口,等待其它程序的请求,比如(mysqld,sshd,防火墙等等),因此我们又成为守护进程,是Linux中非常重要的知识点

> service管理指令

1. service 服务名 [start|stop|restart|reload|status]

2. 在CentOS7.0后<font color='red'>很多服务不再使用service</font>,而是<font color='red'>systemctl</font>

3. service指令管理的服务在`/etc/init.d` 查看

`ls -l /etc/init.d/` 

![image_2023-01-06-15-37-26](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-06-15-37-26.png)

> 查看全部系统服务

`setup` 

如果提示不存在则进行安装`sudo yum -y install setuptool` 

安装完成后即可执行命令:setup

![image-20240208124524934](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124524934.png)

> 服务的运行级别(runlevel)

Linux系统有7种运行级别(runlevel):<font color='red'>常用的是级别3和5</font>

- 运行级别0:系统停机状态,系统默认运行级别不能设为0,否则不能正常启动

- 运行级别1:单用户工作状态,root权限,用于系统维护,禁止远程登陆

- 运行级别2:多用户状态(没有NFS),不持支网络

- 运行级别3:完全的多用户状态(有NFS),登录后进入控制台命令行模式

- 运行级别4:系统未使用,保留

- 运行级别5:X11控制台,登录后进入图形GUI模式

- 运行级别6:系统正常关闭并重启,默认运行级别不能设为6,否则不能正常启动

开机的流程说明:

![image-20240208124539115](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124539115.png)

CentOS7后台运行级别说明

在`/etc/initab` 

进行了简化,如下

multi-user.target:analogous to runlevel 3

graphical.target:analogous to runlevel 5

> chkconfig

1. 通过chkconfig命令可以给服务的各个运行级别设置自 启动/关闭

2. chkconfig指令管理的服务在`/etc/initab.d` 查看

3. 注意:CentOS7.0后,很多服务使用<font color='red'>systemctl</font>管理

chkconfig基本语法:

查看服务:chkconfig  --list[|grep xxx]

chkconfig   服务名  --list

chkconfig   --level 5   服务名  on/off

> 使用细节

chkconfig重新设置服务后自启动或关闭,需要重启机器reboot生效

![image_2023-01-06-17-29-29](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-06-17-29-29.png)

比如将network服务在指定的运行级别下让其关闭的则执行下面命令:

```bash
chkconfig --level 5 network off
```

> Systemctl管理指令

1. 基本语法:systemctl [start|stop|restart|status] 服务名

2. systemctl指令管理的服务在`/usr/lib/systemd/system` 查看

> systemctl设置服务的自启动状态

1. systemctl list-unit-files [|grep 服务名] (查看服务开机启动状态,grep可以进行过滤) 

2. systemctl enable 服务名(设置服务开机启动)

3. systemctl disable 服务名(关闭服务开机启动)

4. systemctl is-enabled 服务名(查询某个服务是否是自启动的)

5. systemctl status 服务名(查看服务的状态)

6. systemctl daemon reload(重新加载系统服务)

- 细节讨论

1. 关闭或者启用防火墙后,立即生效[telnet测试,某个端口即可]

telnet命令为Windows中的命令详细查看Windows中笔记[Windows:Telnet开启](..\..\..\Typoranote\Windows\开启telnet功能.md)

如果开启了防火墙而且没有将端口开放则外部主机访问不到

![image-20240208124554601](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208124554601.png)

2. 这种方式只是临时生效,当重启系统后,还是回归以前对服务的设置

3. 如果希望设置某个服务自启动或关闭永久生效,要使用systemctl [enable|disable] 服务名

> 打开或者关闭指定端口

在真正的生产环境,往往需要将防火墙打开,但问题来了,如果我们把防火墙打开,那么外部请求数据包就不能跟服务器监听端口通讯,这时,需要打开指定的端口,比如80,22,8080等

> firewall指令

打开端口:firewall-cmd --permanent --add-port=端口号/协议

关闭端口:firewall-cmd --permanent --remove-port=端口号/协议

重新载入,才能生效:firewall-cmd --reload

查询端口是否开放:firewall-cmd --query-port=端口/协议

查看所有开放端口:firewall-cmd --list-ports

查看端口号:netstat -ntlp|grep [端口号]

查看防火墙配置规则:iptables -nL

对其它主机开放指定端口:firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address port protocol="tcp" port="8080" accept" 

**开放指定端口**: firewall-cmd --permanent --zone=public --add-port=8080/tcp

- :warning:**注意**: 执行操作后执行重载:firewall-cmd --reload
