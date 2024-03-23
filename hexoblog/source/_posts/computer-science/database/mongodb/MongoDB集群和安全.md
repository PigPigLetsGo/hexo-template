---
title: MongoDB集群和安全
categories:
  - [计算机学科,database,mongodb]
tags:
  - 计算机学科
  - database
  - mongodb
---

# MongoDB集群和安全

学习目标：

-  MongoDB的副本集：操作，主要概念，故障转移，选举规则
-  MongoDB的分片集群：概念，有点，操作，分片策略，故障转移
-  MongoDB的安全认证

## 副本集-Replica Sets

### 简介

MongoDB中的副本集(Replica Set)是一组维护相同数据集的mongod服务，副本集可提供冗余和高可用性，是所有生产部署的基础。

也可以说，副本集类似于有自动故障恢复功能的主从集群，通俗的讲就是多台机器进行同一数据的异步同步，从而使多台机器拥有同一数据的多个副本，并且当主库宕掉时在不需要用户干预的情况下自动切换其它备份服务器做主库，而且还可以利用副本服务器做只读服务器，实现读写分离，提高负载

1.冗余和数据可用性

复制提供冗余并提高数据可用性，通过在不同数据库服务器上提供多个数据副本，复制可提供一定级别的容错功能，以防丢失单个数据库服务器。

在某些情况下，复制可以提供增加的读取性能，因为客户端可以将读取操作发送到不同的服务上，在不同数据中心维护数据副本可以增加分布式应用程序的数据位置和可用性，您还可以为专用目的维护其它副本，例如灾难恢复，报告或备份。

2.MongoDB中的复制

副本集是一组维护相同数据集的mongod实例，副本集包含了多个数据承载节点和可选的一个仲载节点，在承载数据节点中，一个且仅一个成员被视为主节点，而其它节点被视为次要（从）节点。
主节点接受所有写操作，副本集只能有一个主要能够确认具有{w:"most"}写入关注的写入；虽然在某些情况下，另一个mongod实例可能暂时认为自己也是主要的，主要记录其操作日志中的数据集的所有更改，即oplog。

![image-20230605120258300](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031657901.png)

辅助(副本)节点复制主节点的oplog并将操作应用于其数据集，以使辅助节点的数据集反射主节点的数据集，如果主要人员不在，则符合条件的中学将举行选举以选出新的主要人员

3.主从复制和副本集区别

主从集群和副本集最大的区别就是副本集没有固定的"主节点"；整个集群会选出一个"主节点"，当其挂掉后，又在剩下的从节点中选中其它节点为"主节点"，副本集总有一个活跃点(主，primary)和一个或多个备份节点(从，secondary)。

## 副本集的三个角色

副本集有两种类型三种角色

两种类型：

-  主节点(primary)类型：数据操作的主要连接点，可读写。
-  次要(辅助，从)节点(Secondaries)类型：数据冗余备份节点，可以读或选举

三种角色

主要成员(Primary)：主要接受所有写操作，就是主节点

副本成员(Replicate)：从主节点通过复制操作以维护相同的数据集，即备份数据，不可写操作，但可以读操作(但需要配置)。是默认的一种从节点类型

仲裁者(Arbiter)：不保留任何数据的副本，只具有投票选举作用，当然也可以将仲载服务器维护为副本集的一部分，即副本成员同时也是仲载者，也是一种从节点类型

![image-20230605121334168](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031656412.png)

一==主==一==副==一==仲载== 

## 副本集的创建

### 第一步创建主节点

建立存放数据和日志的目录

```shell
#----------myrs
#主节点
mkdir -p /mongodb/replica_sets/myrs_27017/log \ &
mkdir -p /mongodb/replica_sets/myrs_27017/data/db
```

新建或修改配置文件：

```shell
vim /mongodb/replica_sets/myrs_27017/mongod.conf
```

往配置文件中写入配置信息

```yaml
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/replica_sets/myrs_27017/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/replica_sets/myrs_27017/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/replica_sets/myrs_27017/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27017
replication:
 #副本集的名称
 replSetName: myrs
```

### 第二步：创建副本节点

建立存放目录

```shell
#----------myrs
#主节点
mkdir -p /mongodb/replica_sets/myrs_27018/log \ &
mkdir -p /mongodb/replica_sets/myrs_27018/data/db
```

新建或修改配置文件:

```shell
vim /mongodb/replica_sets/myrs_27018/mongod.conf
```

往配置文件中写入配置信息

```yaml
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/replica_sets/myrs_27018/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/replica_sets/myrs_27018/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/replica_sets/myrs_27018/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27018
replication:
 #副本集的名称
 replSetName: myrs
```

### 第三步：创建仲载节点

建立存放目录

```shell
#----------myrs
#主节点
mkdir -p /mongodb/replica_sets/myrs_27019/log \ &
mkdir -p /mongodb/replica_sets/myrs_27019/data/db
```

新建或修改配置文件

```shell
vim /mongodb/replica_sets/myrs_27019/mongod.conf
```

往配置中写入配置信息

```yaml
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/replica_sets/myrs_27019/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/replica_sets/myrs_27019/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/replica_sets/myrs_27019/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27019
replication:
 #副本集的名称
 replSetName: myrs
```

启动节点

```shell
[dkx0@192]~% /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27017/log/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 5300
child process started successfully, parent exiting
[dkx0@192]~% /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27018/log/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 5367
child process started successfully, parent exiting
[dkx0@192]~% /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27019/log/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 5434
child process started successfully, parent exiting
[dkx0@192]~% 
```

查看mongod进程

```shell
[dkx0@192]~% ps -ef|grep mongod|grep -v grep
dkx0       5300      1  4 03:05 ?        00:00:04 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27017/log/mongod.conf
dkx0       5367      1  5 03:05 ?        00:00:04 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27018/log/mongod.conf
dkx0       5434      1  8 03:05 ?        00:00:05 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27019/log/mongod.conf
[dkx0@192]~% 
```

### 第四步：初始化配置副本集和主节点

使用客户端命令连接任意一个节点，但这里尽量要连接主节点(27017节点)：

在mongodb的bin目录下执行命令：

```shell
mongo --host=Linux主机地址 --port=27017
```

```shell
E:\MongoDB\mongodb-win32-x86_64-windows-5.0.17\bin>mongo --host=192.168.244.142 --port=27017
MongoDB shell version v5.0.17
connecting to: mongodb://192.168.244.142:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("7149483c-b50d-4e69-8086-b2c913d293e3") }
MongoDB server version: 5.0.18
================
Warning: the "mongo" shell has been superseded by "mongosh",
which delivers improved usability and compatibility.The "mongo" shell has been deprecated and will be removed in
an upcoming release.
For installation instructions, see
https://docs.mongodb.com/mongodb-shell/install/
================
---
The server generated these startup warnings when booting:
        2023-06-06T03:05:22.021-04:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
        2023-06-06T03:05:22.021-04:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never'
        2023-06-06T03:05:22.021-04:00: /sys/kernel/mm/transparent_hugepage/defrag is 'always'. We suggest setting it to 'never'
        2023-06-06T03:05:22.021-04:00: Soft rlimits for open file descriptors too low
        2023-06-06T03:05:22.021-04:00:         currentValue: 1024
        2023-06-06T03:05:22.021-04:00:         recommendedMinimum: 64000
---
> show dbs
uncaught exception: Error: listDatabases failed:{
        "topologyVersion" : {
                "processId" : ObjectId("647edab0454f9ab55a0c8f43"),
                "counter" : NumberLong(0)
        },
        "ok" : 0,
        "errmsg" : "not master and slaveOk=false",
        "code" : 13435,
        "codeName" : "NotPrimaryNoSecondaryOk"
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
Mongo.prototype.getDBs/<@src/mongo/shell/mongo.js:145:19
Mongo.prototype.getDBs@src/mongo/shell/mongo.js:97:12
shellHelper.show@src/mongo/shell/utils.js:956:13
shellHelper@src/mongo/shell/utils.js:838:15
@(shellhelp2):1:1
> show tables
uncaught exception: Error: listCollections failed: {
        "topologyVersion" : {
                "processId" : ObjectId("647edab0454f9ab55a0c8f43"),
                "counter" : NumberLong(0)
        },
        "ok" : 0,
        "errmsg" : "not master and slaveOk=false",
        "code" : 13435,
        "codeName" : "NotPrimaryNoSecondaryOk"
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
DB.prototype._getCollectionInfosCommand@src/mongo/shell/db.js:723:15
DB.prototype.getCollectionInfos@src/mongo/shell/db.js:771:16
shellHelper.show@src/mongo/shell/utils.js:943:9
shellHelper@src/mongo/shell/utils.js:838:15
@(shellhelp2):1:1
>
```

结果，连接上之后，很多命令无法使用，比如`show dbs，show tables`等，必须初始化副本集才行

准备初始化新的副本集：

语法：

```shell
rs.initiate(configuration)
```

选项：

| Parameter     | Type     | Description                                                  |
| ------------- | -------- | ------------------------------------------------------------ |
| configuration | document | Optional.A document that specifiles configuration for the new replicaset. if a configuration is not specifiled,MongoDB uses a default replicaset configuration |

[示例]

使用默认的配置来初始化副本集：

```shell
rs.initiate()
```

执行结果：

```shell
> rs.initiate()
{
        "info2" : "no configuration specified. Using a default configuration for the set",
        "me" : "192.168.244.142:27017",
        "ok" : 1
}
myrs:SECONDARY>
myrs:PRIMARY>
```

提示：

1.  "ok"的值为1，说明创建成功
2.  命令行提示符发生变化，变成了一个从节点角色，此时默认不能读写，稍等片刻，回车，变成主节点。

### 第五步：查看副本集的配置内容

说明：

返回包含当前副本集配置的文档

语法：

```shell
rs.conf(configuration)
```

提示：

`rs.config()`是该方法的别名

configuration：可选，如果没有位置，则使用默认主节点配置

[示例]

在27017上执行副本集中当前节点的默认节点配置

```shell
myrs:PRIMARY> rs.conf()
{
        "_id" : "myrs",
        "version" : 1,
        "term" : 1,
        "members" : [
                {
                        "_id" : 0,
                        "host" : "192.168.244.142:27017",
                        "arbiterOnly" : false,
                        "buildIndexes" : true,
                        "hidden" : false,
                        "priority" : 1,
                        "tags" : {

                        },
                        "secondaryDelaySecs" : NumberLong(0),
                        "votes" : 1
                }
        ],
        "protocolVersion" : NumberLong(1),
        "writeConcernMajorityJournalDefault" : true,
        "settings" : {
                "chainingAllowed" : true,
                "heartbeatIntervalMillis" : 2000,
                "heartbeatTimeoutSecs" : 10,
                "electionTimeoutMillis" : 10000,
                "catchUpTimeoutMillis" : -1,
                "catchUpTakeoverDelayMillis" : 30000,
                "getLastErrorModes" : {

                },
                "getLastErrorDefaults" : {
                        "w" : 1,
                        "wtimeout" : 0
                },
                "replicaSetId" : ObjectId("647edd5c454f9ab55a0c8fbf")
        }
}
myrs:PRIMARY>
```

说明：

1.  `"_id":"myrs"`：副本集的配置数据存储的主键值，默认就是副本集的名字
2.  `"members"`：副本集成员数组，此时只有一个：`"host":"180.76.159.126:27017"`，该成员不是仲载节点`"arbiterOnly":false`，优先级(权重值)：`"priority":1` 
3.  `"settings"`：副本集的参数配置。

提示：副本集配置的查看命令，本质是查询的是`system.replset`的表中的数据

### 第六步：查看副本集状态

检查副本集状态

说明：

返回包含状态信息的文档 ，此输出使用从副本集的其它成员发送的心跳包中获得的数据反映副本集的当前状态。

语法：

```shell
rs.status()
```

[示例]

```shell
myrs:PRIMARY> rs.status()
{
        "set" : "myrs",
        "date" : ISODate("2023-06-06T07:27:48.740Z"),
        "myState" : 1,
        "term" : NumberLong(1),
        "syncSourceHost" : "",
        "syncSourceId" : -1,
        "heartbeatIntervalMillis" : NumberLong(2000),
        "majorityVoteCount" : 1,
        "writeMajorityCount" : 1,
        "votingMembersCount" : 1,
        "writableVotingMembersCount" : 1,
        "optimes" : {
                "lastCommittedOpTime" : {
                        "ts" : Timestamp(1686036465, 1),
                        "t" : NumberLong(1)
                },
                "lastCommittedWallTime" : ISODate("2023-06-06T07:27:45.336Z"),
                "readConcernMajorityOpTime" : {
                        "ts" : Timestamp(1686036465, 1),
                        "t" : NumberLong(1)
                },
                "appliedOpTime" : {
                        "ts" : Timestamp(1686036465, 1),
                        "t" : NumberLong(1)
                },
                "durableOpTime" : {
                        "ts" : Timestamp(1686036465, 1),
                        "t" : NumberLong(1)
                },
                "lastAppliedWallTime" : ISODate("2023-06-06T07:27:45.336Z"),
                "lastDurableWallTime" : ISODate("2023-06-06T07:27:45.336Z")
        },
        "lastStableRecoveryTimestamp" : Timestamp(1686036455, 1),
        "electionCandidateMetrics" : {
                "lastElectionReason" : "electionTimeout",
                "lastElectionDate" : ISODate("2023-06-06T07:16:45.057Z"),
                "electionTerm" : NumberLong(1),
                "lastCommittedOpTimeAtElection" : {
                        "ts" : Timestamp(1686035805, 1),
                        "t" : NumberLong(-1)
                },
                "lastSeenOpTimeAtElection" : {
                        "ts" : Timestamp(1686035805, 1),
                        "t" : NumberLong(-1)
                },
                "numVotesNeeded" : 1,
                "priorityAtElection" : 1,
                "electionTimeoutMillis" : NumberLong(10000),
                "newTermStartDate" : ISODate("2023-06-06T07:16:45.091Z"),
                "wMajorityWriteAvailabilityDate" : ISODate("2023-06-06T07:16:45.106Z")
        },
        "members" : [
                {
                        "_id" : 0,
                        "name" : "192.168.244.142:27017",
                        "health" : 1,
                        "state" : 1,
                        "stateStr" : "PRIMARY",
                        "uptime" : 1348,
                        "optime" : {
                                "ts" : Timestamp(1686036465, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2023-06-06T07:27:45Z"),
                        "lastAppliedWallTime" : ISODate("2023-06-06T07:27:45.336Z"),
                        "lastDurableWallTime" : ISODate("2023-06-06T07:27:45.336Z"),
                        "syncSourceHost" : "",
                        "syncSourceId" : -1,
                        "infoMessage" : "",
                        "electionTime" : Timestamp(1686035805, 2),
                        "electionDate" : ISODate("2023-06-06T07:16:45Z"),
                        "configVersion" : 1,
                        "configTerm" : 1,
                        "self" : true,
                        "lastHeartbeatMessage" : ""
                }
        ],
        "ok" : 1,
        "$clusterTime" : {
                "clusterTime" : Timestamp(1686036465, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1686036465, 1)
}
myrs:PRIMARY>
```

