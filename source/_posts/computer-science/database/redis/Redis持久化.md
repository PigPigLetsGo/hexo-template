---
title: redis-持久化
categories:
    - [计算机学科,databse,redis]
tags:
    - 计算机学科
    - databse
    - redis
---

## Redis持久化

面试和工作, 持久化都是==重点==!

> Redis是内存数据库, 如果不将内存中的数据库状态保存到磁盘, 那么一旦服务器进程退出, 服务器中的数据库状态也会消失, 所以Redis提供了持久化功能

### RDB(Redis DataBase)

在主从复制中,rdb就是备用的, 从机上面!

> 什么是RDB

![image-20240105142624377](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051426460.png)

在指定的时间间隔内将内存中的数据集快照写入磁盘, 也就是行话讲的Snapshot快照, 它恢复时是将快照文件直接读到内存里.

Redis会单独创建(fork), 一个子进程来进行持久化, 会先将数据写入到一个临时文件中, 待持久化过程都结束了, 再用这个临时文件替换上次持久化好的文件, 整个过程中, 主进程是不进行任何IO操作的, 这就确保了极高的性能, 如果需要进行大规模数据的恢复, 且对于数据恢复的完整性不是非常敏感, 那RDB方式要比AOF方式更加的高效, RDB的缺点是最后一次持久化后的数据可能丢失.我们默认的就是RDB, 一般情况下不需要修改这个配置!

> 有时候在生产环境我们会将这个文件进行备份

==rdb保存的文件是 dump.rdb== 都是在配置文件中的快照中进行配置的!

![image-20240105142652722](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051426756.png)

![image-20240105142725442](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051427482.png)

> rdb触发机制

1.save的规则满足的情况下, 会自动触发rdb规则

2.执行flushall命令, 也会触发rdb规则

3.退出redis, 也会产生rdb文件

备份就自动生成一个 `dump.rdb` 文件

![image_2023-01-02-17-53-39](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051425051.png)

> 如何恢复rdb文件

1.只需要将rdb文件放在我们redis启动目录就可以, redis启动的时候会自动检查dump.rdb

2.查看存在的位置

```bash
127.0.0.1:6379> config get dir
1) "dir"
2) "/usr/local/bin"
127.0.0.1:6379> 
```

> 几乎它自己默认的配置就够用了, 但是我们还是需要去学习

#### 优点

1. 适合大规模的数据恢复

2. 对数据的完整性要求不高

#### 缺点

1. 需要一定的时间间隔进程操作, 如果redis意外宏基了, 这个最后一次修改的数据就没了

2. fork(分岔)进程的时候, 会占用一定的内存空间

### AOF(Append only File)

将我们的所有命令都记录下来, 类似与命令`history`, 恢复的时候就把这个文件全部在执行一遍!

![image_2023-01-02-17-17-21](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051425675.png)

以日志的形式来记录每个写操作, 将redis执行过的所有指令记录下来(读操作不记录), 只许追加文件但不可以改写文件, redis启动之初会读取该文件重新构建数据, 换言之, redis重启的话就根据日志文件的内容将写指令从前到后执行一次以完成数据的恢复工作

==Aof保存的是appendonly.aof文件== 

> APPEND

![image-20240105142742273](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051427331.png)

默认是不开启的,我们需要手动进行配置更改为开启yes

![image_2023-01-03-17-16-29](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051425208.png)

一般每间隔一秒修改就好了

![image_2023-01-03-17-19-15](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051425787.png)

> 重写规则说明

aof默认就是文件的无限追加, 文件会越来越大

![image-20240105142755534](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051427580.png)

![image_2023-01-03-17-23-03](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051425123.png)

如果aof文件大于64mb,就会fork一个新的进程来将我们的文件进行重写!

测试 如果破坏aof文件会怎么样

![image_2023-01-03-19-14-06](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051425280.png)

<font style="color:red"><u>报错: 拒绝连接</u></font> 

![image_2023-01-03-19-15-50](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051425831.png)

> 如果这个aof文件有错误, 这时候redis是启动不起来的, 我们需要修复这个aof文件<br>redis给我们提供了一个工具`redis-check-aof` 

![image_2023-01-03-19-20-44](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051426279.png)

查看aof文件里面没有错误命令了

![image_2023-01-03-19-21-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051426997.png)

再次重启后就可以正常启动了

![image_2023-01-03-19-23-33](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051426319.png)

> 优点和缺点

```bash
# appendfsync always #每次修改都会sync, 消耗性能
appendfsync everysec #每秒执行一次sync, 可能会丢失这1秒的数据
# appendfsync no #不执行sync, 这个时候操作系统自动同步数据, 速度最快
```

优点:

1. 每次修改都同步, 文件的完整性会更好

2. 每秒修改同步一次, 可能会丢失一秒的数据

3. 从不同步, 效率最高

缺点:

1. 相对于数据文件来说, aof远远大于rdb, 修复的速度也比rdb慢!

2. aof运行效率要比rdb慢, 所以我们redis默认的配置就是rdb持久化!

==扩展== 

1. rdb持久化方式能够在指定的时间间隔内对你的数据进行快照存储

2. aof持久化方式记录每次对服务器写的操作, 当服务器重启的时候会重新执行这些命令来恢复原始数据, aof命令以Redis协议追加保存每次写的操作到文件末尾, Redis还能对aof文件进行后台重写, 使得aof文件的体积不至于过大

3. 只做缓存, 如果你希望你的数据在服务器运行的时候存在, 你也可以不适用任何持久化

4. 同时开启两种持久化方式
    - 在这种情况下, 当redis重启的时候会优先载入aof文件来恢复原始的数据, 因为在通常情况下aof文件保存的数据集要比rdb文件保存的数据集要完整
    - rdb的数据不实时, 同时使用两者时服务器重启也只会找aof文件, 那要不要只使用aof呢?作者不建议, 因为rdb更适合用于备份数据库(aof在不断变化不好备份), 快速重启, 而且不会有aof可能潜在的bug, 留着作为一个万一的手段

5. 性能建议
    - 因为rdb文件只用作后背用途, 建议只在Slae上持久化rdb文件, 而且只要15分钟备份一次就够了, 只保留Save 900 1这条规则
    - 如果Enable AOF, 好处是在最恶劣的情况下也只会丢失不超过两秒数据, 启动脚本较简单只load自己的aof文件就可以了, 代价一是带来了持续的IO, 二是AOF rewrite的最后将rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的, 只要硬盘许可, 应该尽量减少AOF rewrite的频率, AOF重写的基础大小默认值64mb太小了, 可以设到5G以上, 默认超过原大小100%大小重写可以改到适当的数值 
    - 如果不Enable AOF, 仅靠Master-Slave Repllcation (主从复制) 实现高可用性也可以, 能省掉一大笔IO, 也减少了rewrite是带来的系统波动, 代价是如果Master/Slave同时倒掉, 会丢失十几分钟的数据, 启动脚本也要比较两个Master/Slave中的RDB文件,载入较新的那个, 微博就是这种架构
