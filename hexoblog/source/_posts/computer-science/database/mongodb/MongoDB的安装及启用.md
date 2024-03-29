---
title: MongoDB的安装及启用
categories:
  - [计算机学科,database,mongodb]
tags:
  - 计算机学科
  - database
  - mongodb
---

>  MongoDB是一个开源，高性能，无模式的文档行数据库，NoSQL数据库产品的一种，是最像关系型数据库的非关系型数据库

## **什么时候使用MongoDB** .

-  淘宝用户数据
   -  存储位置：数据库
      特征：永久性存储，修改频度极低

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031654494.png" alt="image-20230508172121935" style="zoom:67%;" />

-  游戏装备数据，游戏道具数据
   -  存储位置：数据库，MongoDB
   -  特征：永久性存储与临时存储相结合，修改频度较高

<img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031654564.png" alt="image-20230508172300193" style="zoom:67%;" />

可以记录一些数据库，并且这个数据有很高的修改频率这种数据不太适合进数据库，而比较适合进MongoDB中

## MongoDB的下载

进入官网: [MongoDB](https://www.mongodb.com/try/download/community-kubernetes-operator) 

![image-20230514194355932](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031654073.png)

下载完成后进行解压:

![image-20230514194620731](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031654323.png)

进入解压后的文件夹中查看结构：

![image-20230514194734924](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031654953.png)

在MongoDB解压后的路面中创建一个存储数据的文件夹:data

![image-20230514194931585](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031655399.png)

在创建一个数据存储相关的文件:db

![image-20230514195004382](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031655089.png)

在bin中MongoDB的执行文件是: mongod.exe

![image-20230514195231717](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031655736.png)

在MongoDB目录中打开在当前位置下的Terminal(终端)

![image-20230514195607777](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031655531.png)

在Terminal中输入指令：mongod [指定存储位置] --dbpath=../data/db , 完整的指令如下： 

```shell
mongod --dbpath=../data/db
```

执行指令后的情况如下：这是相关数据的创建过程,犹如启动Redis中的redis-server一样

![image-20230514195504118](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031655165.png)

默认端口号为：27017

再查看db文件夹下的情况如下：

![image-20230514195841419](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031655245.png)

## 进入到MongoDB中

到MongoDB的bin目录下，执行指令：mongo 启动MongoDB

```shell
mongo
```

![image-20230514200129070](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031655792.png)

## 方式2：配置文件方式启动服务(重要)

部署项目使用该方式，以上的启动方式便以调试使用

在解压目录中新建config文件夹，该文件夹新建配置文件mongod.conf，内容参考如下：

![image-20230603091350281](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031656962.png)

![image-20230603091719879](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031656079.png)

```yaml
storage:
 #The directory where the mongod instance sotres its data.Default value is "\data\db" on Windows.
 dbPath: [配置路径，也就是到db目录的绝对路径]
```

注意：

配置文件中如果使用双引号，比如路径地址，自动会将双引号的内容转义，如果不转义，则会报错：

```
error-parsing-yaml-config-file-yaml-cpp-error-at-line-3-column-15-unknown-escape-character-d
```

解决：

1.  对`\`换成`/`或`//` 
2.  如果路径中没有空格，则无需加引号
3.  配置文件中不能以Tab分割字段

解决：

将其转换成空格。

启动方式：

```shell
mongod -f ../config/mongod.conf
或
mongod --config ../config/mongod.conf
```

![image-20230603094012602](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031656411.png)

更多参数配置：

```yaml
systemLong:
 destination: file
 #The path of the log file to which mondod mongos should send all diagnostic logging information
 path: [mongod.log文件绝对路径地址]
 logAppend: true
storage:
 journal:
  enabled: true
 #The directory where the mongod instance storage its data.Default value is "/data/db"
 dbPath: [配置路径，也就是到db目录的绝对路径]
net:
 #bindIp: 127.0.0.1
 port: 27017
setParameter:
 enableLocalhostAuthBypass: false
```

windows配置如下：

```
systemLog:
  destination: file
  path: D:\mongoDB\mongodb-win32-x86_64-windows-5.0.26\log\mongod.log  # 实际日志文件路径
  logAppend: true

storage:
  dbPath: D:\mongoDB\mongodb-win32-x86_64-windows-5.0.26\data  # 实际数据存储路径

net:
  bindIp: 0.0.0.0
  port: 27017  # MongoDB 默认端口号
```

## Shell连接(mongo命令)

在命令提示符输入以下Shell命令即可完成登录

```shell
mongo
或
mongo --host=127.0.0.1 --port=27017
```

查看已经有的数据库

```shell
> show databases
```

退出mongodb

```shell
> exit
```

更多参数可以通过帮助查看：

```shell
> mongo --help
```

提示：

MongoDB javascript Shell 是一个基于javascript的解释器，故是支持js程序的。

## Linux系统中的安装启动和连接

目标：在Linux中部署一个单机的MongoDB，作为生产环境下使用。

提示：和Windows下操作差不多。

步骤如下：

1.  先到官网下载压缩包：mongodb-linux-x86_64-4.0.10.tgz
2.  上传压缩包到Linux中 ，解压到当前目录

```
tar -xvf mongodb-linux-x86_64-4.0.10.tgz
```

3.  移动解压后的文件夹到指定的目录中：

```
mv mongodb-linxu-x86_64-4.0.10 /usr/local/mongodb
```

4.  新建几个目录，分别用来存储数据和日志：

```
#数据存储目录
mkdir -p /mongodb/single/data/db
#日志存储目录
mkdir -p /mongodb/single/log
```

5.  新建并修改配置文件

```
vi /mongodb/single/mongod.conf
```

```bash
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: [mongod.log绝对路径]比如："/mongodb/single/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/single/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
  fork: true
net:
  #服务实例绑定IP，默认是localhost
  bindIp: localhost192.168.0.2
  #bindIp
  #绑定的端口，默认是27017
  port: 27017
```

6.启动MongoDB服务

```shell
[dkx0@localhost]/% /usr/local/mongodb/bin/mongod -f /mongodb/single/log/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 2708
child process started successfully, parent exiting
```

注意：

如果启动后不是successfully，则是启动失败了。原因基本上就是配置文件有问题

通过进程来查看服务是否启动了：

```shell
[dkx0@localhost]/% ps -ef |grep mongod|grep -v grep
dkx0       2708      1  1 03:30 ?        00:00:05 /usr/local/mongodb/bin/mongod -f /mongodb/single/log/mongod.conf
```

7.分别使用mongo命令和compass工具来连接测试

提示：如果远程连接不上，需要配置防火墙放行，或直接关闭Linux防火墙

```shell
#查看防火墙状态
systemctl status firewalld
#临时关闭防火墙
systemctl stop firewalld
#开机禁止启动防火墙
systemctl disable firewalld
```

8.停止关闭服务

停止服务的方式有两种：快速关闭和标准关闭，下面依次说明：

一，快速关闭方法(快速，简单，数据可能会出错)

目标：通过系统的kill -9命令直接杀死进程

杀完要检查一下，避免有的没有杀掉

```shell
#通过进程编号关闭节点
kill -2 54410
```

[补充]

如果一旦是因为数据损坏，则需要进行如下操作：

1.删除lock文件

```shell
rm -f /mongodb/single/data/db/*.lock
```

2.修复数据：

```shell
/usr/local/mongdb/bin/mongod --repair --dbpath=/mongodb/single/data/db
```

二，标准的关闭方法(数据不容易出错，但麻烦)

目标：通过mongo客户端中的shutdownServer命令来关闭服务。

主要的操作步骤参考如下：

```shell
#客户端登录服务，注意，这里通过localhost登录，如果需要远程登录，必须先登录认证才行
mongo --port 27016
#切换到admin库
use admin
#关闭服务
db.shutdownServer()
```

连接linux中的mongo，指定IP和端口号即可。

![image-20230603154442420](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031656672.png)