说明：

1.  `"set":"myrs"`：副本集的名字
2.  `"myState":1`：说明状态正常
3.  `"members"`：副本集成员数组，此时只有一个：`"name":192.168.244.142:27017`，该成员的角色是`"stateStr":"PRIMARY"`，该节点是健康的：`"health":1` 

### 第七步：添加副本从节点

在主节点添加从节点，将其它成员加入到副本集

语法：

```shell
rs.add(host,arbiterOnly)
```

选项：

| Paraemter   | Type               | Description                                                  |
| ----------- | ------------------ | ------------------------------------------------------------ |
| host        | string or document | 要添加到副本集的新成员，指定为字符串或配置文档。1.如果是一个字符串，则需要指定新成员的主机名和可选的端口号。2.如果是一个文档，请指定在members数组中找到的副本集成员配置文档，您必须在成员配置文档中指定主机字段，有关文档配置字段的说明，详见下方文档："主机成员的配置文档" |
| arbiterOnly | boolean            | 可选的。仅在<host>值为字符串时适用。如果为true，则添加的主机是仲载者 |

主机成员的配置文档：

```shell
{
	_id: <int>,
	host: <String>,		//required
	arbiterOnly: <boolean>,
	buildIndexes: <boolean>,
	hidden: <boolean>,
	priority: <number>,
	tags: <document>,
	slaveDelay: <int>,
	votes: <number>
}
```

[示例]

将27018的副本节点添加到副本集中：

```shell
myrs:PRIMARY> rs.add("192.168.244.142:27018")
{
        "ok" : 1,
        "$clusterTime" : {
                "clusterTime" : Timestamp(1686037410, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1686037410, 1)
}
myrs:PRIMARY>
```

说明：

1.  `"ok":1`：说明添加成功。

查看副本集状态：

```shell
myrs:PRIMARY> rs.status()
{
        "set" : "myrs",
        "date" : ISODate("2023-06-06T07:47:19.794Z"),
        "myState" : 1,
        "term" : NumberLong(1),
        "syncSourceHost" : "",
        "syncSourceId" : -1,
        "heartbeatIntervalMillis" : NumberLong(2000),
        "majorityVoteCount" : 2,
        "writeMajorityCount" : 2,
        "votingMembersCount" : 2,
        "writableVotingMembersCount" : 2,
        "optimes" : {
                "lastCommittedOpTime" : {
                        "ts" : Timestamp(1686037635, 1),
                        "t" : NumberLong(1)
                },
                "lastCommittedWallTime" : ISODate("2023-06-06T07:47:15.489Z"),
                "readConcernMajorityOpTime" : {
                        "ts" : Timestamp(1686037635, 1),
                        "t" : NumberLong(1)
                },
                "appliedOpTime" : {
                        "ts" : Timestamp(1686037635, 1),
                        "t" : NumberLong(1)
                },
                "durableOpTime" : {
                        "ts" : Timestamp(1686037635, 1),
                        "t" : NumberLong(1)
                },
                "lastAppliedWallTime" : ISODate("2023-06-06T07:47:15.489Z"),
                "lastDurableWallTime" : ISODate("2023-06-06T07:47:15.489Z")
        },
        "lastStableRecoveryTimestamp" : Timestamp(1686037605, 1),
        "electionCandidateMetrics" : {
                "lastElectionReason" : "electionTimeout",
                "lastElectionDate" : ISODate("2023-06-06T07:16:45.057Z"),
                "electionTerm" : NumberLong(1),
                "lastCommittedOpTimeAtElection" : {
                        "ts" : Timestamp(1686035805, 1),
                        "t" : NumberLong(-1)
                },
                "lastSeenOpTimeAtElection" : {
                        "ts" : Timestamp(1686035805, 1),
                        "t" : NumberLong(-1)
                },
                "numVotesNeeded" : 1,
                "priorityAtElection" : 1,
                "electionTimeoutMillis" : NumberLong(10000),
                "newTermStartDate" : ISODate("2023-06-06T07:16:45.091Z"),
                "wMajorityWriteAvailabilityDate" : ISODate("2023-06-06T07:16:45.106Z")
        },
        "members" : [
                {
                        "_id" : 0,
                        "name" : "192.168.244.142:27017",
                        "health" : 1,
                        "state" : 1,
                        "stateStr" : "PRIMARY",
                        "uptime" : 2519,
                        "optime" : {
                                "ts" : Timestamp(1686037635, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2023-06-06T07:47:15Z"),
                        "lastAppliedWallTime" : ISODate("2023-06-06T07:47:15.489Z"),
                        "lastDurableWallTime" : ISODate("2023-06-06T07:47:15.489Z"),
                        "syncSourceHost" : "",
                        "syncSourceId" : -1,
                        "infoMessage" : "",
                        "electionTime" : Timestamp(1686035805, 2),
                        "electionDate" : ISODate("2023-06-06T07:16:45Z"),
                        "configVersion" : 3,
                        "configTerm" : 1,
                        "self" : true,
                        "lastHeartbeatMessage" : ""
                },
                {
                        "_id" : 1,
                        "name" : "192.168.244.142:27018",
                        "health" : 1,
                        "state" : 2,
                        "stateStr" : "SECONDARY",
                        "uptime" : 229,
                        "optime" : {
                                "ts" : Timestamp(1686037635, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDurable" : {
                                "ts" : Timestamp(1686037635, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2023-06-06T07:47:15Z"),
                        "optimeDurableDate" : ISODate("2023-06-06T07:47:15Z"),
                        "lastAppliedWallTime" : ISODate("2023-06-06T07:47:15.489Z"),
                        "lastDurableWallTime" : ISODate("2023-06-06T07:47:15.489Z"),
                        "lastHeartbeat" : ISODate("2023-06-06T07:47:18.948Z"),
                        "lastHeartbeatRecv" : ISODate("2023-06-06T07:47:19.436Z"),
                        "pingMs" : NumberLong(0),
                        "lastHeartbeatMessage" : "",
                        "syncSourceHost" : "192.168.244.142:27017",
                        "syncSourceId" : 0,
                        "infoMessage" : "",
                        "configVersion" : 3,
                        "configTerm" : 1
                }
        ],
        "ok" : 1,
        "$clusterTime" : {
                "clusterTime" : Timestamp(1686037635, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1686037635, 1)
}
myrs:PRIMARY>
```

### 第八步：添加仲载从节点

添加一个仲载节点到副本集

语法：

```shell
rs.addArb(host)
```

将27019的仲载节点添加到副本集中：

如果执行命令后没有反应解决办法如下：在主节点中:mys:PRIMARY执行命令

```shell
> db.adminCommand({"setDefaultRWConcern" : 1,"defaultWriteConcern" : {"w" : 2}})
```

```shell
myrs:PRIMARY> rs.addArb("192.168.244.142:27019")
{
        "ok" : 1,
        "$clusterTime" : {
                "clusterTime" : Timestamp(1686038253, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1686038253, 1)
}
myrs:PRIMARY>
```

说明：

1.  `"ok":1`：说明添加成功

查看副本集情况：

```shell
myrs:PRIMARY> rs.conf()
{
        "_id" : "myrs",
        "version" : 4,
        "term" : 1,
        "members" : [
                {
                        "_id" : 0,
                        "host" : "192.168.244.142:27017",
                        "arbiterOnly" : false,
                        "buildIndexes" : true,
                        "hidden" : false,
                        "priority" : 1,
                        "tags" : {

                        },
                        "secondaryDelaySecs" : NumberLong(0),
                        "votes" : 1
                },
                {
                        "_id" : 1,
                        "host" : "192.168.244.142:27018",
                        "arbiterOnly" : false,
                        "buildIndexes" : true,
                        "hidden" : false,
                        "priority" : 1,
                        "tags" : {

                        },
                        "secondaryDelaySecs" : NumberLong(0),
                        "votes" : 1
                },
                {
                        "_id" : 2,
                        "host" : "192.168.244.142:27019",
                        "arbiterOnly" : true,
                        "buildIndexes" : true,
                        "hidden" : false,
                        "priority" : 0,
                        "tags" : {

                        },
                        "secondaryDelaySecs" : NumberLong(0),
                        "votes" : 1
                }
        ],
        "protocolVersion" : NumberLong(1),
        "writeConcernMajorityJournalDefault" : true,
        "settings" : {
                "chainingAllowed" : true,
                "heartbeatIntervalMillis" : 2000,
                "heartbeatTimeoutSecs" : 10,
                "electionTimeoutMillis" : 10000,
                "catchUpTimeoutMillis" : -1,
                "catchUpTakeoverDelayMillis" : 30000,
                "getLastErrorModes" : {

                },
                "getLastErrorDefaults" : {
                        "w" : 1,
                        "wtimeout" : 0
                },
                "replicaSetId" : ObjectId("647edd5c454f9ab55a0c8fbf")
        }
}
myrs:PRIMARY>
```

"arbiterOnly" : true,说明是仲载节点

查看状态：

```shell
myrs:PRIMARY> rs.status()
{
        "set" : "myrs",
        "date" : ISODate("2023-06-06T08:04:25.601Z"),
        "myState" : 1,
        "term" : NumberLong(1),
        "syncSourceHost" : "",
        "syncSourceId" : -1,
        "heartbeatIntervalMillis" : NumberLong(2000),
        "majorityVoteCount" : 2,
        "writeMajorityCount" : 2,
        "votingMembersCount" : 3,
        "writableVotingMembersCount" : 2,
        "optimes" : {
                "lastCommittedOpTime" : {
                        "ts" : Timestamp(1686038655, 1),
                        "t" : NumberLong(1)
                },
                "lastCommittedWallTime" : ISODate("2023-06-06T08:04:15.625Z"),
                "readConcernMajorityOpTime" : {
                        "ts" : Timestamp(1686038655, 1),
                        "t" : NumberLong(1)
                },
                "appliedOpTime" : {
                        "ts" : Timestamp(1686038655, 1),
                        "t" : NumberLong(1)
                },
                "durableOpTime" : {
                        "ts" : Timestamp(1686038655, 1),
                        "t" : NumberLong(1)
                },
                "lastAppliedWallTime" : ISODate("2023-06-06T08:04:15.625Z"),
                "lastDurableWallTime" : ISODate("2023-06-06T08:04:15.625Z")
        },
        "lastStableRecoveryTimestamp" : Timestamp(1686038625, 1),
        "electionCandidateMetrics" : {
                "lastElectionReason" : "electionTimeout",
                "lastElectionDate" : ISODate("2023-06-06T07:16:45.057Z"),
                "electionTerm" : NumberLong(1),
                "lastCommittedOpTimeAtElection" : {
                        "ts" : Timestamp(1686035805, 1),
                        "t" : NumberLong(-1)
                },
                "lastSeenOpTimeAtElection" : {
                        "ts" : Timestamp(1686035805, 1),
                        "t" : NumberLong(-1)
                },
                "numVotesNeeded" : 1,
                "priorityAtElection" : 1,
                "electionTimeoutMillis" : NumberLong(10000),
                "newTermStartDate" : ISODate("2023-06-06T07:16:45.091Z"),
                "wMajorityWriteAvailabilityDate" : ISODate("2023-06-06T07:16:45.106Z")
        },
        "members" : [
                {
                        "_id" : 0,
                        "name" : "192.168.244.142:27017",
                        "health" : 1,
                        "state" : 1,
                        "stateStr" : "PRIMARY",
                        "uptime" : 3545,
                        "optime" : {
                                "ts" : Timestamp(1686038655, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2023-06-06T08:04:15Z"),
                        "lastAppliedWallTime" : ISODate("2023-06-06T08:04:15.625Z"),
                        "lastDurableWallTime" : ISODate("2023-06-06T08:04:15.625Z"),
                        "syncSourceHost" : "",
                        "syncSourceId" : -1,
                        "infoMessage" : "",
                        "electionTime" : Timestamp(1686035805, 2),
                        "electionDate" : ISODate("2023-06-06T07:16:45Z"),
                        "configVersion" : 4,
                        "configTerm" : 1,
                        "self" : true,
                        "lastHeartbeatMessage" : ""
                },
                {
                        "_id" : 1,
                        "name" : "192.168.244.142:27018",
                        "health" : 1,
                        "state" : 2,
                        "stateStr" : "SECONDARY",
                        "uptime" : 1254,
                        "optime" : {
                                "ts" : Timestamp(1686038655, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDurable" : {
                                "ts" : Timestamp(1686038655, 1),
                                "t" : NumberLong(1)
                        },
                        "optimeDate" : ISODate("2023-06-06T08:04:15Z"),
                        "optimeDurableDate" : ISODate("2023-06-06T08:04:15Z"),
                        "lastAppliedWallTime" : ISODate("2023-06-06T08:04:15.625Z"),
                        "lastDurableWallTime" : ISODate("2023-06-06T08:04:15.625Z"),
                        "lastHeartbeat" : ISODate("2023-06-06T08:04:24.037Z"),
                        "lastHeartbeatRecv" : ISODate("2023-06-06T08:04:24.064Z"),
                        "pingMs" : NumberLong(0),
                        "lastHeartbeatMessage" : "",
                        "syncSourceHost" : "192.168.244.142:27017",
                        "syncSourceId" : 0,
                        "infoMessage" : "",
                        "configVersion" : 4,
                        "configTerm" : 1
                },
                {
                        "_id" : 2,
                        "name" : "192.168.244.142:27019",
                        "health" : 1,
                        "state" : 7,
                        "stateStr" : "ARBITER",
                        "uptime" : 412,
                        "lastHeartbeat" : ISODate("2023-06-06T08:04:24.012Z"),
                        "lastHeartbeatRecv" : ISODate("2023-06-06T08:04:24.055Z"),
                        "pingMs" : NumberLong(0),
                        "lastHeartbeatMessage" : "",
                        "syncSourceHost" : "",
                        "syncSourceId" : -1,
                        "infoMessage" : "",
                        "configVersion" : 4,
                        "configTerm" : 1
                }
        ],
        "ok" : 1,
        "$clusterTime" : {
                "clusterTime" : Timestamp(1686038655, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1686038655, 1)
}
myrs:PRIMARY>
```

PRIMARY：主节点，SECONDARY：从节点，ARBITER：仲载节点

## 副本集的数据读写操作

目标：测试三个不同角色的节点的数据读写情况

登录主节点27017，写入和读取数据：

```shell
myrs:PRIMARY> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
myrs:PRIMARY> use articledb
switched to db articledb
myrs:PRIMARY> db
articledb
myrs:PRIMARY> try{db.comment.insert({"name":"张三","age":"18"})}catch(e){print(e)}
WriteResult({ "nInserted" : 1 })
myrs:PRIMARY> db.comment.find()
{ "_id" : ObjectId("647ee966333697b12ce7258a"), "name" : "张三", "age" : "18" }
myrs:PRIMARY>
```

