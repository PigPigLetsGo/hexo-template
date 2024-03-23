---
title: Linux打补丁,安装常用软件 yum源
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

# Linux打补丁,安装常用软件

1.配置官方源更新地址(打补丁下载软件的地址)

官方====>国内(阿里云,网易163,清华源)

CentOS7默认是从官方下载软件的,改为从阿里云网站下载

```bash
curl -s -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```

2.配置第三方epel源更新地址

```bash
curl -s -o /etc/yum. repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

一块使用

```bash
curl -s -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
curl -s -o /etc/yum. repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
```

3.更新所有软件到最新(注意以下说明再考虑是否要执行,不然后果自担)

```bash
yum update -y #总下载量279M
```

==工作中服务器正式环境之前可以执行,否则不能执行,容易破坏服务器==

要保证:

1)镜像备份

2)让服务的公司签字(不承担责任)

3)搭建正式环境的测试环境测试好

4.CentOS6和CentOS7都要安装的企业运维常用基础工具包

```bash
yum install nmap dos2unix lrzsz nc lsof wget -y
```

```bash
yum install tcpdump htop iftop iotop sysstat nethogs -y
```

CentOS7要安装的企业运维常用基础工具包

```bash
yum install psmisc net-tools bash-completion vim-enhanced -y
```

好玩的工具

```bash
yum install coway -y
```

