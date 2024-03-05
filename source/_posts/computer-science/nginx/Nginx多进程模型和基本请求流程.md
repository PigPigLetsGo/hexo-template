---
title: nginx多进程模型和基本请求流程
categories:
    - [计算机学科,nginx]
tags:
    - 计算机学科
    - nginx
    - 基础
---

![image_2023-01-28-11-16-56](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-11-16-56_20230225135830.png)

当我们通过地址发送请求之后,Nginx的主目录下有个sbin/nginx可执行文件,可执行文件启动之后开启Master主进程,这个主进程会把配置文件读取出来然后在进行一次校验,如果没有任何错误则会开启子进程

![image_2023-01-28-11-20-56](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-11-20-56_20230225135843.png)

主进程和子进程都启动完成之后

- 总结
1. nginx启动是多进程同时运行的由主进程fork出来的子进程,已经不单单是多线程了是多进程,主进程用于协调子进程

2. Worker读取配置文件