登录从节点27018

```shell
myrs:SECONDARY> show dbs
uncaught exception: Error: listDatabases failed:{
        "topologyVersion" : {
                "processId" : ObjectId("647edabd93b3e543a0258e7d"),
                "counter" : NumberLong(5)
        },
        "ok" : 0,
        "errmsg" : "not master and slaveOk=false",
        "code" : 13435,
        "codeName" : "NotPrimaryNoSecondaryOk",
        "$clusterTime" : {
                "clusterTime" : Timestamp(1686039005, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1686039005, 1)
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
Mongo.prototype.getDBs/<@src/mongo/shell/mongo.js:145:19
Mongo.prototype.getDBs@src/mongo/shell/mongo.js:97:12
shellHelper.show@src/mongo/shell/utils.js:956:13
shellHelper@src/mongo/shell/utils.js:838:15
@(shellhelp2):1:1
myrs:SECONDARY>
```

发现，不能读取集合的数据。当前从节点只是一个备份，不是奴隶节点，无法读取数据，写当然更不行，因为默认情况下，从节点是没有读写权限的，可以增加读的权限，但需要进行设置。

设置读操作权限：

说明：

设置为奴隶节点，允许在从成员上运行读的操作

语法：

```shell
> rs.slaveOk()
#或
> rs.slaveOk(true)
#新版本中上面命令可能被弃用了，使用下面命令即可
rs.secondaryOk()
```

提示：

该命令是db.getMongo().setSlaveOk()的简化命令。

```shell
myrs:SECONDARY> rs.secondaryOk()
myrs:SECONDARY> show dbs
admin      0.000GB
articledb  0.000GB
config     0.000GB
local      0.000GB
myrs:SECONDARY>
myrs:SECONDARY> use articledb
switched to db articledb
myrs:SECONDARY> db.comment.find()
{ "_id" : ObjectId("647ee966333697b12ce7258a"), "name" : "张三", "age" : "18" }
myrs:SECONDARY>
```

可以看到可以将主节点插入的数据同步过来

但仍然不允许插入 ` "errmsg" : "not master"` 

```shell
myrs:SECONDARY> try{db.comment.insert({"name":"李四","age":"20"})}catch(e){print(e)}
WriteCommandError({
        "topologyVersion" : {
                "processId" : ObjectId("647edabd93b3e543a0258e7d"),
                "counter" : NumberLong(5)
        },
        "ok" : 0,
        "errmsg" : "not master",
        "code" : 10107,
        "codeName" : "NotWritablePrimary",
        "$clusterTime" : {
                "clusterTime" : Timestamp(1686039875, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1686039875, 1)
})
myrs:SECONDARY>
```

现在可实现了读写分离，让主插入数据，让从来读取数据

如果要取消奴隶节点的读取权限：

```shell
> rs.secondaryOk(false)
```

```shell
myrs:SECONDARY> rs.secondaryOk(false)
myrs:SECONDARY> show dbs
uncaught exception: Error: listDatabases failed:{
        "topologyVersion" : {
                "processId" : ObjectId("647edabd93b3e543a0258e7d"),
                "counter" : NumberLong(5)
        },
        "ok" : 0,
        "errmsg" : "not master and slaveOk=false",
        "code" : 13435,
        "codeName" : "NotPrimaryNoSecondaryOk",
        "$clusterTime" : {
                "clusterTime" : Timestamp(1686039975, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1686039975, 1)
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
Mongo.prototype.getDBs/<@src/mongo/shell/mongo.js:145:19
Mongo.prototype.getDBs@src/mongo/shell/mongo.js:97:12
shellHelper.show@src/mongo/shell/utils.js:956:13
shellHelper@src/mongo/shell/utils.js:838:15
@(shellhelp2):1:1
myrs:SECONDARY>
```

仲载者节点，不存放任何业务数据的(存放的是一些配置信息)，可以登录查看，就算设置了`rs.secondaryOk()`同样还是看不到数据

```shell
myrs:ARBITER> show dbs
uncaught exception: Error: listDatabases failed:{
        "topologyVersion" : {
                "processId" : ObjectId("647edaca021ba0c72e879407"),
                "counter" : NumberLong(1)
        },
        "ok" : 0,
        "errmsg" : "not master and slaveOk=false",
        "code" : 13435,
        "codeName" : "NotPrimaryNoSecondaryOk"
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
Mongo.prototype.getDBs/<@src/mongo/shell/mongo.js:145:19
Mongo.prototype.getDBs@src/mongo/shell/mongo.js:97:12
shellHelper.show@src/mongo/shell/utils.js:956:13
shellHelper@src/mongo/shell/utils.js:838:15
@(shellhelp2):1:1
myrs:ARBITER>
```

我们想要查看的话就会提示不是slaveOk不能读取，但是我们设置仲载可读呢

```shell
myrs:ARBITER> rs.secondaryOk()
myrs:ARBITER> show dbs
uncaught exception: Error: listDatabases failed:{
        "topologyVersion" : {
                "processId" : ObjectId("647edaca021ba0c72e879407"),
                "counter" : NumberLong(1)
        },
        "ok" : 0,
        "errmsg" : "node is not in primary or recovering state",
        "code" : 13436,
        "codeName" : "NotPrimaryOrSecondary"
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
Mongo.prototype.getDBs/<@src/mongo/shell/mongo.js:145:19
Mongo.prototype.getDBs@src/mongo/shell/mongo.js:97:12
shellHelper.show@src/mongo/shell/utils.js:956:13
shellHelper@src/mongo/shell/utils.js:838:15
@(shellhelp2):1:1
myrs:ARBITER>
```

报错提示翻译：节点未处于主状态或正在恢复状态

## 主节点的选举原则

MongoDB在副本集中，会自动进行主节点的选举，主节点选举的触发条件：

1.  节点故障
2.  主节点网络不可达(默认心跳信息为10秒)
3.  人工干预(rs.stepDown(600))

一旦触发选举，就要根据一定规则来选主节点

选举规则是根据票数来决定谁获胜：

-  票数最高，且获得了"大多数"成员的投票支持的节点获胜

   "大多数"的定义为：假设复制集内投票成员数量为N，则大多数为N/2 + 1，例如：3个投票成员，则大多数的值是2，当复制集内存活成员数量不足大多数时，整个复制集将无法选举出Primary，复制集将无法提供写服务，处于只读状态

-  若投票数相同，且都获得了"大多数"成员的投票支持的，数据新的节点获胜。

   数据的新旧是通过操作日志oplog来对比的

在获得票数的时候，优先级(priority)参数影响重大。

可以通过设置优先级(priority)来设置额外票数，优先级即权重，取值为0~1000，相当于可额外增加0`~`1000的票数，优先级的值越大，就越可能获得多数成员的投票(votes)数。指定较高的值可使成员更有资格成员主要成员，更低的值可使成员更不符合条件

默认情况下，优先级的值是1

```shell
myrs:SECONDARY> rs.conf()
{
        "_id" : "myrs",
        "version" : 4,
        "term" : 1,
        "members" : [
                {
                        "_id" : 0,
                        "host" : "192.168.244.142:27017",
                        "arbiterOnly" : false,
                        "buildIndexes" : true,
                        "hidden" : false,
                        "priority" : 1,
                        "tags" : {

                        },
                        "secondaryDelaySecs" : NumberLong(0),
                        "votes" : 1
                },
                {
                        "_id" : 1,
                        "host" : "192.168.244.142:27018",
                        "arbiterOnly" : false,
                        "buildIndexes" : true,
                        "hidden" : false,
                        "priority" : 1,
                        "tags" : {

                        },
                        "secondaryDelaySecs" : NumberLong(0),
                        "votes" : 1
                },
                {
                        "_id" : 2,
                        "host" : "192.168.244.142:27019",
                        "arbiterOnly" : true,
                        "buildIndexes" : true,
                        "hidden" : false,
                        "priority" : 0,
                        "tags" : {

                        },
                        "secondaryDelaySecs" : NumberLong(0),
                        "votes" : 1
                }
        ],
        "protocolVersion" : NumberLong(1),
        "writeConcernMajorityJournalDefault" : true,
        "settings" : {
                "chainingAllowed" : true,
                "heartbeatIntervalMillis" : 2000,
                "heartbeatTimeoutSecs" : 10,
                "electionTimeoutMillis" : 10000,
                "catchUpTimeoutMillis" : -1,
                "catchUpTakeoverDelayMillis" : 30000,
                "getLastErrorModes" : {

                },
                "getLastErrorDefaults" : {
                        "w" : 1,
                        "wtimeout" : 0
                },
                "replicaSetId" : ObjectId("647edd5c454f9ab55a0c8fbf")
        }
}
myrs:SECONDARY>
```

除了仲载节点其它priority(优先级)都是1，仲载不能读写数据它只是一个参与投票的人，仲载不可能为王

(要注意是，官方说了，仲载节点的优先级必须是0，不能是别的值，即不具备选举权，但具有投票权)

### 修改优先级

比如，下面提升从节点的优先级：

1.  先将配置导入cfg变量

    ```shell
    myrs:SECONDARY> cfg=rs.conf()
    ```

2.  然后修改值(ID号默认从0开始)

    ```shell
    myrs:SECONDARY> cfg.members[1].priority=2
    ```

    稍等片刻会重新开始选举

## 故障测试

### 副本节点故障测试

关闭27018副本节点：

发现，主节点和仲载节点对27018的心跳失败。因为主节点还在，因此，没有触发投票选举。

```shell
[dkx0@192]~% ps -ef|grep mongod|grep -v grep
dkx0       5300      1  1 03:05 ?        00:07:30 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27017/log/mongod.conf
dkx0       5367      1  1 03:05 ?        00:07:57 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27018/log/mongod.conf
dkx0       5434      1  1 03:05 ?        00:06:20 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27019/log/mongod.conf
[dkx0@192]~% kill -2 5367
[dkx0@192]~% ps -ef|grep mongod|grep -v grep
dkx0       5300      1  1 03:05 ?        00:07:31 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27017/log/mongod.conf
dkx0       5367      1  1 03:05 ?        00:07:58 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27018/log/mongod.conf
dkx0       5434      1  1 03:05 ?        00:06:20 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27019/log/mongod.conf
[dkx0@192]~% ps -ef|grep mongod|grep -v grep
dkx0       5300      1  1 03:05 ?        00:07:31 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27017/log/mongod.conf
dkx0       5434      1  1 03:05 ?        00:06:21 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27019/log/mongod.conf
[dkx0@192]~% 
```

kill -2 没有及时杀掉，证明了-2是等待任务完成自动杀掉而不是暴力杀掉的。

```shell
myrs:SECONDARY>
>
>
>
```

从节点被杀掉后按几下回车看下反应，已经没反应了。

如果此时，在主节点写入数据。

```shell
myrs:PRIMARY> db.comment.insert({"_id":"3","name":"马保国","age":"22"})
```

再启动从节点并赋予可读权限，会发现，主节点写入的数据，会自动同步给从节点。

```shell
myrs:SECONDARY> rs.secondaryOk()
myrs:SECONDARY> use articledb
switched to db articledb
myrs:SECONDARY> db.comment.find()
{ "_id" : ObjectId("647ee966333697b12ce7258a"), "name" : "张三", "age" : "18" }
{ "_id" : "3", "name" : "马保国", "age" : "22" }
{ "_id" : ObjectId("647f48485710428bcfbf9dbf"), "name" : "李四", "age" : "23" }
{ "_id" : "2", "name" : "123", "age" : "23" }
myrs:SECONDARY>
```

可以看到数据同步过来了"马保国"

### 主节点故障测试

关闭27017节点

```shell
[dkx0@192]~% kill -2 5300                   
[dkx0@192]~% ps -ef|grep mongod|grep -v grep
dkx0       5434      1  1 03:05 ?        00:06:34 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27019/log/mongod.conf
dkx0      29707      1  3 11:01 ?        00:00:06 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27018/log/mongod.conf
[dkx0@192]~% 
```

发现，从节点和仲载节点对27017的心跳失败，当失败超过10秒，此时因为没有了主节点，会自动发起投票。

```shell
myrs:PRIMARY>
>
>
```

27017节点已挂

而副本节点只有27018，因此，候选人只有一个就是27018，开始投票。

27019向27018投了一票，27018本身自带一票，因此共两票，超过了"大多数"

27019是仲载节点，没有选举权，27018不向其投票，其票数是0.

最终结果，27018成为主节点，具备读写功能。

```shell
myrs:SECONDARY>
myrs:PRIMARY>
```

回车刷新状态

在27018写入数据查看。

```shell
myrs:PRIMARY> db.comment.insert({"_id":"4","name":"弟中之弟","age":"123"})
myrs:PRIMARY> db.comment.find()
{ "_id" : ObjectId("647ee966333697b12ce7258a"), "name" : "张三", "age" : "18" }
{ "_id" : "3", "name" : "马保国", "age" : "22" }
{ "_id" : ObjectId("647f48485710428bcfbf9dbf"), "name" : "李四", "age" : "23" }
{ "_id" : "2", "name" : "123", "age" : "23" }
{ "_id" : "4", "name" : "弟中之弟", "age" : "123" }
myrs:PRIMARY>
```

再启动27017节点，发现27017变成了从节点，27018仍保持主节点。

```shell
[dkx0@192]~% /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27017/log/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 30161
child process started successfully, parent exiting
[dkx0@192]~% 
```

```shell
E:\MongoDB\mongodb-win32-x86_64-windows-5.0.17\bin>mongo --host=192.168.244.142 --port=27017
MongoDB server version: 5.0.18
================
Warning: the "mongo" shell has been superseded by "mongosh",
which delivers improved usability and compatibility.The "mongo" shell has been deprecated and will be removed in
an upcoming release.
For installation instructions, see
https://docs.mongodb.com/mongodb-shell/install/
================
---
The server generated these startup warnings when booting:
        2023-06-06T11:07:33.780-04:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
        2023-06-06T11:07:33.781-04:00: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never'
        2023-06-06T11:07:33.781-04:00: /sys/kernel/mm/transparent_hugepage/defrag is 'always'. We suggest setting it to 'never'
        2023-06-06T11:07:33.781-04:00: Soft rlimits for open file descriptors too low
        2023-06-06T11:07:33.781-04:00:         currentValue: 1024
        2023-06-06T11:07:33.781-04:00:         recommendedMinimum: 64000
---
myrs:SECONDARY>
```

登录27017节点，发现是从节点了需要赋予可读权限，数据自动从27018同步。

