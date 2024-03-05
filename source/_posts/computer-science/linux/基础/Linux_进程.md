---
title: linux进程
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

1. 在Linux中,每个<font color='red'>执行的程序</font>都被称为一个进程,每一个进程都分配一个ID号(pid,进程号)

2. 每个进程都可能以两种方式存在,<font color='red'>前台</font>与<font color='red'>后台</font>,所谓前台进程就是用户目前的屏幕上可以进行操作的,后台进程则是实际在操作,但由于屏幕上无法看到的进程,通常使用后台方式进行

3. 一般系统的服务都是以后台进程的方式存在(如,MySQL,Tomcat等等),而且都会常驻在系统中,直到关机才结束

> 显示系统执行的进程

ps命令是用来查看目前系统中,有哪些正在执行,以及它们执行的状况,可以不加任何参数

ps显示的信息选项:

| 字段 | 说明 |
|------|------|
| PID | 进程识别号 |
| TIY | 终端机号 |
| TIME | 此进程所消CPU时间 |
| CMD | 正在执行的命令或进程名 |

- 参数:

-a :显示当前终端的所有进程信息

-u :以用户的格式显示进程信息

-x :显示后台进程运行的参数

![image-20240208125421467](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208125421467.png)

> ps详解

1. 指令:ps -aux|grep xxx,比如我看看有没有sshd服务

2. 指令说明:

- System V展示风格

- USER  用户名称

- PID   进程号

- %CPU  进程占用CPU的百分比

- $MEM  进程所占用物理内存的百分比

- VSZ   进程占用的虚拟内存大小(单位:KB)

- RSS   进程占用的物理内存大小(单位:KB)

- TTY    终端名称,缩写

- STAT  进程状态:其中S:睡眠,s:表示该进程是会话的先导进程,N:表示进程拥有比普通优先级更低的优先级,R:表示正在运行,D:短期等待,Z:僵死进程,T:被跟踪或者被停止等等

- STARTED   进程的启动时间

- TIME  CPU时间,即进程使用CPU的总时间

- COMMAND   启动进程所用的命令和参数,如果过长会被截断显示

> ps -ef 是以全格式显示当前所有的进程

参数:-e 显示所有进程,-f 全格式

格式:`ps -ef|grep xxx` 

- 是BSD风格

- UID   用户ID

- PID   进程ID

- PPID  父进程ID

- C CPU用于计算执行优先级的因子,数值越大,表明进程是CPU密集型运算,执行优先级会降低;数值越小,表明进程是I/O密集型运算,执行优先级会提高

- STIME 进程启动时间

- TTY   完整的终端名称

- TIME  CPU时间

- CMD   启动进程所用的命令和参数

![image-20240208125432431](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208125432431.png)

- 查看sshd的父进程

![image_2023-01-06-13-49-12](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-06-13-49-12.png)

![image-20240208125446349](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208125446349.png)

<font style="color:red">**过滤掉grep的查询出来的grep进程**</font> 

```bash
ps -ef|grep Xxx.server|grep -v grep
```

可以看到Systemd的进程的PPID为0表示它没有父进程

> 终止进程kill和killall

- 介绍

若是某个进程执行一半需要停止时,或是已取消了很大的系统资源,此时可以考虑停止该进程,使用kill命令来完成此项任务

- 基本语法格式:

kill    [选项] 进程号(功能描述:通过进程号杀死进程)

killall 进程名称(功能描述:通过进程名称杀死进程,也支持通配符,这在系统因负载过大而变得很慢时很有用)

- 常用选项

-9  表示强迫进程立刻停止

> 查看进程树 pstree

- 基本语法格式:

pstree  [选项],可以更加直观的来看进程信息

- 常用选项

-p  显示进程的PID

-u  显示进程的所属用户

命令不存在执行命令进行安装:`sudo yum -y install psmisc` 

![image_2023-01-06-15-07-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-06-15-07-14.png)



