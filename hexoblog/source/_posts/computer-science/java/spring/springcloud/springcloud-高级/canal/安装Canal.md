---
title: 安装和配置Canal
categories:
    - [计算机学科,java,spring,springcloud,springcloud-高级,canal]
tags:
    - 计算机学科
    - springcloud
    - Canal
    - 基础
---

# 安装和配置Canal

下面我们就开启mysql的主从同步机制，让Canal来模拟salve

# 1.开启MySQL主从

Canal是基于MySQL的主从同步功能，因此必须先开启MySQL的主从功能才可以。

这里以之前用Docker运行的mysql为例：

## 1.1.开启binlog

打开mysql容器挂载的日志文件，我的在`/tmp/mysql/conf`目录:

![image-20210813153241537](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310210931806.png)

修改文件：

```sh
vi /tmp/mysql/conf/my.cnf
```

添加内容：

```ini
log-bin=/var/lib/mysql/mysql-bin
# 数据库名称
binlog-do-db=dkx
```

配置解读：

- `log-bin=/var/lib/mysql/mysql-bin`：设置binary log文件的存放地址和文件名，叫做mysql-bin
- `binlog-do-db=dkx`：指定对哪个database记录binary log events，这里记录heima这个库

最终效果：

```ini
[mysqld]
skip-name-resolve
character_set_server=utf8
datadir=/var/lib/mysql
server-id=1000
log-bin=/var/lib/mysql/mysql-bin
binlog-do-db=dkx
```

重启mysql

```sh
docker restart mysql
```

可以看到如下图中出现了指定文件表示成功！

![image-20231021100731994](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310211007103.png)

## 1.2.设置用户权限

接下来添加一个仅用于数据同步的账户，出于安全考虑，这里仅提供对heima这个库的操作权限。

```mysql
create user canal@'%' IDENTIFIED by 'canal';
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT,SUPER ON *.* TO 'canal'@'%' identified by 'canal';
FLUSH PRIVILEGES;
```



```sql
CREATE USER canal IDENTIFIED BY 'canal';
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON . TO 'canal'@'%';
GRANT ALL PRIVILEGES ON . TO 'canal'@'%' ;
FLUSH PRIVILEGES;
```

mysql 8.0.2 可以使用如下命令：

```sql
CREATE USER canal IDENTIFIED BY 'canal';
GRANT SELECT, SHOW VIEW, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';
FLUSH PRIVILEGES;
```



重启mysql容器即可

```
docker restart mysql
```



测试设置是否成功：在mysql控制台，或者Navicat中，输入命令：

```
show master status;
```

![image-20231021101552134](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310211015054.png) 



# 2.安装Canal



## 2.1.创建网络

我们需要创建一个网络，将MySQL、Canal、MQ放到同一个Docker网络中：

```sh
docker network create dkx
```

让mysql加入这个网络：

```sh
docker network connect dkx mysql
```

## 2.3.安装Canal

课前资料中提供了canal的镜像压缩包:

![image-20210813161804292](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310211145229.png) 

大家可以上传到虚拟机，然后通过命令导入：

```
docker load -i canal.tar
```



然后运行命令创建Canal容器：

```sh
docker run -p 11111:11111 --name canal \
-e canal.destinations=dkx1 \
-e canal.instance.master.address=192.168.249.128:3306  \
-e canal.instance.dbUsername=canal  \
-e canal.instance.dbPassword=canal  \
-e canal.instance.connectionCharset=UTF-8 \
-e canal.instance.tsdb.enable=true \
-e canal.instance.gtidon=false  \
-e canal.instance.filter.regex=dkx\\..* \
--network dkx1 \
-d canal/canal-server:v1.1.5
```



说明:

- `-p 11111:11111`：这是canal的默认监听端口
- `-e canal.instance.master.address=mysql:3306`：数据库地址(同一网络可以使用容器名互联)和端口，如果不知道mysql容器地址，可以通过`docker inspect 容器id`来查看
- `-e canal.instance.dbUsername=canal`：数据库用户名
- `-e canal.instance.dbPassword=canal` ：数据库密码
- `-e canal.instance.filter.regex=`：要监听的表名称

表名称监听支持的语法：

```
mysql 数据解析关注的表，Perl正则表达式.
多个正则之间以逗号(,)分隔，转义符需要双斜杠(\\) 
常见例子：
1.  所有表：.*   or  .*\\..*
2.  canal schema下所有表： canal\\..*
3.  canal下的以canal打头的表：canal\\.canal.*
4.  canal schema下的一张表：canal.test1
5.  多个规则组合使用然后以逗号隔开：canal\\..*,mysql.test1,mysql.test2 
```

查看启动日志，查看是否启动成功了：

```sh·
[root@localhost tmp]# docker logs -f canal
DOCKER_DEPLOY_TYPE=VM
==> INIT /alidata/init/02init-sshd.sh
==> EXIT CODE: 0
==> INIT /alidata/init/fix-hosts.py
==> EXIT CODE: 0
==> INIT DEFAULT
Generating SSH1 RSA host key: [  OK  ]
Starting sshd: [  OK  ]
Starting crond: [  OK  ]
==> INIT DONE
==> RUN /home/admin/app.sh
==> START ...
start canal ...
start canal successful
==> START SUCCESSFUL ...
```

查看canal是否与mysql进行建立连接了

步骤：

1、进入canal内部

```sh
[root@localhost tmp]# docker exec -it canal bash
[root@a02803287608 admin]#
```

2、使用命令监控canal的日志信息

```sh
[root@a02803287608 admin]# ll
total 8
-rwxr-xr-x. 1 admin admin 3941 Apr 19  2021 app.sh
drwxr-xr-x. 2 admin admin   43 Apr 19  2021 bin
drwxr-xr-x. 1 admin admin   66 Apr 19  2021 canal-server
-rwxr-xr-x. 1 admin admin  670 Apr 19  2021 health.sh
lrwxrwxrwx. 1 admin admin   44 Apr 19  2021 node_exporter -> /home/admin/node_exporter-0.18.1.linux-arm64
drwxr-xr-x. 2 admin admin   56 Jun  5  2019 node_exporter-0.18.1.linux-arm64
[root@a02803287608 admin]# tail -f canal-server/logs/canal/canal.log
```

打印日志如下

```sh
2023-10-21 10:31:37.324 [main] INFO  com.alibaba.otter.canal.deployer.CanalLauncher - ## set default uncaught exception handler
2023-10-21 10:31:37.376 [main] INFO  com.alibaba.otter.canal.deployer.CanalLauncher - ## load canal configurations
2023-10-21 10:31:37.392 [main] INFO  com.alibaba.otter.canal.deployer.CanalStarter - ## start the canal server.
2023-10-21 10:31:37.466 [main] INFO  com.alibaba.otter.canal.deployer.CanalController - ## start the canal server[172.22.0.3(172.22.0.3):11111]
2023-10-21 10:31:39.126 [main] INFO  com.alibaba.otter.canal.deployer.CanalStarter - ## the canal server is running now ......
```

可以看到启动没什么问题

查看mysql连接的日志 

```sh
[root@a02803287608 admin]# tail -f canal-server/logs/dkx1/dkx1.log
find start position successfully, EntryPosition[included=false,journalName=mysql-bin.000006,position=4,serverId=1000,gtid=<null>,timestamp=1697857809000] cost : 521ms , the next step is binlog dump
```