```shell
myrs:SECONDARY> use articledb
switched to db articledb
myrs:SECONDARY> db.comment.find()
Error: error: {
        "topologyVersion" : {
                "processId" : ObjectId("647f4bb2f95feeec6c69242c"),
                "counter" : NumberLong(3)
        },
        "ok" : 0,
        "errmsg" : "not master and slaveOk=false",
        "code" : 13435,
        "codeName" : "NotPrimaryNoSecondaryOk",
        "$clusterTime" : {
                "clusterTime" : Timestamp(1686064102, 1),
                "signature" : {
                        "hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
                        "keyId" : NumberLong(0)
                }
        },
        "operationTime" : Timestamp(1686064102, 1)
}
myrs:SECONDARY> rs.secondaryOk()
myrs:SECONDARY> db.comment.find()
{ "_id" : ObjectId("647ee966333697b12ce7258a"), "name" : "张三", "age" : "18" }
{ "_id" : ObjectId("647f48485710428bcfbf9dbf"), "name" : "李四", "age" : "23" }
{ "_id" : "2", "name" : "123", "age" : "23" }
{ "_id" : "3", "name" : "马保国", "age" : "22" }
{ "_id" : "4", "name" : "弟中之弟", "age" : "123" }
myrs:SECONDARY>
```

从而实现了高可用。

### 仲载节点和主节点故障

先关掉仲载节点27019

关掉现在的主节点27018

登录27017后，发现，27017仍然是从节点，副本集中没有主节点了，导致此时，副本集是只读状态，无法写入

为啥不选举了？因为27017的票数，没有获得大多数，既没有大于等于2，它只有默认的一票(优先级是1)

如果要出发选举，随便加入一个成员即可。

-  如果只加入27019仲载节点，则主节点一定是27017，因为没得选了，仲载节点不参与选举，但参与投票
-  如果只加入27018，会发起选举，因为27017和27018都是两票，按照谁数据新，谁当主节点。

### 仲载节点和从节点故障

先关掉仲载节点27019

关掉现在的副本节点27018

10秒后，27017主节点自动降级为副本节点(服务降级)

副本集不可写数据了，已经故障了。

## Compass连接副本集

如果使用云服务器需要修改配置中的主节点IP

```shell
var config = rs.config();
 config.members[0].host="192.168.244.142:27017";
 rs.reconfig(config)
```

compass连接：

![image-20230606234834193](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031658537.png)

输入后直接连接即可。

注意：

但是在Compass中即便连接的是secondary从节点但是还是可以插入数据的。

## SpringDataMongoDB连接副本集

副本集语法：

```shell
mongodb://host1,host2,host3/?connect=replicaSet&secondaryOk=true&replicaSet=副本集名字
```

其中：

-  secondaryOk=true：开启副本节点读的功能，实现读写分离。
-  connect=replicaSet：自动到副本集中选择读写的主机，如果secondaryOk是打开的，则实现了读写分离

[示例]

连接replica set三台服务器(端口27017，27018，和27019)，直接连接第一个服务器，无论是replica set一部分或者主服务器或者从服务器，写入操作应用在主服务器并且分布查询到从服务器。

修改配置文件：application.yml

连接副本集的时候就只能使用uri的格式了

```yaml
spring:
  #数据源配置
  data:
    mongodb:
      #主机地址
      #host: 192.168.244.142
      #数据库
      #database: articledb
      #端口号
      #port: 27017
      #也可以使用uri连接
      #uri: mongodb://192.168.244.142:27017/articledb
      #副本集的连接字符串
      uri:
        mongodb://192.168.244.142:27017,192.168.244.142:27018,192.168.244.142:27019/articledb?
        connect=replicaSet&secondaryOk=true&replicaSet=myrs
```

测试是否可用：查询全部数据

```java
@Test
public void test() {
   List<Comment> list = commentService.findCommentList();
   for(Comment i:list){
      System.out.println(i);
   }
}
-----------------RUN Result-----------------
Comment(id=647ee966333697b12ce7258a, content=null, publishtime=null, userid=null, nickname=null, createdatatim=null, likenum=null, replynum=null, state=null, parentid=null, articleid=null)
Comment(id=3, content=null, publishtime=null, userid=null, nickname=null, createdatatim=null, likenum=null, replynum=null, state=null, parentid=null, articleid=null)
Comment(id=647f48485710428bcfbf9dbf, content=null, publishtime=null, userid=null, nickname=null, createdatatim=null, likenum=null, replynum=null, state=null, parentid=null, articleid=null)
Comment(id=2, content=null, publishtime=null, userid=null, nickname=null, createdatatim=null, likenum=null, replynum=null, state=null, parentid=null, articleid=null)
Comment(id=4, content=null, publishtime=null, userid=null, nickname=null, createdatatim=null, likenum=null, replynum=null, state=null, parentid=null, articleid=null)
Comment(id=647f5445f81b0775f8f31924, content=null, publishtime=null, userid=null, nickname=null, createdatatim=null, likenum=null, replynum=null, state=null, parentid=null, articleid=null)
Comment(id=647f54b24cf08bc145fd6995, content=null, publishtime=null, userid=null, nickname=null, createdatatim=null, likenum=null, replynum=null, state=null, parentid=null, articleid=null)
```

id能查出来就证明可用，其它为null是因为实体类与表结构

注意：

主机必须是副本集中所有的主机，包括主节点，副本节点，仲载节点。

## 分片集群-Sharded Cluster

### 分片概念

分片(sharding)是一种跨多台机器分布数据的方法，MongoDB使用分片来支持具有非常大的数据集和高吞吐量操作的部署。

换句话说：分片(sharding)是指将数据拆分，将其分散存在不同的机器上的过程，有时也用分区(partitioning)来表示这个概念，将数据分散道不同的机器上，不需要功能强大的大型计算机就可以存储更多的数据，处理更多的负载。

具有大型数据集或吞吐量应用程序的数据库系统可以会挑战单个服务器的容量，例如，高查询率会耗尽服务器的CPU容量，工作集大小大于系统的RAM会强调磁盘驱动的I/O容量

有两种解决系统增长的办法：垂直扩展和水平扩展。

垂直扩展意味着增加单个服务器的容量，例如使用更强大的CPU，添加更多RAM或增加存储空间量。可用技术的局限性可能会限制单个机器对于给定工作负载而言而言足够强大，此外，基于云 的提供商基于可用的硬件配置具有硬性上限，结果，垂直缩放有实际的最大值

水平扩展意味着划分系统数据集并加载多个服务器，添加其他服务器以根据需要增加容量，虽然单个机器的总题速度或容量可能不高，但每台机器处理整个工作负载的子集，可能提供比单个高速大容量服务器更高的效率，扩展部署容量只需要根据需要添加额外的服务器，这可能比单个机器的高端硬件的总体成本更低，权衡是基础架构和部署维护的复杂性增加。

MongoDB支持通过分片进行水平扩展。

### 分片集群包含的组件

MongoDB分片集群包含以下组件：

-  分片(存储)：每个分片包含分片数据的子集，每个分片都可以部署为副本集
-  mongos(路由)：mongos充当查询路由器，在客户端应用程序和分片集群之间提供接口
-  config server("调度"的配置)：配置服务器存储群集的元数据和配置设置，从MongoDB3.4开始，必须将配置服务器部署为副本集(CSRS)

下图描述了分片集群中组件的交互：

![image-20230607171021552](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031659686.png)

MongoDB在集合级别对数据库进行分片，将集合数据分布在集群中的分片上

27018 if mongod is a shared member

27019 if mongod is a config server member

### 分片集群架构目标

两个分片节点副本集(3+3) + 一个配置节点副本集(3) + 两个路由节点(2) ，共11个服务节点。

![image-20230607171256122](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031659822.png)

### 分片(存储)节点副本集的创建

所有的配置文件都直接放到sharded_cluster的相应的子目录下，默认配置文件名字：mongod.conf

### 第一套副本集

准备存放数据和日志的目录

```shell
mkdir -p /mongodb/sharded_cluster/myshardrs01_27018/log \ &
mkdir -p /mongodb/sharded_cluster/myshardrs01_27018/data/db \ &

mkdir -p /mongodb/sharded_cluster/myshardrs01_27118/log \ &
mkdir -p /mongodb/sharded_cluster/myshardrs01_27118/data/db \ &

mkdir -p /mongodb/sharded_cluster/myshardrs01_27218/log \ &
mkdir -p /mongodb/sharded_cluster/myshardrs01_27218/data/db
```

==第一个服务== 

新建或修改配置文件：

```shell
vim /mongodb/sharded_cluster/myshardrs01_27018/mongod.conf
```

配置文件内容：

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs01_27018/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/sharded_cluster/myshardrs01_27018/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs01_27018/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27017
replication:
 #副本集的名称
 replSetName: myshardrs01
sharding:
 #分片角色
 clusterRole: shardsvr
```

sharding.clusterRole：

| Value     | Description                                                  |
| --------- | ------------------------------------------------------------ |
| configsvr | Start this instance as a config server. The instance starts on port 27019 by default |
| shardsvr  | Start this instance as a shard. The instance starts on port 27018 by default |

注意：

设置sharding.clusterRole需要mongod实例运行复制，要将实例部署为副本即成员，请使用replSetName设置并制定副本集的名称。

==第二个服务== 

新建或修改配置文件：

```shell
vim /mongodb/sharded_cluster/myshardrs01_27118/mongod.conf
```

配置文件内容：

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs01_27118/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/sharded_cluster/myshardrs01_27118/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs01_27118/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27118
replication:
 #副本集的名称
 replSetName: myshardrs01
sharding:
 #分片角色
 clusterRole: shardsvr
```

==第三个服务==  

新建或修改配置文件：

```shell
vim /mongodb/sharded_cluster/myshardrs01_27218/mongod.conf
```

配置文件内容：

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs01_27218/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/sharded_cluster/myshardrs01_27218/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs01_27218/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27218
replication:
 #副本集的名称
 replSetName: myshardrs01
sharding:
 #分片角色
 clusterRole: shardsvr
```

启动第一套副本集：一主一副一仲载

依次启动三个mongod服务：

```shell
➜  / /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27018/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 2821
child process started successfully, parent exiting
➜  / /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27118/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 2894
child process started successfully, parent exiting
➜  / /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27218/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 2963
child process started successfully, parent exiting
➜  /
```

查看启动进程情况：

```shell
➜  / ps -ef|grep mongod|grep -v grep
dkx0       2821      1  7 06:55 ?        00:00:03 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27018/mongod.conf
dkx0       2894      1 10 06:55 ?        00:00:02 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27118/mongod.conf
dkx0       2963      1 16 06:56 ?        00:00:02 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27218/mongod.conf
➜  / 
```

1.初始化副本集和创建主节点：

使用客户端命令连接任意一个节点，但这里尽量要连接主节点：

```shell
➜  ~ /usr/local/mongodb/bin/mongo  --host=192.168.244.142 --port=27018
```

执行初始化副本集命令：

```shell
> rs.initiate()
{
	"info2" : "no configuration specified. Using a default configuration for the set",
	"me" : "192.168.244.142:27018",
	"ok" : 1
}
myshardrs01:SECONDARY> 
myshardrs01:PRIMARY> 
```

添加从节点和仲载节点：

```shell
myshardrs01:PRIMARY> rs.add("192.168.244.142:27118")
myshardrs01:PRIMARY> rs.add("192.168.244.142:27218",true)
```

查看副本集的配置情况：

```shell
myshardrs01:PRIMARY> rs.conf()
{
	"_id" : "myshardrs01",
	"version" : 3,
	"term" : 1,
	"members" : [
		{
			"_id" : 0,
			"host" : "192.168.244.142:27018",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 1,
			"host" : "192.168.244.142:27118",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 2,
			"host" : "192.168.244.142:27218",
			"arbiterOnly" : true,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 0,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		}
	],
	"protocolVersion" : NumberLong(1),
	"writeConcernMajorityJournalDefault" : true,
	"settings" : {
		"chainingAllowed" : true,
		"heartbeatIntervalMillis" : 2000,
		"heartbeatTimeoutSecs" : 10,
		"electionTimeoutMillis" : 10000,
		"catchUpTimeoutMillis" : -1,
		"catchUpTakeoverDelayMillis" : 30000,
		"getLastErrorModes" : {
			
		},
		"getLastErrorDefaults" : {
			"w" : 1,
			"wtimeout" : 0
		},
		"replicaSetId" : ObjectId("648128e18751ababd297a5f4")
	}
}
myshardrs01:PRIMARY> 
```

### 第二套副本集

创建存放数据和日志的目录：

```shell
mkdir -p /mongodb/sharded_cluster/myshardrs01_27318/log \ &
mkdir -p /mongodb/sharded_cluster/myshardrs01_27318/data/db \ &

mkdir -p /mongodb/sharded_cluster/myshardrs01_27418/log \ &
mkdir -p /mongodb/sharded_cluster/myshardrs01_27418/data/db \ &

mkdir -p /mongodb/sharded_cluster/myshardrs01_27518/log \ &
mkdir -p /mongodb/sharded_cluster/myshardrs01_27518/data/db
```

==第一个服务== 

新建或修改配置文件：

```shell
➜  ~ vim /mongodb/sharded_cluster/myshardrs01_27318/mongod.conf
```

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs01_27318/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/sharded_cluster/myshardrs01_27318/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs01_27318/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27318
replication:
 #副本集的名称
 replSetName: myshardrs02
sharding:
 #分片角色
 clusterRole: shardsvr
```

==第二个服务== 

新建或修改配置文件：

```shell
➜  ~ vim /mongodb/sharded_cluster/myshardrs01_27418/mongod.conf
```

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs01_27418/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/sharded_cluster/myshardrs01_27418/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs01_27418/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27418
replication:
 #副本集的名称
 replSetName: myshardrs02
sharding:
 #分片角色
 clusterRole: shardsvr
```

==第三个服务== 

新建或修改配置文件：

```shell
➜  ~ vim /mongodb/sharded_cluster/myshardrs01_27518/mongod.conf
```

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs01_27518/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/sharded_cluster/myshardrs01_27518/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs01_27518/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27518
replication:
 #副本集的名称
 replSetName: myshardrs02
sharding:
 #分片角色
 clusterRole: shardsvr
```

启动服务：

```shell
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27318/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 5200
child process started successfully, parent exiting
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27418/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 5268
child process started successfully, parent exiting
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27518/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 5337
child process started successfully, parent exiting
➜  ~ 
```

查看进程：

```shell
➜  ~ ps -ef|grep mongod|grep -v grep
dkx0       2291      1  1 20:57 ?        00:00:52 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27018/mongod.conf
dkx0       2368      1  2 20:57 ?        00:01:11 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27118/mongod.conf
dkx0       2443      1  2 20:57 ?        00:01:08 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27218/mongod.conf

dkx0       5200      1  4 21:42 ?        00:00:03 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27318/mongod.conf
dkx0       5268      1  5 21:42 ?        00:00:03 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27418/mongod.conf
dkx0       5337      1  5 21:43 ?        00:00:02 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27518/mongod.conf
➜  ~ 
```

