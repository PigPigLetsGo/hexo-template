---
title: redis-配置文件详解
categories:
    - [计算机学科,database,redis]
tags:
    - 计算机学科
    - database
    - redis
---

## Redis.Config配置文件详解

启动的时候,就通过配置文件来启动

工作中, 一些小小的配置, 可以让你脱颖而出

行家有没有, 出手就知道

> 单位

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051429088.png)

1.配置文件unit单位, 对大小写不敏感

> 包含

![image-20240105142938522](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051429577.png)

- 可以导入其它的配置文件

> 网络

![image_2023-01-02-14-59-41](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051428712.png)

- 如果想要让redis可以远程连接则需要将`bind` 更换为redis本机的ip地址或者`bind *` 更换为通配符

- 连接时输入Linux的ip即可连接

```bash
protected-mode yes #保护模式是否开启如果远程连接失败可以考虑将其设置为no如果考虑安全性则设为yes
port 6379 #端口号
```

> GENERAL 通用

```bash
daemonize yes #是否守护进程开启 默认为no , 我们希望退出后server还是运行的则需要设置为yes
pidfile /var/run/redis_6379.pid #如果以后台的方式运行, 我们就需要指定一个pid文件!
```

```bash
#日志
# Specify the server verbosity level.
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably) #生产环境适用
# warning (only very important / critical messages are logged)
loglevel notice
logfile "/usr/local/bin/redis-log.log" #日志文件的文件位置, 这个日志文件是自己创建的信息会打印到这个文件中
databases 16 #默认的数据库数量
```

```bash
always-show-logo yes #是否显示log, 下面为开启示例 默认开启
```
![image-20240105143005080](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051430154.png)

> SNAPSHOTTING 快照

持久化, 在规定的时间内, 执行了多少次操作, 则会持久化到文件`.rdb` , `.aof` 

redis是内存数据库, 如果没有持久化, 那么数据断电即失

```bash
#如果900秒内, 如果至少有一个key进行了修改, 这个时候就进行持久化操作
save 900 1
#如果300秒内, 如果至少有十个key进行了修改, 这个时候就进行持久化操作
save 300 10
#如果300秒内比如说高并发的时候, 如果至少有10000个key进行了修改, 这个时候就进行持久化操作
save 60 10000
```

- 我们之后学习持久化, 会自己定义这个测试!

```bash
stop-writes-on-bgsave-error yes #持久化出现错误之后, 是否继续进行工作, 默认开启
rdbcompression yes #是否压缩rdb文件, 默认开启, rdb就是持久化的文件, 开启需要消耗一些CPU的资源
rdbchecksum yes #保存rdb文件的时候进行rdb文件校验, 如果出错了可以自定进行一些修复
dir ./ #rdb默认保存目录
```

> REPLICATION 主从复制

```bash
replica-serve-stale-data yes #是否保存数据
replica-read-only yes #是否为只读
```

> SECURITY 安全

设置密码默认没有密码

![image_2023-01-02-16-25-06](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051428738.png)

密码为空

如果我们使用命令来设置密码的话当我们重启redis后密码就会失效所以我们对配置文件进行修改就不会失效了

![image-20240105142900035](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051429088.png)

- 补充:命令设置密码为:config set requirepass "密码"

- 获取:config get requirepass

- 登入:auth "密码"

- 注意:如果我们设置了密码那么可以看下是否可以ping如果可以就重启以下如果还可以就说明密码没有设置成功, 有密码不登陆执行命令就会报错如下图

![image_2023-01-02-16-36-45](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051428768.png)

> CLIENTS 客户端的限制

![image-20240105143019251](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051430304.png)

```bash
maxclients 10000 #设置能连接上客户端的数,最大1w个
```

> MEMORY MANAGEMENT 内存管理

![image-20240105143032841](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051430915.png)

```bash
maxmemory <bytes> #redis配置最大内存容量
maxmemory-policy noeviction #内存达到上限后的处理策略
1.volatile-lru:只对设置了过期时间的key进行LRU(默认值)
2.allkeys-lru:删除lru算法的key
3.volatile-random:随机删除即将过期key
4.allkeys-random:随机删除
5.volatile-ttl:删除即将过期的
6.noeviction:永不过期, 返回错误
```

> APPEND ONLY MODE aof配置

```bash
appendonly no #默认不开启, 为什么不开启呢 因为默认使用的是rdb方式持久化的, 在大部分情况下rdb完全够用
appendfilename "appendonly.aof" #持久化的文件的名称后缀名为.aof 而rdb后缀为.rdb
# appendfsync always #每次修改都会 sync(同步), 消耗性能
appendfsync everysec #每秒执行一次, sync(同步), 可能会丢失1秒的数据
# appendfsync no #不执行 sync(同步), 这个时候操作系统自己同步数据, 速度最快
```