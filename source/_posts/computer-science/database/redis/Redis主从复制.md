---
title: redis-主从复制
categories:
    - [计算机学科,database,redis]
tags:
    - 计算机学科
    - database
    - redis
---

## Redis主从复制

<font size=4>概念</font> 

主从复制, 是指<font style="color:red"><u>==将一台Redis的服务器的数据==, ==复制到其它的Redis服务器==, ==前者称为主节点(master/leader)== , ==后者称为从节点(slave/follower)==; ==数据的复制是单向的==, ==只能由主节点到从节点==, ==Master以写为主, Slave以读为主==</u></font>  

<font style="color:red"><u>默认情况下, 每台Redis服务器都是主节点</u>, <u>且一个主节点可以有多个从节点(或没有从节点), 但一个从节点只能有一个主节点</u></font> 

主从复制的作用主要包括:

1. 数据冗余: 主从复制实现了数据的热备份, 是持久化之外的一种数据冗余方式

2. 故障恢复: 当主节点出现问题时, 可以由从节点服务器, 实现快速的故障恢复, 实际上是一种服务的冗余

3. 负载均衡: 在主从复制的基础上, 配合读写分离, 可以由主节点提供写服务, 由从节点提供读服务(即写Redis数据时应用连接主节点, 读Redis数据时应用连接从节点), 分担服务器负载;尤其是在写少读多的场景下, 通过多个从节点分担读负载, 可以大大提高Redis服务器的并发量

4. 高可用(集群)基石:除了上述作用以外, 主从复制还是哨兵和集群能够实施的基础, 因此说主从复制是Redis高可用的基础

一般来说, 要将Redis运用于工程项目中, 只使用一台Redis是万万不能的(因为宕机就完了, 一般要配三个就是:一主三从), 原因如下:

1. 从结构上, 单个Redis服务器会发生单点故障, 并且一台服务器需要处理所有的请求负载, 压力较大;

2. 从容量上, 单个Redis服务器内存容量有限, 就算一台Redis服务器内存容量为256G, 也不能将所有内存用作Redis存储内存, 一般来说, 单台Redis最大使用内存不应该超过20G

电商网站上的商品, 一般都是一次上传, 无数次浏览的, 说专业点就是"多读少写", 对于这种场景, 我们可以使如下这种架构:

一主三从

![image-20240105140950566](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051409674.png)

只要在公司中, 主从复制就是必须要使用的, 因为在真实的项目中不可能单机使用`Redis` !

### 环境配置

只配置从库, 不用配置主库!(默认就是主库)

查看当前库信息:`info replication` 

```bash
127.0.0.1:6379> info replication #查看当前库信息
# Replication
role:master #角色
connected_slaves:0 #从机数量
#随机生成的id
master_replid:dae261b703d0f68b44c7b95cf50be33d8a3f0c50
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379> 
```

> 测试单机多集群

配置多个从库配置文件主要改:`port|pidfile|dbfilename|logfile`  <font style="color:red">`logfile` **配置文件如果文件不存在则自动创建**</font> 

#### 操作步骤:

1.拷贝出3个配置文件,伪集群我们需要开三台redis来进行实验

![![image-20230327154220388](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051407515.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327154220388_20230327154614.png?v=1&type=image&token=V1:SNxXhfHSE20cGh0jieB-92RojNkNp9jz0k5M4JIB8ik)

2.配置==6379.conf==的信息主要配置: port     |     pidfile     |        dbfilename       |        logfile

![![image-20230327152554550](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051410294.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327152554550_20230327154627.png?v=1&type=image&token=V1:klL1texdf3iJ6yjEFDkq7J8VqDpq-RSpxH5cdk2lPAQ)

每个程序运行的pid都是不同的

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051411052.png)

每个redis的数据库数据都是不同的

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051411429.png)

每个redis的日志文件也是不同的