初始化副本集和创建主节点：

```shell
➜  ~ /usr/local/mongodb/bin/mongo --host=192.168.244.142 --port=27318
```

初始化副本集：

```shell
> rs.initiate()
{
	"info2" : "no configuration specified. Using a default configuration for the set",
	"me" : "192.168.244.142:27318",
	"ok" : 1
}
myshardrs02:SECONDARY> 
myshardrs02:PRIMARY> 
```

添加从节点和仲载节点：

```shell
myshardrs02:PRIMARY> rs.add("192.168.244.142:27418")
myshardrs02:PRIMARY> rs.add("192.168.244.142:27518",true)
```

查看状态：

```shell
myshardrs02:PRIMARY> rs.conf()
{
	"_id" : "myshardrs02",
	"version" : 3,
	"term" : 1,
	"members" : [
		{
			"_id" : 0,
			"host" : "192.168.244.142:27318",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 1,
			"host" : "192.168.244.142:27418",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 2,
			"host" : "192.168.244.142:27518",
			"arbiterOnly" : true,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 0,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		}
	],
	"protocolVersion" : NumberLong(1),
	"writeConcernMajorityJournalDefault" : true,
	"settings" : {
		"chainingAllowed" : true,
		"heartbeatIntervalMillis" : 2000,
		"heartbeatTimeoutSecs" : 10,
		"electionTimeoutMillis" : 10000,
		"catchUpTimeoutMillis" : -1,
		"catchUpTakeoverDelayMillis" : 30000,
		"getLastErrorModes" : {
			
		},
		"getLastErrorDefaults" : {
			"w" : 1,
			"wtimeout" : 0
		},
		"replicaSetId" : ObjectId("648132fed44efefc463628be")
	}
}
myshardrs02:PRIMARY>
```

到此两个分片副本集就搭建完成了

![image-20230608095125494](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031659810.png)

下面搭建配置服务

![image-20230608095211835](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031659497.png)

### 配置节点副本集的创建

创建存放数据和日志的目录：

```shell
mkdir -p /mongodb/sharded_cluster/myshardrs01_27019/log \ &
mkdir -p /mongodb/sharded_cluster/myshardrs01_27019/data/db \ &

mkdir -p /mongodb/sharded_cluster/myshardrs01_27119/log \ &
mkdir -p /mongodb/sharded_cluster/myshardrs01_27119/data/db \ &

mkdir -p /mongodb/sharded_cluster/myshardrs01_27219/log \ &
mkdir -p /mongodb/sharded_cluster/myshardrs01_27219/data/db
```

==第一个服务== 

新建或修改配置文件：

```shell
➜  ~ vim /mongodb/sharded_cluster/myshardrs01_27019/mongod.conf
```

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs01_27019/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/sharded_cluster/myshardrs01_27019/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs01_27019/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27019
replication:
 #副本集的名称
 replSetName: myshardrs
sharding:
 #分片角色
 clusterRole: configsvr
```

==第二个服务== 

新建或修改配置文件：

```shell
➜  ~ vim /mongodb/sharded_cluster/myshardrs01_27119/mongod.conf
```

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs01_27119/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/sharded_cluster/myshardrs01_27119/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs01_27119/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27119
replication:
 #副本集的名称
 replSetName: myshardrs
sharding:
 #分片角色
 clusterRole: configsvr
```

==第三个服务== 

新建或修改配置文件：

```shell
➜  ~ vim /mongodb/sharded_cluster/myshardrs01_27219/mongod.conf
```

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs01_27219/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
storage:
 #mongod实例存储其数据的目录。storage.dbPath设置仅适用于mongod。
 #The directory where the mongod instance stores its data.Defaul value is "/data/db"
 dbPath: "/mongodb/sharded_cluster/myshardrs01_27219/data/db"
 journal:
  #启用或禁用持久性日志以确保数据文件保持有效和可恢复
  enabled: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs01_27219/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27219
replication:
 #副本集的名称
 replSetName: myshardrs
sharding:
 #分片角色
 clusterRole: configsvr
```

启动服务：

```shell
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27019/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 7659
child process started successfully, parent exiting
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27119/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 7736
child process started successfully, parent exiting
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27219/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 7808
child process started successfully, parent exiting
➜  ~ 
```

查看进程：

```shell
➜  ~ ps -ef|grep mongod|grep -v grep
#分片副本集
dkx0       2291      1  1 20:57 ?        00:01:36 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27018/mongod.conf
dkx0       2368      1  2 20:57 ?        00:01:55 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27118/mongod.conf
dkx0       2443      1  1 20:57 ?        00:01:40 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27218/mongod.conf

dkx0       5200      1  1 21:42 ?        00:00:48 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27318/mongod.conf
dkx0       5268      1  2 21:42 ?        00:00:49 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27418/mongod.conf
dkx0       5337      1  1 21:43 ?        00:00:38 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27518/mongod.conf
#配置副本集
dkx0       7659      1  3 22:19 ?        00:00:08 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27019/mongod.conf
dkx0       7736      1  3 22:19 ?        00:00:07 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27119/mongod.conf
dkx0       7808      1  3 22:19 ?        00:00:07 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27219/mongod.conf
➜  ~ 
```

初始化配置服务：

```shell
> rs.initiate()
{
	"info2" : "no configuration specified. Using a default configuration for the set",
	"me" : "192.168.244.142:27019",
	"ok" : 1,
	"$gleStats" : {
		"lastOpTime" : Timestamp(1686191196, 1),
		"electionId" : ObjectId("000000000000000000000000")
	},
	"lastCommittedOpTime" : Timestamp(1686191196, 1)
}
myshardrs:SECONDARY> 
myshardrs:PRIMARY> 
```

添加从节点，配置服务中没有仲载节点：

```shell
myshardrs:PRIMARY> rs.add("192.168.244.142:27119")
myshardrs:PRIMARY> rs.add("192.168.244.142:27219")
```

查看状态：

```shell
myshardrs:PRIMARY> rs.conf()
{
	"_id" : "myshardrs",
	"version" : 5,
	"term" : 1,
	"members" : [
		{
			"_id" : 0,
			"host" : "192.168.244.142:27019",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"secondaryDelaySecs" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 1,
			"host" : "192.168.244.142:27119",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"secondaryDelaySecs" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 2,
			"host" : "192.168.244.142:27219",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"secondaryDelaySecs" : NumberLong(0),
			"votes" : 1
		}
	],
	"configsvr" : true,
	"protocolVersion" : NumberLong(1),
	"writeConcernMajorityJournalDefault" : true,
	"settings" : {
		"chainingAllowed" : true,
		"heartbeatIntervalMillis" : 2000,
		"heartbeatTimeoutSecs" : 10,
		"electionTimeoutMillis" : 10000,
		"catchUpTimeoutMillis" : -1,
		"catchUpTakeoverDelayMillis" : 30000,
		"getLastErrorModes" : {
			
		},
		"getLastErrorDefaults" : {
			"w" : 1,
			"wtimeout" : 0
		},
		"replicaSetId" : ObjectId("64813c5c4c6469e9d2fe9264")
	}
}
myshardrs:PRIMARY>
```



![image-20230608102950142](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031700512.png)

配置服务搭建完成！

分片和配置副本集服务都搭建完成了下面搭建路由节点服务，注意它使用的不是mongod服务了而是mongos服务

![image-20230608103603425](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031700137.png)

### 路由节点的创建和操作

#### 第一个路由节点的创建和连接

路由的主要作用就是分发不会存储具体的数据，所以不需要data目录

创建存放数据和日志的目录：

```shell
➜ ~ mkdir -p /mongodb/sharded_cluster/myshardrs_27017/log
```

新建或修改配置文件：

```shell
➜  ~ vim /mongodb/sharded_cluster/myshardrs_27017/mongos.conf
```

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs_27017/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs_27017/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27017
sharding:
 #指定配置节点副本集
 #        副本集名称 / 主机地址与端口号
 configDB: myconfigrs/192.168.244.142:27019,192.168.244.142:27119,192.168.244.142:27219
```

启动服务：

```shell
➜  ~ /usr/local/mongodb/bin/mongos -f /mongodb/sharded_cluster/myshardrs_27017/mongos.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 10092
child process started successfully, parent exiting
➜  ~ 
```

查看进程：

```shell
#分片副本集
➜  ~ ps -ef|grep mongod|grep -v grep
dkx0       2291      1  1 20:57 ?        00:02:07 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27018/mongod.conf
dkx0       2368      1  2 20:57 ?        00:02:26 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27118/mongod.conf
dkx0       2443      1  1 20:57 ?        00:02:02 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27218/mongod.conf

dkx0       5200      1  1 21:42 ?        00:01:19 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27318/mongod.conf
dkx0       5268      1  1 21:42 ?        00:01:19 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27418/mongod.conf
dkx0       5337      1  1 21:43 ?        00:01:00 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27518/mongod.conf
#配置服务副本集
dkx0       7659      1  2 22:19 ?        00:00:53 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27019/mongod.conf
dkx0       7736      1  2 22:19 ?        00:00:54 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27119/mongod.conf
dkx0       7808      1  2 22:19 ?        00:00:53 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27219/mongod.conf
#路由副本集
dkx0      10092      1  0 22:53 ?        00:00:00 /usr/local/mongodb/bin/mongos -f /mongodb/sharded_cluster/myshardrs_27017/mongos.conf
➜  ~ 
```

连接客户端：还是同样的操作，但是命令符提示为：mongos

```shell
➜  ~ /usr/local/mongodb/bin/mongo --host=192.168.244.142 --port=27017
MongoDB shell version v5.0.18
connecting to: mongodb://192.168.244.142:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("f9216227-99d6-4270-bb3e-b47ab59e205b") }
MongoDB server version: 5.0.18
================
Warning: the "mongo" shell has been superseded by "mongosh",
which delivers improved usability and compatibility.The "mongo" shell has been deprecated and will be removed in
an upcoming release.
For installation instructions, see
https://docs.mongodb.com/mongodb-shell/install/
================
---
The server generated these startup warnings when booting: 
        2023-06-07T22:53:18.509-04:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
---
mongos> 
```

此时，能查看数据库，但是不能写入数据否则报错：

```shell
mongos> show dbs
admin   0.000GB
config  0.000GB
mongos> use aabb
switched to db aabb
mongos> db
aabb
mongos> db.aa.insert({"name":"刘桑","age":"22"})
WriteCommandError({
	"ok" : 0,
	"errmsg" : "Database aabb could not be created :: caused by :: No shards found",
	"code" : 70,
	"codeName" : "ShardNotFound",
	"$clusterTime" : {
		"clusterTime" : Timestamp(1686193139, 1),
		"signature" : {
			"hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
			"keyId" : NumberLong(0)
		}
	},
	"operationTime" : Timestamp(1686193139, 1)
})
mongos> 
```

报错信息：Database aabb could not be created :: caused by :: No shards found

翻译：无法创建数据库 aabb ：： 由 ：： 未找到分片

解释：

>  我们新建路由节点的时候配置文件中只指定了配置服务副本集如下：
>
>  ```shell
>  sharding:
>   #指定配置节点副本集
>   #        副本集名称 / 主机地址与端口号
>   configDB: myconfigrs/192.168.244.142:27019,192.168.244.142:27119,192.168.244.142:27219
>  ```
>
>  mongos启动的时候指定了副本集代表路由和配置服务是联通的，但是没有分片不能存储数据，因为最终存储数据是分片

![image-20230608110542590](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031700965.png)

#### 在路由节点上进行分片配置操作

使用命令添加分片：

1.添加分片：

语法：

```shell
sh.addShard("IP:Port")
```

将第一套分片副本集添加进来：

```shell
mongos> sh.addShard("myshardrs01/192.168.244.142:27018,192.168.244.142:27118,192.168.244.142:27218")
{
	"shardAdded" : "myshardrs01",
	"ok" : 1,
	"$clusterTime" : {
		"clusterTime" : Timestamp(1686193852, 1),
		"signature" : {
			"hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
			"keyId" : NumberLong(0)
		}
	},
	"operationTime" : Timestamp(1686193852, 1)
}
mongos> 
```

继续将第二套分片副本集添加进来：

```shell
mongos> sh.addShard("myshardrs02/192.168.244.142:27318,192.168.244.142:27418,192.168.244.142:27518")
{
	"shardAdded" : "myshardrs02",
	"ok" : 1,
	"$clusterTime" : {
		"clusterTime" : Timestamp(1686194172, 3),
		"signature" : {
			"hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
			"keyId" : NumberLong(0)
		}
	},
	"operationTime" : Timestamp(1686194172, 3)
}
mongos> 
```

查看状态：可以看到shards中有两个分片副本集了

"host" : "myshardrs01/192.168.244.142:27018,192.168.244.142:27118" 没有把仲载节点加进来

因为仲载不存储数据所以分片的时候没必要知道它，但是使用sh.addShard的时候还是必须带上的

```shell
mongos> sh.status()
--- Sharding Status --- 
  sharding version: {
  	"_id" : 1,
  	"minCompatibleVersion" : 5,
  	"currentVersion" : 6,
  	"clusterId" : ObjectId("64813c5c4c6469e9d2fe9269")
  }
  shards:
        {  "_id" : "myshardrs01",  "host" : "myshardrs01/192.168.244.142:27018,192.168.244.142:27118",  "state" : 1,  "topologyTime" : Timestamp(1686193851, 2) }
        {  "_id" : "myshardrs02",  "host" : "myshardrs02/192.168.244.142:27318,192.168.244.142:27418",  "state" : 1,  "topologyTime" : Timestamp(1686194172, 1) }
  active mongoses:
        "5.0.18" : 1
  autosplit:
        Currently enabled: yes
  balancer:
        Currently enabled: yes
        Currently running: yes
        Failed balancer rounds in last 5 attempts: 0
        Migration results for the last 24 hours: 
                55 : Success
  databases:
        {  "_id" : "articledb",  "primary" : "myshardrs01",  "partitioned" : false,  "version" : {  "uuid" : UUID("b94e53c7-d498-49b8-8965-1aafcab2df18"),  "timestamp" : Timestamp(1686193851, 3),  "lastMod" : 1 } }
        {  "_id" : "config",  "primary" : "config",  "partitioned" : true }
                config.system.sessions
                        shard key: { "_id" : 1 }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	969
                                myshardrs02	55
                        too many chunks to print, use verbose if you want to force print
mongos>
```

提示：

如果添加分片失败，需要先手动移除分片，检查添加分片的信息正确性后，再次添加分片

移除分片参考：

```shell
use admin
db.runcommand({removeShard:"myshardrs02"})
```

注意： 如果只剩下最后一个shard，是无法删除的

移除时会自动转移分片数据，需要一个时间过程

完成后，再次执行删除分片命令才能真正删除

2.开启分片功能：sh.enableSharding("库名")，sh.shardCollection("库名.集合名",{"key":1}")

在mongos上的articledb数据库配置sharding:	

```shell
mongos> sh.enableSharding("articledb")
{
	"ok" : 1,
	"$clusterTime" : {
		"clusterTime" : Timestamp(1686194695, 38),
		"signature" : {
			"hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
			"keyId" : NumberLong(0)
		}
	},
	"operationTime" : Timestamp(1686194695, 38)
}
mongos> 
```

查看分片状态：

```shell
mongos> sh.status()
--- Sharding Status --- 
  sharding version: {
  	"_id" : 1,
  	"minCompatibleVersion" : 5,
  	"currentVersion" : 6,
  	"clusterId" : ObjectId("64813c5c4c6469e9d2fe9269")
  }
  shards:
        {  "_id" : "myshardrs01",  "host" : "myshardrs01/192.168.244.142:27018,192.168.244.142:27118",  "state" : 1,  "topologyTime" : Timestamp(1686193851, 2) }
        {  "_id" : "myshardrs02",  "host" : "myshardrs02/192.168.244.142:27318,192.168.244.142:27418",  "state" : 1,  "topologyTime" : Timestamp(1686194172, 1) }
  active mongoses:
        "5.0.18" : 1
  autosplit:
        Currently enabled: yes
  balancer:
        Currently enabled: yes
        Currently running: no
        Failed balancer rounds in last 5 attempts: 0
        Migration results for the last 24 hours: 
                480 : Success
  databases:
        {  "_id" : "articledb",  "primary" : "myshardrs01",  "partitioned" : true,  "version" : {  "uuid" : UUID("b94e53c7-d498-49b8-8965-1aafcab2df18"),  "timestamp" : Timestamp(1686193851, 3),  "lastMod" : 1 } }
        {  "_id" : "config",  "primary" : "config",  "partitioned" : true }
                config.system.sessions
                        shard key: { "_id" : 1 }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	544
                                myshardrs02	480
                        too many chunks to print, use verbose if you want to force print
mongos>
```

3.集合分片

对集合分片，你必须使用sh.shardCollection()方法指定集合的分片键。

语法：

```shell
sh.shardCollection(namespace,key,unique)
```

参数：

| Parameter | Type     | Description                                                  |
| --------- | -------- | ------------------------------------------------------------ |
| namespace | string   | 要(分片)共享的目标集合的命名空间，格式：<database>.<collection> |
| key       | document | 用作分片键的索引规范文档，shard键决定MongoDB如何在shard之间分发文档，除非集合为空，否则索引必须在shard collection命令之前存在，如果集合为空，则MongoDB在对集合进行分片之前创建索引，前提是支持分片键的索引不存在，简单的说：由包含字段和该字段的索引遍历方向的文档组成 |
| unique    | boolean  | 当值为true情况下，片键字段上会限制为确保是唯一索引，哈希策略片键不支持唯一索引，默认是false |



对集合进行分片时，你需要选择一个片键(Shard key),shard key是每条记录都必须包含的，且建立了索引的单个字段或复合字段，MongoDB按照片键将数据划分到不同的数据块中，并将数据块 均衡的分不到所有分片中，为了按照片键划分数据库MongoDB使用基于哈希的分片方式(随机平均分配)或者基于范围的分片方式(数值大小分配)

用什么字段当片键都可以，如：nickname作为片键，但一定是必填字段



==注意==：MongoDB只能通过一个字段给某个集合分片不能说一个集合即按哈希有按范围分片

分片规则一：哈希策略

对于基于哈希的分片，MongoDB计算一个字段的哈希值，并用这个哈希值来创建数据块

在使用基于哈希分片的系统中，拥有"相近"片键的文档，很可能不会存储在同一个数据块中，因此数据的分离性更好一些。

使用nickname作为片键，根据其值的哈希值进行数据分片

```shell
mongos> sh.shardCollection("articledb.comment",{"nickname":"hashed"})
{
	"collectionsharded" : "articledb.comment",
	"ok" : 1,
	"$clusterTime" : {
		"clusterTime" : Timestamp(1686196321, 36),
		"signature" : {
			"hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
			"keyId" : NumberLong(0)
		}
	},
	"operationTime" : Timestamp(1686196321, 32)
}
mongos> 
```

查看状态：

shard key: { "nickname" : "hashed" } 分片策略哈希

```shell
mongos> sh.status()
--- Sharding Status --- 
  sharding version: {
  	"_id" : 1,
  	"minCompatibleVersion" : 5,
  	"currentVersion" : 6,
  	"clusterId" : ObjectId("64813c5c4c6469e9d2fe9269")
  }
  shards:
        {  "_id" : "myshardrs01",  "host" : "myshardrs01/192.168.244.142:27018,192.168.244.142:27118",  "state" : 1,  "topologyTime" : Timestamp(1686193851, 2) }
        {  "_id" : "myshardrs02",  "host" : "myshardrs02/192.168.244.142:27318,192.168.244.142:27418",  "state" : 1,  "topologyTime" : Timestamp(1686194172, 1) }
  active mongoses:
        "5.0.18" : 1
  autosplit:
        Currently enabled: yes
  balancer:
        Currently enabled: yes
        Currently running: no
        Failed balancer rounds in last 5 attempts: 0
        Migration results for the last 24 hours: 
                512 : Success
  databases:
        {  "_id" : "articledb",  "primary" : "myshardrs01",  "partitioned" : true,  "version" : {  "uuid" : UUID("b94e53c7-d498-49b8-8965-1aafcab2df18"),  "timestamp" : Timestamp(1686193851, 3),  "lastMod" : 1 } }
                articledb.comment
                        shard key: { "nickname" : "hashed" }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	2
                                myshardrs02	2
                        { "nickname" : { "$minKey" : 1 } } -->> { "nickname" : NumberLong("-4611686018427387902") } on : myshardrs01 Timestamp(1, 0) 
                        { "nickname" : NumberLong("-4611686018427387902") } -->> { "nickname" : NumberLong(0) } on : myshardrs01 Timestamp(1, 1) 
                        { "nickname" : NumberLong(0) } -->> { "nickname" : NumberLong("4611686018427387902") } on : myshardrs02 Timestamp(1, 2) 
                        { "nickname" : NumberLong("4611686018427387902") } -->> { "nickname" : { "$maxKey" : 1 } } on : myshardrs02 Timestamp(1, 3) 
        {  "_id" : "config",  "primary" : "config",  "partitioned" : true }
                config.system.sessions
                        shard key: { "_id" : 1 }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	512
                                myshardrs02	512
                        too many chunks to print, use verbose if you want to force print
mongos>
```

提示：

报错：please create an index that starts with the shard key before sharding

解决方式：判断分支是查找是否有可用的索引存在，当无可用的索引，并且表不为空时，就会出现这个错误信息。





分片规则二：值范围策略

对于基于范围的分片，MongoDB按照片键的范围把数据分成不同部分，假设有一个数字的片键：想象一个从负无穷到正无穷的直线，每一个片键的值都在直线上画了一个点。MongoDB把这条直线划分为更短的不重叠的片段，并称之为数据块，每个数据块包含了片键在一定范围内的数据。

在使用片键做范围划分的系统中，拥有"相近"片键的文档很可能存储在同一个数据块中，因此也会存储在同一个分片中。

如果使用作者年龄字段作为片键，按照点赞数的值进行分片：

```shell
mongos> sh.shardCollection("articledb.author",{"age":1})
{
	"collectionsharded" : "articledb.author",
	"ok" : 1,
	"$clusterTime" : {
		"clusterTime" : Timestamp(1686196802, 6),
		"signature" : {
			"hash" : BinData(0,"AAAAAAAAAAAAAAAAAAAAAAAAAAA="),
			"keyId" : NumberLong(0)
		}
	},
	"operationTime" : Timestamp(1686196802, 2)
}
mongos> 
```

查看状态：

shard key: { "age" : 1 }

```shell
mongos> sh.status()
--- Sharding Status --- 
  sharding version: {
  	"_id" : 1,
  	"minCompatibleVersion" : 5,
  	"currentVersion" : 6,
  	"clusterId" : ObjectId("64813c5c4c6469e9d2fe9269")
  }
  shards:
        {  "_id" : "myshardrs01",  "host" : "myshardrs01/192.168.244.142:27018,192.168.244.142:27118",  "state" : 1,  "topologyTime" : Timestamp(1686193851, 2) }
        {  "_id" : "myshardrs02",  "host" : "myshardrs02/192.168.244.142:27318,192.168.244.142:27418",  "state" : 1,  "topologyTime" : Timestamp(1686194172, 1) }
  active mongoses:
        "5.0.18" : 1
  autosplit:
        Currently enabled: yes
  balancer:
        Currently enabled: yes
        Currently running: no
        Failed balancer rounds in last 5 attempts: 0
        Migration results for the last 24 hours: 
                512 : Success
  databases:
        {  "_id" : "articledb",  "primary" : "myshardrs01",  "partitioned" : true,  "version" : {  "uuid" : UUID("b94e53c7-d498-49b8-8965-1aafcab2df18"),  "timestamp" : Timestamp(1686193851, 3),  "lastMod" : 1 } }
                articledb.author
                        shard key: { "age" : 1 }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	1
                        { "age" : { "$minKey" : 1 } } -->> { "age" : { "$maxKey" : 1 } } on : myshardrs01 Timestamp(1, 0) 
                articledb.comment
                        shard key: { "nickname" : "hashed" }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	2
                                myshardrs02	2
                        { "nickname" : { "$minKey" : 1 } } -->> { "nickname" : NumberLong("-4611686018427387902") } on : myshardrs01 Timestamp(1, 0) 
                        { "nickname" : NumberLong("-4611686018427387902") } -->> { "nickname" : NumberLong(0) } on : myshardrs01 Timestamp(1, 1) 
                        { "nickname" : NumberLong(0) } -->> { "nickname" : NumberLong("4611686018427387902") } on : myshardrs02 Timestamp(1, 2) 
                        { "nickname" : NumberLong("4611686018427387902") } -->> { "nickname" : { "$maxKey" : 1 } } on : myshardrs02 Timestamp(1, 3) 
        {  "_id" : "config",  "primary" : "config",  "partitioned" : true }
                config.system.sessions
                        shard key: { "_id" : 1 }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	512
                                myshardrs02	512
                        too many chunks to print, use verbose if you want to force print
mongos>
```

注意的是：

1.  一个集合只能指定一个片键，否则报错
2.  一旦对一个集合分片，分片键和分片值就不可改变，如：不能给集合选择不同的分片键，不能更新分片键的值
3.  根据age索引进行分配数据

基于范围的分片方式与基于哈希的分片方式性能对比：

基于范围的分片方式提供了更高效的范围查询，给定一个片键的范围，分发路由可以很简单的确定哪个数据块存储了请求需要的数据，并将请求转发到相应的分片中

不过，基于范围的分片会导致数据在不同分片上的不均衡，有时候，带来的消极作用会大于查询性能的积极作用，比如，如果片键所在的字段是线性增长的，一定时间内的所有请求都会落到某个固定的数据块中，最终导致分布在同一个分片中，在这种情况下，一小部分分片承载了集群大部分的数据，系统并不能很好的进行扩展

与此相比，基于哈希的分片方式已范围查询性能的损失为代价，保证了集群中数据的均衡，哈希值的随机性使数据随机分布在每个数据块中，因此也随机分布在不同分片中，但是也正由于随机性，一个范围查询很难确定应该请求哪些分片，通常为了返回需要的结果，需要请求所有分片

如无特殊情况，一般推荐使用Hash Sharding。

而使用_id作为片键是一个不错的选择，因为它是必有得，你可以使用数据文档`_id`的哈希作为片键。

这个方案能够使读和写都能够平均分布，并且它能够保证每个文档都有不同的片键所以数据块能够很精细

似乎还是不够完美，因为这样的话对多个文档的查询必将命中所有的分片，虽说如此，这也是一种比较好的方案了。



理想化的shard key可以让document均匀的在集群分布：

![image-20230608121323081](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031700375.png)

显示集群的详细信息：

```shell
mongos> db.printShardingStatus()
```

```shell
mongos> db.printShardingStatus()
--- Sharding Status --- 
  sharding version: {
  	"_id" : 1,
  	"minCompatibleVersion" : 5,
  	"currentVersion" : 6,
  	"clusterId" : ObjectId("64813c5c4c6469e9d2fe9269")
  }
  shards:
        {  "_id" : "myshardrs01",  "host" : "myshardrs01/192.168.244.142:27018,192.168.244.142:27118",  "state" : 1,  "topologyTime" : Timestamp(1686193851, 2) }
        {  "_id" : "myshardrs02",  "host" : "myshardrs02/192.168.244.142:27318,192.168.244.142:27418",  "state" : 1,  "topologyTime" : Timestamp(1686194172, 1) }
  active mongoses:
        "5.0.18" : 1
  autosplit:
        Currently enabled: yes
  balancer:
        Currently enabled: yes
        Currently running: no
        Failed balancer rounds in last 5 attempts: 0
        Migration results for the last 24 hours: 
                512 : Success
  databases:
        {  "_id" : "articledb",  "primary" : "myshardrs01",  "partitioned" : true,  "version" : {  "uuid" : UUID("b94e53c7-d498-49b8-8965-1aafcab2df18"),  "timestamp" : Timestamp(1686193851, 3),  "lastMod" : 1 } }
                articledb.author
                        shard key: { "age" : 1 }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	1
                        { "age" : { "$minKey" : 1 } } -->> { "age" : { "$maxKey" : 1 } } on : myshardrs01 Timestamp(1, 0) 
                articledb.comment
                        shard key: { "nickname" : "hashed" }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	2
                                myshardrs02	2
                        { "nickname" : { "$minKey" : 1 } } -->> { "nickname" : NumberLong("-4611686018427387902") } on : myshardrs01 Timestamp(1, 0) 
                        { "nickname" : NumberLong("-4611686018427387902") } -->> { "nickname" : NumberLong(0) } on : myshardrs01 Timestamp(1, 1) 
                        { "nickname" : NumberLong(0) } -->> { "nickname" : NumberLong("4611686018427387902") } on : myshardrs02 Timestamp(1, 2) 
                        { "nickname" : NumberLong("4611686018427387902") } -->> { "nickname" : { "$maxKey" : 1 } } on : myshardrs02 Timestamp(1, 3) 
        {  "_id" : "config",  "primary" : "config",  "partitioned" : true }
                config.system.sessions
                        shard key: { "_id" : 1 }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	512
                                myshardrs02	512
                        too many chunks to print, use verbose if you want to force print
mongos>
```

查看均衡器是否工作(需要重新均衡时系统才会自动启动，不用管它)：

```shell
mongos> sh.isBalancerRunning()
false
mongos> 
```

查看当前Balancer状态：

```shell
mongos> sh.getBalancerState()
true
mongos> 
```

### 分片后插入数据测试

测试一(哈希规则)：登录mongos后，向comment循环插入1000条数据做测试：

```shell
mongos> use articledb
switched to db articledb
mongos> db
articledb
mongos> for(let i=1;i<=1000;i++){db.comment.insert({"_id":i+"","nickname":"BoBo"+i})}
WriteResult({ "nInserted" : 1 })
mongos> db.comment.count()
1000
mongos> 
```

提示：js的语法，因为mongo的shell是一个javaScript的shell

注意：从路由上插入的数据，必须包含片键，否则无法插入

分别登陆两个片的主节点，统计文档数量

第一个分片副本集：

```shell
myshardrs01:PRIMARY> db
articledb
myshardrs01:PRIMARY> db.comment.count()
507
myshardrs01:PRIMARY> 
```

第二个分片副本集：

```shell
myshardrs02:PRIMARY> use articledb
switched to db articledb
myshardrs02:PRIMARY> db
articledb
myshardrs02:PRIMARY> db.comment.count()
493
myshardrs02:PRIMARY> 
```

可以看到，1000条数据近似均匀的分不到了2个shard上，是根据片键的哈希值分配的。

这种分配方式非常易于水平扩展：一旦数据存储需要更大空间，可以直接再增加分片即可，同时提升了性能使用db.comment.stats()查看单个集合的完整情况，mongos执行该命令可以查看该集合的数据分片的情况。

使用sh.status()查看本库内所有集合的分片信息。

---

测试二(值范围规则)：登录mongos后，向comment循环插入1000条数据做测试：

```shell
mongos> db
articledb
mongos> for(let i=1;i<=2000;i++){db.author.save({"name":"BoBoBo"+i,"age":NumberInt(i%120)})}
WriteResult({ "nInserted" : 1 })
mongos> db.author.count()
2000
mongos> 
```

插入成功后，仍然要分别查看两个分片副本集的数据情况。

分片效果：

```shell
articledb.author
                        shard key: { "age" : 1 }
                        unique: false
                        balancing: true
                        chunks:
                                myshardrs01	1
                        { "age" : { "$minKey" : 1 } } -->> { "age" : { "$maxKey" : 1 } } on : myshardrs01 Timestamp(1, 0)
```

提示：

如果查看状态发现没有分片，则可能是由于以下原因造成了：

1.  系统繁忙，正在分片中。

2.  数据块(chunk)没有填满，默认的数据块尺寸(chunksize)是64M，填满后才会考虑向其它片的数据块填充数据，因此，为了测试，可以将其改小，这里改为1M，操作如下：

    ```shell
    mongos> use config
    switched to db config
    mongos> db
    config
    mongos> db.settings.save({_id:"chunksize",value:1})
    WriteResult({ "nMatched" : 0, "nUpserted" : 1, "nModified" : 0, "_id" : "chunksize" })
    mongos> 
    ```

    测试完改回来：

    ```shell
    mongos> db.settings.save({_id:"chunksize",value:64})
    ```

    注意：要先改小，再设置分片，为了测试，可以先删除集合，重新建立集合的分片策略，再插入数据测试即可。

### 再增加一个路由节点

目录:

```shell
➜  ~ mkdir -p /mongodb/sharded_cluster/myshardrs_27117/log
```

新建或修改配置文件：

```shell
➜  ~ vim /mongodb/sharded_cluster/myshardrs_27117/mongos.conf
```

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/sharded_cluster/myshardrs_27117/log/mongod.log"
 #当mongos或mongod实例重新启动时，mongos或mongod会将新条目附加到现有日志文件的末尾。
 logAppend: true
processManagement:
  #启用在后台运行mongos或mongod进程的守护进程模式。
 fork: true
 #指定用于保存mongos或mongod进程的进行ID的文件位置，其中mongos或mongod将写入其PID
 pidFilePath: "/mongodb/sharded_cluster/myshardrs_27117/log/mongod.pid"
net:
  #服务实例绑定IP，默认是localhost
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27117
sharding:
 #指定配置节点副本集
 #        副本集名称 / 主机地址与端口号
 configDB: myconfigrs/192.168.244.142:27019,192.168.244.142:27119,192.168.244.142:27219
```

启动mongos2：

```shell
➜  ~ /usr/local/mongodb/bin/mongos -f /mongodb/sharded_cluster/myshardrs_27117/mongos.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 27750
child process started successfully, parent exiting
➜  ~ 
```

使用Mongo客户端登录27117，发现，第二个路由无需配置，因为分片配置都保存到了配置服务器中了

## Compass连接分片集群

直接连接路由



![image-20230608143006151](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031700753.png)

![image-20230608143031512](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031700972.png)

## SpringDataMongoDB连接分片集群

Java客户端常用的是SpringDataMongoDB，其连接的是mongos路由，配置和单机mongod的配置是一样的。

多个路由的时候的SpringDataMongoDB的客户端配置参考如下：

直接连接路由节点即可

```yaml
server:
  port: 80
spring:
  #数据源配置
  data:
    mongodb:
      #主机地址
      #host: 192.168.244.142
      #数据库
      #database: articledb
      #端口号
      #port: 27017
      #也可以使用uri连接
      #uri: mongodb://192.168.244.142:27017/articledb
      #副本集的连接字符串
      #uri:
        #mongodb://192.168.244.142:27017,192.168.244.142:27018,192.168.244.142:27019/articledb?
        #connect=replicaSet&secondaryOk=true&replicaSet=myrs
      #连接路由字符串
      uri: mongodb://192.168.244.142:27017,192.168.244.142:27117/articledb
```

通过日志发现，写入数据的时候，会选择一个路由写入：

## 安全认证

### MongoDB的用户和角色权限简介

默认情况下，MongoDB实例启动运行时是没有启用用户访问权限控制的，也就是说，在实例本机服务器上都可以随意连接到实例进行各种操作，MongoDB不会对连接客户端进行用户验证，这是非常危险的。

mongoDB官网上说，为了能保障mongodb的安全可以做以下几个步骤：

1.  使用新的端口，默认的27017端口如果一旦知道了ip就能连接上，不太安全
2.  设置mongodb的网络环境，最好将mongodb部署到公司服务器内网，这样外网是访问不到的，公司内部访问使用vpn等
3.  开启安全认证，认证要同时设置服务器之间的内部认证方式，同时要设置客户端连接到集群的账号密码认证方式

为了强制开启用户访问控制(用户验证)，则需要在MongoDB实例启动时使用选项--auth或在指定启动配置文件中添加选项auth=true

在开始之前需要了解一下概念

1.  启用访问控制：

MongoDB使用的是基于角色的访问控制(Role-Based Access Control,RBAC)来管理用户対实例的访问，通过对用户授予一个或多个角色来控制用户访问数据库资源的权限和数据库操作的权限，在对用户分配角色之前，用户无法访问实例。

在实例启动时添加选项--auth或指定启动配置文件中添加选项auth=true

2.  角色：

在MongoDB中通过角色对用户授予相应数据库资源的操作权限，每个角色当中的权限可以显式指定，也可以通过继承其它角色的权限，或者两都存在的权限

3.  权限：

权限由指定的数据库资源(resource)以及允许在指定资源上进行的操作(action)组成。

1.  资源(resource)包括：数据库，集合，部分集合和集群；
2.  操作(action)包括：对资源进行的增，删，改，查(CRUD)操作。

在角色定义时可以包含一个或多个已存在的角色，新创建的角色会继承包含的角色所有的权限，在同一个数据库中，新创建角色可以继承其它角色的权限，在admin数据库中创建的角色可以继承在其它任意数据库中角色的权限。

关于角色权限的查看，可以通过如下命令查询：

```shell
#查询所有角色权限(仅用户自定义角色)
> db.runCommand({rolesInfo:1})
#查询所有用户角色权限(包含内置角色)
> db.runCommand({rolesInfo:1,showBuiltinRoles:true})
#查询当前数据库中的某角色的权限
> db.runCommand({rolesInfo:"<rolename>"})
#查询其它数据库中指定的角色权限
> db.runCommand({rolesInfo:{role:"<rolename>",db:"<database>"}})
#查询多个角色权限
> db.runCommand({rolesInfo:["rolename",{role:"<rolename>",db:"database"},...]})
```

常用的内置角色：

-  数据库用户角色：read，readWrite；
-  所有数据库用户角色：readAnyDatabase，readWriteAnyDatabase，userAdminAnyDatabase，dbAdminAnyDatabase
-  数据库管理角色：dbAdmin，dbOwner，userAdmin；
-  集群管理角色：clusterAdmin，clusterManager，clusterMonitor，hostManager；
-  备份恢复角色：backup，restore；
-  超级用户角色：root
-  内部角色：system

角色说明：

| 角色                 | 权限描述                                                     |
| -------------------- | ------------------------------------------------------------ |
| read                 | 可以读取指定数据库中任何数据                                 |
| readWrite            | 可以读写指定数据库中任何数据，包括创建，重命名，删除集合     |
| readAnyDatabase      | 可以读取所有数据库中任何数据(除了数据库config和loacl之外)    |
| readWriteAnyDatabase | 可以读写所有数据库中任何数据(除了数据库config和local之外)    |
| userAdminAnyDatabase | 可以在指定数据库创建和修改用户(除了数据库config和local之外)  |
| dbAdminAnyDatabase   | 可以读取任何数据库以及对数据库进行清理，修改，压缩，获取统计信息，执行检查等操作(除了数据库config和local之外) |
| dbAdmin              | 可以读取指定数据库以及对数据库进行清理，修怪，压缩，获取统计信息，执行检查等操作。 |
| userAdmin            | 可以在指定数据库创建和修改用户                               |
| clusterAdmin         | 可以对整个集群或数据库系统进行管理操作                       |
| backup               | 备份MongoDB数据最小的权限                                    |
| restore              | 从备份文件中还原恢复MongoDB数据(除了system.profile集合)的权限。 |
| root                 | 超级账号，超级权限                                           |

## 单实例环境

目标：对单实例的MongoDB服务开启安全认证，这里的单实例指定是未开启副本集或分片的MongoDB实例。

### 关闭已开启的服务(可选)

增加mongod的单实例的安全认证功能，可以在服务搭建的时候直接添加，也可以在之前搭建好的服务上添加。

本文使用之前搭建好的服务，因此，先停止之前的服务

```shell
➜  ~ ps -ef|grep mongod|grep -v grep
dkx0       2291      1  2 Jun07 ?        00:09:22 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27018/mongod.conf
dkx0       2368      1  2 Jun07 ?        00:09:19 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27118/mongod.conf
dkx0       2443      1  1 Jun07 ?        00:05:27 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27218/mongod.conf
dkx0       5200      1  2 Jun07 ?        00:07:06 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27318/mongod.conf
dkx0       5268      1  1 Jun07 ?        00:06:37 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27418/mongod.conf
dkx0       5337      1  1 Jun07 ?        00:04:25 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27518/mongod.conf
dkx0       7659      1  3 Jun07 ?        00:09:26 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27019/mongod.conf
dkx0       7736      1  2 Jun07 ?        00:08:15 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27119/mongod.conf
dkx0       7808      1  2 Jun07 ?        00:08:11 /usr/local/mongodb/bin/mongod -f /mongodb/sharded_cluster/myshardrs01_27219/mongod.conf
dkx0      10092      1  0 Jun07 ?        00:02:23 /usr/local/mongodb/bin/mongos -f /mongodb/sharded_cluster/myshardrs_27017/mongos.conf
dkx0      27750      1  0 02:22 ?        00:00:31 /usr/local/mongodb/bin/mongos -f /mongodb/sharded_cluster/myshardrs_27117/mongos.conf
➜  ~ 
```

停止服务的方式有两种：快速关闭和标准关闭，下面依次说明：

1.  快速关闭方式(快速，简单，数据可能会出错)

目标：通过系统的kill命令直接杀死进程：

杀完要检查一下，避免有的没有杀掉。

```shell
#通过进程编号关闭节点
➜  ~ kill -2 2291 2368 2443 5200 5268 5337 7659 7736 7808 10092 27750
➜  ~ ps -ef|grep mongod|grep -v grep
➜  ~ 
```

[补充]

如果一旦是因为数据损坏，则需要进行如下操作：

1.  删除lock文件：

    ```shell
    rm -f /mongodb/single/data/db/*.lock
    ```

2.  修复数据：

    ```shell
    /usr/local/mongodb/bin/mongod --repair --dbpath=/mongodb/single/data/db
    ```

    2.标准的关闭方法(数据不容易出错，但是麻烦)

    目标：通过mongo客户端中的shutdownServer命令来关闭服务

    ```shell
    > db.shutdownServer()
    ```

### 添加用户和权限

​    1.先按照普通无授权认证的配置，来配置服务端的配置文件/mongodb/single/mongod.conf：(参考，复用之前的)

```shell
systemLog:
 #MongoDB发送所有日志输出的目标指定为文件
 #The path of the log file to which mongod or mongos should send all diagnostic logging information
 destination: file
 #mongod或mongos应向其发送所有诊断日志记录信息的日志文件的路径
 path: "/mongodb/single/log/mongod.log"
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
 bindIp: localhost,192.168.244.142
  #bindIp
  #绑定的端口，默认是27017
 port: 27017
```

2.按之前为开启认证的方式(不添加--auth参数)来启动MongoDB服务：

```shell
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/single/log/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 34342
child process started successfully, parent exiting
➜  ~ 
```

提示：

在操作用户时，启动mongod服务时尽量不要开启授权

3.使用Mongo客户端登录：

```shell
➜  ~ /usr/local/mongodb/bin/mongo --host=192.168.244.142 --port=27017
```

4.创建两个管理员用户，一个是系统的超级管理员myroot，一个是admin库的管理用户myadmin：

```shell
#切换到admin库
> use admin
#创建系统超级用户myroot，设置密码123456，设置角色root
> db.createUser({user:"myroot",pwd:"123456",roles:[{"role":"root","db":"admin"}]})
#或 上面指定了集合下面默认是当前所在集合都是admin
> db.createUser({user:"myroot",pwd:"123456",roles:["root"]})
Successfully added user: { "user" : "myroot", "roles" : [ "root" ] }
#创建专门用来管理admin库的账号myadmin，只用来作为用户权限的管理
> db.createUser({user:"myadmin",pwd:"123456",roles:[{role:"userAdminAnyDatabase",db:"admin"}]})
Successfully added user: {
	"user" : "myadmin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	]
}
> 
#查看已经创建了的用户的情况：
> db.system.users.find()
{ "_id" : "admin.myroot", "userId" : UUID("9ecafd01-cef1-43a4-85ad-4ec21e905f81"), "user" : "myroot", "db" : "admin", "credentials" : { "SCRAM-SHA-1" : { "iterationCount" : 10000, "salt" : "LefHopC8WY3knLfOVz56iw==", "storedKey" : "fFDpj6y1AZCw60aOTM8fBXzUwBc=", "serverKey" : "YvTH+hyBHN1XThjYnrotc57/hgs=" }, "SCRAM-SHA-256" : { "iterationCount" : 15000, "salt" : "35I31t2xZ5NIK0wrw0LZajRGtnY8B8YLwjHfgg==", "storedKey" : "iy3/8WvkxrNDcKPJmFQ7rTAXxNvzDtjK7uMxzUkhgJ4=", "serverKey" : "V8Nhn6pBt50M5UsUNBNDGfaOU23f+CLYcYLnxBRTRy0=" } }, "roles" : [ { "role" : "root", "db" : "admin" } ] }
{ "_id" : "admin.myadmin", "userId" : UUID("c8145c3a-b1a1-4be2-a226-39e067cbe9b0"), "user" : "myadmin", "db" : "admin", "credentials" : { "SCRAM-SHA-1" : { "iterationCount" : 10000, "salt" : "Gc9gasfekYv2F9njhAd/Sg==", "storedKey" : "vy3mzY/oer2ONiUO8nV0DRrLwUg=", "serverKey" : "oVgCRHfLt/HW4PslM3QCghvVHZ4=" }, "SCRAM-SHA-256" : { "iterationCount" : 15000, "salt" : "vXE6XCPtMv2ErDmXM/21KlbiTlEQEQ89rmcIgQ==", "storedKey" : "MexKD1eAYxvws8ANYtvNPdDZajNTAjfJphlIDtvPn80=", "serverKey" : "Zzp84uYSusKlbNoMmTi/rPvskHl531RQDjQIjcdDGkY=" } }, "roles" : [ { "role" : "userAdminAnyDatabase", "db" : "admin" } ] }
> 
#删除用户
> db.dropUser("myadmin")
true
> db.system.users.find()
{ "_id" : "admin.myroot", "userId" : UUID("9ecafd01-cef1-43a4-85ad-4ec21e905f81"), "user" : "myroot", "db" : "admin", "credentials" : { "SCRAM-SHA-1" : { "iterationCount" : 10000, "salt" : "LefHopC8WY3knLfOVz56iw==", "storedKey" : "fFDpj6y1AZCw60aOTM8fBXzUwBc=", "serverKey" : "YvTH+hyBHN1XThjYnrotc57/hgs=" }, "SCRAM-SHA-256" : { "iterationCount" : 15000, "salt" : "35I31t2xZ5NIK0wrw0LZajRGtnY8B8YLwjHfgg==", "storedKey" : "iy3/8WvkxrNDcKPJmFQ7rTAXxNvzDtjK7uMxzUkhgJ4=", "serverKey" : "V8Nhn6pBt50M5UsUNBNDGfaOU23f+CLYcYLnxBRTRy0=" } }, "roles" : [ { "role" : "root", "db" : "admin" } ] }
> 
#修改密码
> db.changeUserPassword("myroot","123456")
> 
```

提示：

1.  本案例创建了两个用户，分别对应超管和专门用来管理用户的角色，事实上，你只需要一个用户即可。如果你对安全要求很高，防止超管泄露，则不要创建超管用户。
2.  和其它数据库(MySQL)一样，权限的管理都差不多一样，也是将用户和权限信息保存到数据库对应的表中。MongoDB存储所有的用户信息在admin数据库的集合system.users中，保存用户名，密码和数据库信息。
3.  如果不指定数据库，则创建的指定的权限的用户在所有的数据库上有效，如{role:"userAdminAnyDatabase",db:""}

认证测试：

测试添加的用户是否正确：

```shell
> use admin
switched to db admin
#输入错误密码
> db.auth("myroot","1234")
Error: Authentication failed.
0
#输入正确密码
> db.auth("myroot","123456")
1
> 
```

创建普通用户

创建普通用户可以在没有开启认证的时候添加，也可以在开启认证之后添加，但开启认证之后，必须使用有操作admin库的用户登录认证后才能操作，底层都是将用户信息保存在了admin数据库的集合system.users中。

```shell
> use articledb
switched to db articledb
> db
articledb
#创建用户，拥有articledb数据库的读写权限readWrite，密码是123456
> db.createUser({user:"bobo",pwd:"123456",roles:[{role:"readWrite","db":"articledb"}]})
Successfully added user: {
	"user" : "bobo",
	"roles" : [
		{
			"role" : "readWrite",
			"db" : "articledb"
		}
	]
}
#测试是否可用
> db.auth("bobo","123456")
1
> 
```

提示：

如果开启了认证后，登录的客户端的用户必须使用admin库的角色，如拥有root角色的myadmin用户，再通过myadmin用户去创建其它角色的用户

### 服务端开启认证和客户端连接登录

1.  关闭已经启动的服务

2.  使用linux命令杀死进程：

    ```shell
    ➜  ~ ps -ef|grep mongod|grep -v grep
    dkx0      34342      1  1 03:53 ?        00:00:29 /usr/local/mongodb/bin/mongod -f /mongodb/single/log/mongod.conf
    dkx0      34546  34416  0 03:55 pts/1    00:00:00 /usr/local/mongodb/bin/mongo --host=192.168.244.142 --port=27017
    ➜  ~ kill -2 34342
    ➜  ~ 
    ```

3.  在mongo客户端中使用shutdownServer命令来关闭

    ```shell
    > db.shutdownServer()
    shutdown command only works with the admin database; try 'use admin'
    > use admin
    switched to db admin
    > db.shutdownServer()
    server should be down...
    > 
    ```

    需要几个条件：

    -  必须是在admin库下执行该关闭命令
    -  如果没有认证，必须是从localhost登录的，才能执行关闭服务命令
    -  非localhost得，通过远程登录，必须有登录且必须登录用户有对admin操作权限才可以。

    2.以开启认证的方式启动服务

    有两种方式开启权限认证启动服务：一种是参数方式，一种是配置文件方式(推荐)。

    1.参数方式

    在启动时指定参数--auth，如：

    ```shell
    ➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/single/log/mongod.conf --auth
    ```

    2.配置文件方式

    在single/mongod.conf配置文件中加入:

    ``` shell
    security:
     #开启授权认证
     authorization: enabled
    ```

    启动时可不加--auth参数该方式避免忘记加--auth导致不安全问题发生：

    ```shell
    ➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/single/log/mongod.conf
    ```

    启动服务并连接客户端查看效果：

    ```shell
    ➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/single/log/mongod.conf 
    about to fork child process, waiting until server is ready for connections.
    forked process: 36759
    child process started successfully, parent exiting
    ➜  ~ 
    ➜  ~ /usr/local/mongodb/bin/mongo --host=192.168.244.142 --port=27017
    MongoDB shell version v5.0.18
    connecting to: mongodb://192.168.244.142:27017/?compressors=disabled&gssapiServiceName=mongodb
    Implicit session: session { "id" : UUID("ea29ccf7-9d79-4fc5-9615-b26267842fd8") }
    MongoDB server version: 5.0.18
    ================
    Warning: the "mongo" shell has been superseded by "mongosh",
    which delivers improved usability and compatibility.The "mongo" shell has been deprecated and will be removed in
    an upcoming release.
    For installation instructions, see
    https://docs.mongodb.com/mongodb-shell/install/
    ================
    > show dbs
    > use articledb
    switched to db articledb
    > show tables
    Warning: unable to run listCollections, attempting to approximate collection names by parsing connectionStatus
    > 
    ```

    可以看到不能访问任何数据

登录myroot用户：

```shell
> use admin
switched to db admin
> db.auth("myroot","123456")
1
> show dbs
admin      0.000GB
articledb  0.000GB
config     0.000GB
local      0.000GB
> 
```

登录bobo普通用户：

```shell
> show dbs
> db.auth("bobo","123456")
Error: Authentication failed.
0
> use articledb
switched to db articledb
> db.auth("bobo","123456")
1
> show dbs
articledb  0.000GB
> show tables
comment
> 
```

==注意==：哪个用户权限属于哪个集合就需要use到该集合中执行db.auth命令才行

### SpringDataMongoDB连接认证

使用用户名和密码连接到MongoDB服务器，你必须使用`username:password@hostname/dbname`格式，`username`为用户名，`password`为密码

目标：使用用户bobo使用密码123456连接到MongoDB服务上。

application.yml

```yaml
spring:
	#数据源配置
	data:
		mongodb:
			#主机地址
			#host: 192.168.244.142
			#数据库
			#database: articledb
			#默认端口号
			#port: 27017
			#账号
			#username: bobo
			#密码
			#password: 123456
			#单机有认证的情况下，也是用字符串连接
			uri: mongodb://bobo:123456@192.168.244.142:27017/articledb
```

提示：

分别测试用户名密码正确以及不正确的情况.

Compass连接需要指定用户名和密码否则连接不上

![image-20230608165514763](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031701928.png)

![image-20230608165613122](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031701801.png)

![image-20230608165629084](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031701100.png)

## 副本集环境

### 前言

对于搭建好的mongodb副本集，为了安全，启动安全认证，使用账号密码登。

副本集环境使用之前搭建好的，架构如下：

![image-20230608165821185](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031701536.png)

### 关闭已开启的副本集服务

### 通过主节点添加一个管理员账号

只需要在主节点上添加用户，副本集会自动同步

开启认证之前，创建超管用户：myroot，密码：123456

```shell
myrs:PRIMARY> use admin
myrs:PRIMARY> db.createUser({user:"myroot",pwd:"123456",roles:["root"]})
```

详细操作见单实例环境的`添加用户和权限`的相关操作。

提示：

该步骤也可以在开启认证之后，但需要通过localhost登录才允许添加用户，用户数据也会自动同步到副本集，后续再创建其它用户，都可以使用该超管用户创建。

### 创建副本集认证的key文件

第一步：生成一个key文件当当前文件夹中

可以使用任何方法生成密钥文件，例如：以下操作使用openssl生成密码文件，然后使用chmod来 更改文件权限，仅为文件所有者提供读取权限

```shell
➜  ~ openssl rand -base64 90 -out ./mongo.keyfile
> chmod 400 ./mongo.keyfile
```

提示：

所有副本集节点都必须要用同一份keyfile，一般是在一台机器上生成，然后拷贝到其它机器上，且必须有读的权限，否则将来会报错：

```shell
permissions on /mongodb/replica_sets/myrs_27017/mongo.keyfile are too
```

一定要保证密钥文件一致，文件位置随便，但是为了方便查找，建议每台机器都放到一个固定位置，都放到和配置文件一起的目录中

这里将该文件分别拷贝到多个目录中：

```shell
> cp mongo.keyfile /mongodb/replica_sets/myrs_27017
> cp mongo.keyfile /mongodb/replica_sets/myrs_27018
> cp mongo.keyfile /mongodb/replica_sets/myrs_27019
```

### 修改配置文件指定keyfile

分别编辑几个服务的mongod.conf文件，添加相关内容：

```yaml
security:
	#keyfile签权文件
	keyFile: /mongodb/replica_sets/myrs_27017/mongo.keyfile
	#开启认证方式运行
	authorization: enabled
```

```yaml
security:
	#keyfile签权文件
	keyFile: /mongodb/replica_sets/myrs_27018/mongo.keyfile
	#开启认证方式运行
	authorization: enabled
```

```yaml
security:
	#keyfile签权文件
	keyFile: /mongodb/replica_sets/myrs_27019/mongo.keyfile
	#开启认证方式运行
	authorization: enabled
```

### 重新启动副本集

如果副本集是开启状态，则先分别关闭副本集中的每个mongod，从次节点开始，直到副本集的所有成员都离线，包括任何仲载者，主节点必须是最后一个成员关闭以避免潜在的回滚。

```shell
➜  ~ kill -2 2453 2224 2110 2013 
```

分别启动副本集节点：

```shell
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27017/log/mongod.conf 
about to fork child process, waiting until server is ready for connections.
forked process: 3960
child process started successfully, parent exiting
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27018/log/mongod.conf
about to fork child process, waiting until server is ready for connections.
forked process: 4043
child process started successfully, parent exiting
➜  ~ /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27019/log/mongod.conf
about to fork child process, waiting until server is ready for connections.
forked process: 4144
child process started successfully, parent exiting
➜  ~ 
```

查看进程：

```shell
➜  ~ ps -ef|grep mongod|grep -v grep
dkx0       3960      1  9 22:44 ?        00:00:03 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27017/log/mongod.conf
dkx0       4043      1 10 22:44 ?        00:00:03 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27018/log/mongod.conf
dkx0       4144      1 11 22:44 ?        00:00:02 /usr/local/mongodb/bin/mongod -f /mongodb/replica_sets/myrs_27019/log/mongod.conf
➜  ~ 
```

登录myroot用户查看

```shell
myrs:PRIMARY> show dbs
myrs:PRIMARY> use admin
switched to db admin
myrs:PRIMARY> db.auth("myroot","123456")
1
myrs:PRIMARY> show dbs
admin      0.000GB
articledb  0.000GB
config     0.000GB
local      0.001GB
test       0.000GB
myrs:PRIMARY> 
```

### 在主节点上添加普通账号

```shell
#添加普通用户管理articledb集合
myrs:PRIMARY> db.createUser({user:"bobo",pwd:"123456",roles:[{role:"readWrite",db:"articledb"}]})
Successfully added user: {
	"user" : "bobo",
	"roles" : [
		{
			"role" : "readWrite",
			"db" : "articledb"
		}
	]
}
myrs:PRIMARY> 
```

重新连接，使用普通用户bobo重新登录，查看数据

注意：也要使用rs.status()命令查看副本集是否健康



### SpringDataMongoDB连接副本集

使用用户名和密码连接到MongoDB服务器，必须使用`username:password@hostname/dbname`格式

目标：使用用户bobo使用密码123456连接到mongoDB服务上

application.yml

```yaml
server:
  port: 80
spring:
  #数据源配置
  data:
    mongodb:
      #主机地址
      #host: 192.168.244.142
      #数据库
      #database: articledb
      #端口号
      #port: 27017
      #也可以使用uri连接
      #uri: mongodb://192.168.244.142:27017/articledb
      #副本集的连接字符串
      uri:
        mongodb://bobo:123456@192.168.244.142:27017,192.168.244.142:27018,192.168.244.142:27019/articledb?
        connect=replicaSet&secondaryOk=true&replicaSet=myrs
```

### 分片集群环境(扩展)

#### 关闭已开启的集群服务

分片集群环境下的安全认证和副本集环境下基本上一样。

但分片集群的服务器环境和架构较为复杂，建议在搭建分片集群的时候，直接加入安全认证和服务器间的签权，如果之前有数据，可先将之前的数据备份出来，再还原回去

本文使用之前搭建好的集群服务，因此，先停止之前的集群服务

依次杀死mongod路由，配置副本集服务，分片副本服务，从次节点开始，直到所有成员都离线，副本集杀的时候，建议先杀仲载者，再杀副本节点，最后是主节点，以避免潜在的回滚，杀完检查一下