![![image-20230327152904341](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051411918.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327152904341_20230327154709.png?v=1&type=image&token=V1:fxfntNKPHAj74z8a_wZSGJAODAGrMARZHA15kXnob3A)

3.配置==6380.conf==的信息

![![image-20230327153341866](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051411750.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327153341866_20230327154723.png?v=1&type=image&token=V1:jnYNuO_P2RdhP1e_gybBBWx1NLpwr0ka1DoBvQmNPXI)

![![image-20230327153525882](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051411115.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327153525882_20230327154734.png?v=1&type=image&token=V1:dyEjnJRk8ryXskMZoBq29xs6w3az5jTnad5D_Jc10Zc)

![![image-20230327153535221](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051411533.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327153535221_20230327154746.png?v=1&type=image&token=V1:uQO8vtt2AGypUJ07at5K-7DEwKxUNtERKfkLaRZ22ns)

![![image-20230327153605193](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051411431.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327153605193_20230327154758.png?v=1&type=image&token=V1:VM6f9q3E6oWXwyAsXwuaacVUvmpQ3rFLM7jbRhuigNM)

4.配置==6381.conf==的信息

![![image-20230327153804788](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051418996.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327153804788_20230327154811.png?v=1&type=image&token=V1:B7632TiOjUzBa9T1eezMSTVQdGHO42vKIQwsJ0oDk1Q)

![![image-20230327153829068](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051419082.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327153829068_20230327154821.png?v=1&type=image&token=V1:bKBSYdVEmfM8wA2AkSWffBaIzXqRFfTQUdaRJJ-eNNQ)

![![image-20230327153843176](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051418328.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327153843176_20230327154833.png?v=1&type=image&token=V1:ijg9azq_Fb6FknmbIREeupwRe8-X_8i31nTqgKQ75wA)

![![image-20230327153903835](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051418065.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327153903835_20230327154845.png?v=1&type=image&token=V1:cRI6xeHzkCiw2TrbsamGy07_H12ZCUroy-ag3r_yVLM)

一次对应着配置文件启动redis

**启动后** 为伪集群redis配置完成了

![![image-20230327154403607](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051419904.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327154403607_20230327154857.png?v=1&type=image&token=V1:qySh2C_JQy12TXWSBLTHHpN96bEeEnI4yVeCGfkVAk8)

其它配置确保是否后台开启 ... 

修改完毕后启动三个Redis服务,可以通过`ps -ef|grep redis` 查看进程

![image_2023-01-04-11-01-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051420779.png)

<font size=4>一主二从</font> 

==默认情况下, 每台Redis服务器都是主节点== 我们一般情况下只用配置从机就好了

一主(79), 二从(80, 81)

从机认主机命令:`slaveof host port` host:主机的ip , port:主机中redis的端口

从机

```bash
127.0.0.1:6381> slaveof 127.0.0.1 6379
OK
127.0.0.1:6381> info replication
# Replication
role:slave #角色 slave:奴隶
master_host:127.0.0.1 #主机的ip
master_port:6379 #主机的端口port
master_link_status:up
master_last_io_seconds_ago:4
master_sync_in_progress:0
slave_repl_offset:56
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:3ef9f108a1abe59116f3d4716364383f51414a19
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:56
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:57
repl_backlog_histlen:0
127.0.0.1:6381> 
```

主机

```bash
127.0.0.1:6379> info replication
# Replication
role:master #角色 master:主人
connected_slaves:2 #连接的奴隶数量
slave0:ip=127.0.0.1,port=6380,state=online,offset=70,lag=1 #奴隶80的ip与port和一些信息
slave1:ip=127.0.0.1,port=6381,state=online,offset=70,lag=1 #奴隶81的ip与port和一些信息
master_replid:3ef9f108a1abe59116f3d4716364383f51414a19
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:70
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:70
127.0.0.1:6379> 
```

真实的主从配置应该==在配置文件中配置==, 这样的话==是永久的==, 我们这里使用的是==命令, 暂时的(重启后就没了)== 

![image_2023-01-04-13-10-51](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051420523.png)

> 细节

主机可以写, 从机只能读! 主机中的所有信息和数据, 都会自动被从机保存!

主机

```bash
127.0.0.1:6379> set k1 v1
OK
127.0.0.1:6379> get k1
"v1"
127.0.0.1:6379> 
```

从机

```bash
127.0.0.1:6380> get k1
"v1"
127.0.0.1:6380> set k2 v2
(error) READONLY You can't write against a read only replica.
127.0.0.1:6380> 
```

如果主机断开了, 从机依旧还是`slave` 奴隶状态, 依旧==可以获取值==, 但是还是==不能设置值==, 需要我们手动配置因为没有哨兵[^下面有解释 (哨兵模式)] 

<u>如果主机又连接回来了, 主机写了一个值, 从机还是可以读到的</u> <font style="color:red">**主机连接回来后还是主机从机会自动找主机认主**</font> 

主机

```bash
127.0.0.1:6379> shutdown
not connected> exit
 dkx@192  /usr/local/bin  
```

从机

```bash
127.0.0.1:6380> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6379
master_link_status:down
master_last_io_seconds_ago:-1
master_sync_in_progress:0
slave_repl_offset:8452
master_link_down_since_seconds:12
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:3ef9f108a1abe59116f3d4716364383f51414a19
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:8452
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:8452
127.0.0.1:6380> 
```

从机

```bash
127.0.0.1:6380> get k1
"v1"
127.0.0.1:6380> set k3 v3
(error) READONLY You can't write against a read only replica.
127.0.0.1:6380> 
```

如果是使用命令行来配置的主从, 这个时候如果重启了, 就会变回主机(也就是从机重启会变回主机状态)

从机变回了主机必然是不能在读到master写的值了,我们在将其设置为之前master的slave后依旧可以读到主机的值

> 复制原理

Slave启动成功连接到`master` 后会发送一个`syn` 命令

`Master` 接收到命令, 启动后台的存盘进程, 同时收集所有接收到的用于修改数据集命令, 在后台进程执行完毕之后, master将传送整个数据文件到slave, 并完成一次同步

<font color='green' size=4>全量复制</font>(只要连接则全部`syn`):而slave服务在接收到数据库文件数据后, 将其存盘并加载到内存中

<font color='green' size=4>增量复制</font>(连接后的依次读取):`Master` 继续将新的所有收集到的修改命令依次传给slave, 完成同步

但是只要是重新连接`master` , 一次完全同步(全量复制)将被自动执行!我们的数据一定可以在从机中看到!

> 层级链路

以链式的形式, 完成主从复制但是还是只有一个主机, 一旦主机宕机, 从机也不能写只能读

![image-20240105142153186](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051421265.png)

> 如果没有主人了, 这时就会候选出一个老大

==谋朝篡位== 

但是链式的只要`master` 宕机了死掉了, 我们可以让候选人来当老大依旧可以完成主从复制

执行命令:`slaveof no one` :命令解释:奴隶说:没有老大了, 我来当老大

```bash
#当老大前
127.0.0.1:6380> info replication
# Replication
role:slave #角色为奴隶
master_host:127.0.0.1
master_port:6379
master_link_status:down
master_last_io_seconds_ago:-1
master_sync_in_progress:0
slave_repl_offset:420
master_link_down_since_seconds:13
slave_priority:100
slave_read_only:1
connected_slaves:1
slave0:ip=127.0.0.1,port=6381,state=online,offset=420,lag=2
master_replid:08e30f50a346437d9ce38596868b3eb4a2d3765b
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:420
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:420
#当老大后
127.0.0.1:6380> slaveof no one
OK
127.0.0.1:6380> info replication
# Replication
role:master #角色为主人
connected_slaves:1
slave0:ip=127.0.0.1,port=6381,state=online,offset=420,lag=0
master_replid:5ceda312f5f005f7fed7c1e2e5bb1c787fcd267b
master_replid2:08e30f50a346437d9ce38596868b3eb4a2d3765b
master_repl_offset:420
second_repl_offset:421
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:420
127.0.0.1:6380> 
```

执行命令:`slaveof no one` 之后以链式的形式, 第二个当了主人, 而第三个第n个还是奴隶

但是 如果主人(主机)被修复了又回来了, 却不再是主人了手下的江山已经不再了已经是候选人的了, 除非让候选人重新归顺于主机

### 哨兵模式

(自动选举老大的模式)

> 概述

主从切换技术的方法是:当主服务器宕机后, 需要手动把一台服务器切换为主服务器, 这就需要人工干预, 费时费力, 还会造成一段时间内服务不可用, 这不是一种推荐的方式, 更多时候, 我们优先考虑哨兵模式, Redis从2.8开始正式提供了Sentinel(哨兵)架构来解决这个问题

<font style="color:red">**谋朝篡位**</font>的自动版, 能够后台监控主机是否故障, 如果故障了根据票数自动将从库转为主库

哨兵模式是一种特殊的模式, 首先Redis提供了哨兵的命令, 哨兵是一个独立的进程, 作为进程, 它会独立运行, 其原理是哨兵通过发送命令, 等待Redis服务器响应, 从而监控运行的多个Redis实例

![image-20240105142231100](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051422206.png)

这里的哨兵有两个作用

- 通过发送命令, 让Redis服务器返回监控其运行状态, 包括主服务器和从服务器

- 当哨兵监测到`master` 宕机, 会自动将slave切换成master, 然后通过发布订阅模式通知其它的从服务器, 修改配置文件, 让它们切换主机

然而一个哨兵进程对Redis服务器进行监控, 可能会出现问题, 为此, 我们可以使用多个哨兵进行监控, 各个哨兵之间还会进行监控,这样就形成了多哨兵模式

![image-20240105142300373](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051423496.png)

假设主服务器宕机, 哨兵1先检测到这个结果, 系统并不会马上进行failover (故障转移) 过程, 仅仅是哨兵1主观的认为主服务器不可用, 这个现象称为`主观下线` , 当后面的哨兵也检测到主服务器不可用, 并且数量达到一定值时, 那么哨兵之间就会进行一次投票, 投票的结果由一个哨兵发起, 进行failover[故障转移]操作, 切换成功后, 就会通过发布订阅模式, 让各个哨兵把自己监控的从服务器实现切换主机, 这个过程称为<font size=4>客观下线</font>

> 测试

我们目前的状态是 一主二从

1. 在redisconfig文件夹  (<u>如果没有则自己创建用于存放配置文件的</u>)  中创建sentinel.conf哨兵的配置文件进行配置

基本的配置(核心)sentinel的配置远远不止这一行

- 注意:如果Redis要设置密码都全有一致的密码否则都一致不要设置密码
    - 原因:设置密码连接从库需要配置masterauth连接有密码的库可以但是不能连接没有密码的所有密码设置与不设置一致确保能连接成功

```bash
#哨兵    检测   主机名[随意起名] 被监控的ip port 投票数
sentinel monitor myredis 127.0.0.1 6379 1
sentinel auth-pass myredis dkx #主机密码,如果主机设置了密码不配置这条命令连接不上
```

后面的数据1, 代表主机挂了, slave投票看让谁接替成为主机, 票数最多的, 就会成为主机

2. 启动哨兵

```bash
 dkx@192  /usr/local/bin  ./redis-sentinel redisconfig/sentinel.conf                                                    ✔  441  01:57:29
38944:X 04 Jan 2023 01:57:38.739 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
38944:X 04 Jan 2023 01:57:38.739 # Redis version=5.0.4, bits=64, commit=00000000, modified=0, pid=38944, just started
38944:X 04 Jan 2023 01:57:38.739 # Configuration loaded
38944:X 04 Jan 2023 01:57:38.740 # You requested maxclients of 10000 requiring at least 10032 max file descriptors.
38944:X 04 Jan 2023 01:57:38.740 # Server can't set maximum open files to 10032 because of OS error: Operation not permitted.
38944:X 04 Jan 2023 01:57:38.740 # Current maximum open files is 4096. maxclients has been reduced to 4064 to compensate for low ulimit. If you need higher maxclients increase 'ulimit -n'.
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 5.0.4 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in sentinel mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 26379
 |    `-._   `._    /     _.-'    |     PID: 38944
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

38944:X 04 Jan 2023 01:57:38.741 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
38944:X 04 Jan 2023 01:57:38.742 # Sentinel ID is 8ab32644bb1086cbb0bef3aa54b56016ca76422f
38944:X 04 Jan 2023 01:57:38.742 # +monitor master myredis 127.0.0.1 6379 quorum 1
38944:X 04 Jan 2023 01:57:38.743 * +slave slave 127.0.0.1:6380 127.0.0.1 6380 @ myredis 127.0.0.1 6379
38944:X 04 Jan 2023 01:57:38.744 * +slave slave 127.0.0.1:6381 127.0.0.1 6381 @ myredis 127.0.0.1 6379
```

我们将主机关闭

![image_2023-01-04-15-01-28](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051420959.png)

过一会之后哨兵发现主机挂了, 就会发起投票选举主机

```bash
38944:X 04 Jan 2023 02:01:43.540 # +sdown master myredis 127.0.0.1 6379
38944:X 04 Jan 2023 02:01:43.540 # +odown master myredis 127.0.0.1 6379 #quorum 1/1
38944:X 04 Jan 2023 02:01:43.540 # +new-epoch 1
38944:X 04 Jan 2023 02:01:43.540 # +try-failover master myredis 127.0.0.1 6379
38944:X 04 Jan 2023 02:01:43.541 # +vote-for-leader 8ab32644bb1086cbb0bef3aa54b56016ca76422f 1
38944:X 04 Jan 2023 02:01:43.541 # +elected-leader master myredis 127.0.0.1 6379
38944:X 04 Jan 2023 02:01:43.541 # +failover-state-select-slave master myredis 127.0.0.1 6379
38944:X 04 Jan 2023 02:01:43.604 # +selected-slave slave 127.0.0.1:6381 127.0.0.1 6381 @ myredis 127.0.0.1 6379
38944:X 04 Jan 2023 02:01:43.604 * +failover-state-send-slaveof-noone slave 127.0.0.1:6381 127.0.0.1 6381 @ myredis 127.0.0.1 6379
38944:X 04 Jan 2023 02:01:43.667 * +failover-state-wait-promotion slave 127.0.0.1:6381 127.0.0.1 6381 @ myredis 127.0.0.1 6379
38944:X 04 Jan 2023 02:01:44.496 # +promoted-slave slave 127.0.0.1:6381 127.0.0.1 6381 @ myredis 127.0.0.1 6379 #promoted-slave:提升奴隶选举81为主机
38944:X 04 Jan 2023 02:01:44.496 # +failover-state-reconf-slaves master myredis 127.0.0.1 6379
38944:X 04 Jan 2023 02:01:44.586 * +slave-reconf-sent slave 127.0.0.1:6380 127.0.0.1 6380 @ myredis 127.0.0.1 6379 
38944:X 04 Jan 2023 02:01:45.545 * +slave-reconf-inprog slave 127.0.0.1:6380 127.0.0.1 6380 @ myredis 127.0.0.1 6379
```

80

```bash
127.0.0.1:6380> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6381
master_link_status:down
master_last_io_seconds_ago:-1
master_sync_in_progress:0
slave_repl_offset:17761
master_link_down_since_seconds:164
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:b0b6ff7c006cb105240c9b032a9207481222ef21
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:17761
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:17761
127.0.0.1:6380> 
```

81

```bash
127.0.0.1:6381> info replication
# Replication
role:master
connected_slaves:0
master_replid:c490db106a8973cd53e490619faffa84d3085d78
master_replid2:b0b6ff7c006cb105240c9b032a9207481222ef21
master_repl_offset:27024
second_repl_offset:17762
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:169
repl_backlog_histlen:26856
127.0.0.1:6381> 
```

> 那主机又回来了呢?

主机连接后sentinel又发布了一个新的消息, 但是不幸的是主机被转为了奴隶:convert-to-slave:转换为奴隶

- 主机回来之后不再是`master` 但是不一定会认候选出来的`master` 作为主人 , 可能会选择候选人的候选人为主人

```bash
38944:X 04 Jan 2023 02:10:20.662 * +convert-to-slave slave 127.0.0.1:6379 127.0.0.1 6379 @ myredis 127.0.0.1 6381
```

优点:

1. 哨兵集群, 基于主从复制模式, 所有的主从配置优点, 它全有

2. 主从可以切换, 故障可以转移, 系统的可用性会更好

3. 哨兵模式就是主从模式的升级, 手动到自动,更加健壮

缺点:

1. Redis不好在线扩容的, 集群容量一旦到达上限, 在线扩容就十分麻烦

2. 实现哨兵模式的配置其实是很麻烦的, 里面有很多选择

> 哨兵模式的全部配置

```bash
#Example sentinel.conf
#哨兵sentinel实例运行的端口 默认26379
port 26379
#哨兵sentinel的工作目录
dir /tmp
#哨兵sentinel 监控的redis主节点的ip port
#master-name 可以自己命名的主节点名称, 只能由字母A-z, 数字0-9, 这三个字符".-_"组成
#quorum配置多少个sentinel哨兵统一认为master主节点失联, 那么这时客观上认为主节点失联了
#sentinel monitor <master-name> <ip> <redis-port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 2
#当在Redis实例中开启requirepass foobared授权密码, 这样所有连接Redis实例的客户端都要提供密码
#设置哨兵sentinel连接主从的密码, 注意必须为主从设置一样的验证密码
#sentinel auth-pass <master-name> <password>
sentinel auth-pass mymaster MySUPER--secret-0123password
#指定多少毫秒之后 主节点没有应答哨兵sentinel此时, 哨兵主观上认为主节点下线,默认30秒
#sentinel down-after-milliseconds <master-name> <milliseconds>
sentinel down-after-milliseconds mymaster 30000
#这个配置项指定了在发生failover主备切换时最多可以有多少个slave同时对新的master进行,同步
#这个数字越小,完成failover所需的时间就越长
#但是如果这个数字越大,就意味着越多的slave因为replication而不可用
#可以通过将这个值设为1 来保证每次只有一个slave处于不能处理命令请求的状态
#Sentinel parallel-syncs <master-name> <numslaves>
sentinel parallel-syncs mymaster 1
#故障转移的超时时间failover-timeout可以用在一下这些方面
#1. 同一个sentinel对同一个master两次failover之间的间隔时间
#2. 当一个slave从一个错误的master那里同步数据开始计算时间,知道slave被纠正为正确的master那里同步数据时
#3. 当想要取消一个正在进行的failover所需要的时间
#4. 当进行failover时, 配置所有slaves指向新的master所需的最大时间, 不过, 即使过了这个超时, slaves依然会被正确配置为指向master, 但是就不按parallel-syncs所配置的规则来了
#默认三分钟
#sentinel failover-timeout <master-name> <milliseconds>
sentinel failover-timeout mymaster 180000
#SCRIPTS EXECUTIOU
#配置当某一事件发生时所需执行的脚本,可以通过脚本来通知管理员,例如当系统运行不正常时发邮件通知相关人员
#对于脚本的运行结果有以下规则
#若脚本执行后返回1,那么该脚本稍后将会被再次执行, 重复次数目前默认为10
#若脚本执行后返回2, 或者比2更高一个返回值, 脚本将不会重复执行
#如果脚本在执行过程中由于收到系统中断信号被终止了, 则同返回值为1时的行为相同
#一个脚本的最大执行时间为60s, 如果超过这个时间,脚本将会被一个SIGKILL信号终止, 之后重新执行

#通知型脚本:当sentinel有任何警告级别的事件发生时, (比如说redis实例的主管失效和客观失效等等), 将会去调用这个脚本, 这时这个脚本应该通过邮件, SMS等方式去通知系统管理员关于系统不正常运行的信息,调用该脚本时,将传递给脚本两个参数, 一个是事件的类型, 一个是事件的描述, 如果sentinel.conf配置文件中配置了这个脚本路径, 那么必须保证这个脚本存在与这个路径, 并且是可执行的, 否则sentinel无法正常启动成功
#通知脚本
#shell编程
#sentinel notification-script <master-name> <script-path>
sentinel norification-script mymaster /var/redis/nofity.sh
#客户端重新配置主节点参数脚本
#当一个master由于failover而发生改变时,这个脚本将会被调用,通知相关的客户端关于master地址已经发生改变的信息
#以下参数将会在调用脚本时传递给脚本:
#<master-name> <role> <state> <from-ip> <from-port> <to-ip> <to-port>
#目前<state>总是"failover"
#<role>是"leader"或者"observer"中的一个
#参数from-ip, from-port, to-ip, to-port是用来和旧的master和新的master(即旧的slave)通信的
#这个脚本应该是通用的,能被多次调用,不是针对性的
#sentinel client-reconfig-script <master-name> <script-path>
sentinel client-reconfig-script mymaster /var/redis/reconfig.sh
```

#### 注意:

配置哨兵模式后配置文件会发生变化可能导致一些问题具体发生变化的内容

![![image-20230327202721502](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051420448.png)](Redis%E4%B8%BB%E4%BB%8E%E5%A4%8D%E5%88%B6_md_files/image-20230327202721502_20230327202857.png?v=1&type=image&token=V1:6HAI1c2eHTs9H9Q4rGHs5PXrhYHhYHCq4vefJFxiQq4)

