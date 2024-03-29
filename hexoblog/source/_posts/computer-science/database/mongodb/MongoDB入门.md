---
title: MongoDB入门
categories:
  - [计算机学科,database,mongodb]
tags:
  - 计算机学科
  - database
  - mongodb
---

## MongoDB相关概念

### 业务应用场景

传统的关系型数据库(如MySQL)，在数据库操作的"三高"需求以及应对Web2.0的网站需求，显得力不从心

**解释**："三高" **需求**：

-  High performance - 对数据库高并发读写的需求。
-  Huge Storage - 对海量数据的高效率存储和访问的需求。
-  High Scalabillity && High Availability - 对数据库的高可扩展性和高可用性的需求。

而MongoDB可应对"三高"需求。

**具体的应用场景如**：

1.  社交场景，使用MongoDB存储用户信息，以及用户发表的朋友圈信息，通过地理位置索引实现附近的人，地点等功能。
2.  游戏场景，使用MongoDB存储游戏用户信息，用户的装备，积分等直接以内嵌文档的形式存储，方便查询，高效率存储和访问。
3.  物流场景，使用MongoDB存储订单信息，订单状态在运送过程中会不断更新，以MongoDB内嵌数据的形式来存储，一次查询就能将订单所有的变更读取出来。
4.  物联网场景，使用MongoDB存储所有接入的智能设备信息，以及设备汇报的日志信息，并对这些信息进行多维度的分析。
5.  视频直播，使用MongoDB存储用户信息，点赞互动信息等。

这些应用场景中，数据操作方面的共同特点是：

1.  数据量大
2.  写入操作频繁(读写很频繁)
3.  价值较低的数据，对事物要求性不高

对于这样的数据，我们更适合使用MongoDB来实现数据的存储

### 什么时候选择MongoDB

在构架选型上，除了上述的三个特点外，如果你还犹豫是否选择它，可以考虑以下的一些问题：

应用不需要事务以及复杂join支持

新应用，需求会变，数据模型无法确定，想快速迭代开发

应用需要2000~3000以上的读写QPS(更高也可以)

应用需要TB甚至PB级别数据存储

应用要求存储的数据不丢失

应用需要99.999%高可用

应用需要大量的地理位置查询，文本查询

如果上述有1个符合，可以考虑MongoDB，2个以及的符合，选择MongoDB绝不会后悔

思考：如果用MySQL呢？

答：相对MySQL，可以以更低的成本解决问题(包括学习，开发，运维等成本)

### MongoDB简介

MongoDB是一个开源，高性能，无模式的文档型数据库，当初的设计就是用于简化开发和方便扩展，是NoSQL数据库产品的一种，是最像关系型数据库(MySQL)的非关系型数据库。

它支持的数据结构非常松散，是一种类似于JSON的格式叫BSON，所以它既可以存储比较复杂的数据类型，有相当灵活。

MongoDB中的记录是一个文档，它是一个由字段和值对(field:value)组成的数据结构，MongoDB文档类似于JSON对象，即一个文档认为就是一个对象，字段的数据类型是字符串，它的值除了使用基本的一些类型外，还可以包括其它文档，普通数组和文档数组。

## 体系结构

MySQL和MongoDB对比

![image-20230602204736615](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031701137.png)

| SQL术语/概念 | MongoDB术语/概念 | 解释/说明                            |
| ------------ | ---------------- | ------------------------------------ |
| database     | databse          | 数据库                               |
| table        | collection       | 数据库表/集合                        |
| row          | document         | 数据记录行/文档                      |
| column       | field            | 数据字段(列)/域(字段)                |
| index        | index            | 索引                                 |
| table joins  |                  | 表连接，MongoDB不支持                |
|              | 嵌入文档         | MongoDB通过嵌入式文档来替代多表连接  |
| primary key  | primary key      | 主键，MongoDB自动将_id字段设置为主键 |

## 数据模型

MongoDB的最小存储单位就是文档(document)对象，文档(document)对象对应于关系型数据库的行，数据在MongoDB中以BSON(Binary-JSON)文档的格式存储在磁盘上。

BSON(Binary Serialized Document Format)是一种类似json的一种二进制形式的存储格式，简称Binary JSON。

BSON和JSON一样，支持内嵌的文档对象和数组对象，但是BSON有JSON没有的一些数据类型，如Date和BinData类型。

BSON采用了类似于C语言结构体的名称，对表示方法，支持内嵌的文档对象和数组对象，具有轻量性，可遍历性，高效性的三个特点，可以有效描述非结构化数据的结构化数据。这种格式的优点是灵活性高，但它的缺点是空间利用率不是很理想。

Bson中，除了基本的JSON类型：string，integer，boolean，double，null，array和object，mongo还是用了特殊的数据类型，这些类型包括date，object，binary，data，regular，expression和code。每一个驱动都以特定语言的方式实现了这些类型，查看你的驱动的文档来获取详细信息。

BSON数据类型参考列表：

| 数据类型      | 描述                                                         | 举例                                                 |
| ------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| 字符串        | UTF-8字符串都可以表示为字符串类型的数据                      | {"x":foobar"}                                        |
| 对象id        | 对象id是文档的12字节的唯一ID                                 | {"x":ObjectId()}                                     |
| 布尔值        | 真或者假：true或者false                                      | {"x":true}+                                          |
| 数组          | 值的集合或者列表可以表示成数组                               | {"x":["a","b","c"]}                                  |
| 32位整数      | 类型不可用，JavaScript仅支持64位浮点数，所以32位整数会被自动转换 | shell是不支持该类型的，shell中默认会转换成64位浮点数 |
| 64位浮点数    | shell中的数字就是一种类型                                    | {"x":3.14159,"y":3}                                  |
| null          | 表示空值或者未定义的对象                                     | {"x":null}                                           |
| undefined     | 文档中也可以使用未定义类型                                   | {"x":undefined}                                      |
| 符号          | shell不支持，shell会将数据库中的符号类型的数据自动转换成字符串 |                                                      |
| 正则表达式    | 文档中可以包含正则表达式，采用JavaScript的正则表达式         | {"x":/foobar/i}                                      |
| 代码          | 文档中还可以包含JavaScript代码                               | {"x":function(){/* .... */}}                         |
| 二进制数据    | 二进制数据可以由任意字节的串组成，不过shell中无法使用        |                                                      |
| 最大值/最小值 | BSON包括一个特殊类型，表示可能的最大值shell中没有这个类型    |                                                      |

提示：

shell默认使用64位浮点值。{"x":3.14}或{"x":3}。对于整型值，可以使用NumberInt(4个字节符号整数)或NumberLong(8字节符号整数)，{"x":NumberInt("3"){"x":NumberLong("3")}}

## MongoDB的特点

MongoDB主要有如下特点：

1.高性能

MongoDB提供高性能的数据持久性，特别是对嵌入式数据模型的支持减少了数据库系统上的I/O活动。

索引支持更快的查询，并且可以包含来自嵌入式文档和数组的键。(文本索引解决搜索的要求，TTL索引解决历史数据自动过期的需求，地理位置索引可用于构建各种O2O应用)

mmapv1，wiredtiger，mongorocks(rocksdb)，in-memory等多引擎支持满足各种场景需求。

Gridfs解决文件存储的需求。

2.高可用性

MongoDB的复制工具成为副本集(replica set)，它可提供自动故障转移和数据冗余。

3.高扩展性

MongoDB提供了水平可扩展性作为其核心功能的一部分。

分片将数据分布在一组集群的机器上。(海量数据存储，服务能力水平扩展)

从3.4开始，MongoDB支持基于片创建数据区域，在一个平衡的集群中，MongoDB将一个区域所覆盖的读写只定向到该区域内的那些片。

4.丰富的查询支持

MongoDB支持丰富的查询语言，支持读和写操作(CRUD)，比如数据聚合，文本搜索和地理空间查询等。

5.其它特点：如无模式(动态模式)，灵活的文档模型

## 基本常用命令

### 案例需求：

存放文章评论的数据存放到MongoDB中，数据结构参考如下：

数据库：articledb

| 专栏文章评论   | comment        |                  |                           |
| -------------- | -------------- | ---------------- | ------------------------- |
| 字段名称       | 字段含义       | 字段类型         | 备注                      |
| _id            | ID             | ObjectId或String | Mongo的主键的字段         |
| articleid      | 文章ID         | String           |                           |
| content        | 评论内容       | String           |                           |
| userid         | 评论人ID       | String           |                           |
| nickname       | 评论人昵称     | String           |                           |
| createdatetime | 评论的日期时间 | Date             |                           |
| likenum        | 点赞数         | int32            |                           |
| replynum       | 回复数         | int32            |                           |
| state          | 状态           | String           | 0：不可见，1：可见        |
| parentid       | 上级ID         | String           | 如果为0表示文章的顶级评论 |

## 数据库操作

### 选择和创建数据库

选择和创建数据库的语法格式：

```shell
> use 数据库名称
```

如果数据库不存在则自动创建，例如，以下语句创建spitdb数据库：

```shell
> use articledb
```

查看有权限查看的所有数据库命令

```shell
> show dbs
或
> show databases
```

>  注意：在MongoDB中，集合只有在内容插入后才会创建！就是说，创建集合(数据表)后要再插入一个文档(记录)，集合才会真正创建。

查看当前正在使用的数据库命令

```shell
> db
```

MongoDB中默认的数据库为test，如果你没有选择数据库，集合将存放在test数据库中。

另外：

数据库名可以是满足以下条件的任意UTF-8字符串。

-  不能是空字符串("")
-  不得含有' '(空格)，.，$，/，\，和\0(空字符)
-  应全部小写
-  最多64字节

有一些数据库名是保留的，可以直接访问这些有特殊作用的数据库

-  admin：从权限的角度来看，这是"roor"数据库，要是将一个用户添加到这个数据库，这个用户自动继承所有数据库的权限，一些特定的服务器端命令也只能从这个数据库运行，比如列出所有的数据库或者关闭服务器
-  local：这个数据库的数据永远不会被复制，可以用来存储限于本地单台服务器的任意集合
-  config：当Mongo用于分片设置时，config数据库在内部使用，用于保存分片的相关信息

创建数据库：

```shell
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
> use articledb
switched to db articledb
> show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
>
```

可以看到检查数据库时并没有articledb这是为什么呢，看如下解释：

>  MongoDB的存储分为两部分：当使用use articledb创建数据库时存放到了内存中了，没有持久化到磁盘所以使用show dbs是看不到的，当articledb中有一个集合的时候，不是一个空的数据库的时候就会自动持久化到磁盘中

![image-20230603161134922](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031701895.png)

查看当前所在的数据库：

```shell
> db
articledb
>
```

### 数据库的删除

MongoDB删除数据库的语法格式如下：在要删除的数据库中执行

```shell
> db.dropDatabase()
```

提示：主要用来删除已经持久化的数据库

### 集合操作

集合，类似关系型数据库中的表

可以显示的创建，也可以隐式的创建

### 集合的显示创建

基本语法格式：

```shell
> db.createCollection(name)
```

参数说明：

-  name：要创建的集合名称

例如：创建一个名为mycollection的普通集合

```shell
> db.createCollection("mycollection")
```

查看当前数据库中的表：show tables命令

```shell
> show collections
或
> show tables
```

集合的命名规范：

-  集合名不能是空字符串
-  集合名不能含有\0字符(空字符)，这个字符表示集合名的结尾
-  集合名不能以"system."开头，这是为系统集合保留的前缀
-  用户创建的集合名字不能含有保留字符，有些驱动程序的确支持在集合名里面包含，这是因为某些系统生成的集合中包含该字符，除非你要访问这种系统创建的集合，否则千万不要在名字里出现$

### 集合的隐式创建

当向一个集合插入一个文档的时候，如果集合不存在，则会自动创建集合。

详见文档的插入 章节。

提示：通常我们使用隐式创建文档即可。

### 集合的删除

集合的删除语法格式如下：

```shell
> db.collection.drop()
或
> db.集合.drop()
```

**返回值** 

如果成功删除选定集合，则drop()方法返回true，否则返回false

例如：要删除mycollection集合

```shell
> db.mycollection.drop()
```

```shell
> show tables
name
> db.name.drop()
true
> show tables
>
```

## 文档基本CRUD

文档(document)的数据结构和JSON基本一样。

所有存储在集合中的数据都是BSON格式

### 文档的插入

#### 1.单个文档插入

使用insert()或save()方法向集合中插入文档，语法格式如下：

```shell
> db.collection.insert(
	<document or array of documents>,
	{
		writeConcern: <document>,
		ordered: <boolean>
	}
)
```

参数：

| Parameter    | Type              | Description                                                  |
| ------------ | ----------------- | ------------------------------------------------------------ |
| document     | document or array | 要插入到集合中的文档或文档数组(json格式)                     |
| writeConcern | document          | Optional.A document expressing the write concern.Omit to use the<br>default write concern.See write Concern.Do not explicitly set the write<br>concerrn for  the operation if run in a transaction.To use write concern<br>with transactions,see Transactions and Write Concern |
| orrderd      | boolean           | 可选，如果为真，则按顺序插入数组中的文档 ，如果其中一个文档出现错误，MongoDB将返回而不处理数组中其余文档，如果为假，则执行无序插入，如果其中一个文档出现错误，则继续处理数组中的主文档，在版本2.6+中默认为true |

[示例]

要向comment的集合(表)中插入一条测试数据：

```shell
> db.comment.insert(
	{"articleid":"10000","content":"今天天气真好","userid":"1001","nickname":"Rose","createdatetime":"now Date()","likenum":NumberInt(10),"state":null}
)
```

提示：

1.  comment集合如果不存在，则会隐式创建
2.  mongo中的数字，默认情况下是double类型，如果要存储整型，必须使用函数NumberInt(整型数字)，否则取出来就有问题了
3.  插入当前日期使用now Date()
4.  插入的数据没有指定_id，会自动生成主键值
5.  如果某字段没有值，可以赋值为null，或不写该字段

执行后，如下，说明插入一个数据成功了。

```shell
WriteResult({ "nInserted" : 1 })
```

现在数据库中没有任何的集合：

```shell
> show tables
>
```

向一个不存在的集合插入数据：

```shell
> db.comment.insert()
uncaught exception: Error: no object passed to insert! :
DBCollection.prototype.insert@src/mongo/shell/collection.js:275:15
@(db.comment.insert({"articleid":"1000","content":"今天天气真好","useid":"1001","inckname":"Rose","createdatetime":"now Date()","likenum":NumberInt(10),"state":null})001","incknam"})
WriteResult({ "nInserted" : 1 })
> show tables
comment
>
```

查看集合中插入的数据：

```shell
> db.comment.find()
{ "_id" : ObjectId("647b250656447c36332f1c37"), "articleid" : "1000", "content" : "今天天气真好", "useid" : "1001", "inckname" : "Rose", "createdatetime" : "now Date()", "likenum" : 10, "state" : null }
>
```

不存在的集合就被隐式的创建了。

==注意==：

1.  文档中 的键/值对是有序的
2.  文档中的值不仅可以是在双引号里面的字符串，还可以是其它几种数据类型(甚至可以是整个嵌入的文档)
3.  MongoDB区分类型和大小写
4.  MongoDB的文档不能有重复的键
5.  文档的键是字符串，除了少数例外情况，键可以使用任意UTF-8字符

文档键命名规范：

-  键不能含有\0(空字符)，这个字符用来表示键的结尾。
-  .和$有特别的意义，只有在特定环境下才能使用
-  以下划线"_"开头的键是保留的(不是严格要求的)

#### 2.批量插入

语法：

```shell
> db.collection.insertMany([<document 1>,<document 2>],{
	writeConcern: <document>,
	ordered: <boolean>
})
```

参数：

| Parameter    | Type     | Description                                                  |
| ------------ | -------- | ------------------------------------------------------------ |
| document     | document | 要插入到集合中的文档或文档数组(json格式)                     |
| writeConcern | document | Optional.A document expressing the write concern.Omit to use the default write concern.Do not explicitly set the write concern for the operation if run in a transaction. To use write concern with<br>transactions,see Transactions and write concern |
| ordered      | boolean  | 可选，一个布尔值，指定Mongod示例执行有序插入还是无序插入，默认为true |

[示例]

批量插入多条文章评论：

```shell
> db.comment.insertMany([
	{"_id":"1","articleid":"10001","contet":"我们不应该把清晨浪费在手机上，健康很重要，一杯温水幸福你我他","userid":"1002","nickname":"相忘于江湖","createdatatime":new Date("2019-08-05T22:08:15.522Z","likenum":NumberInt(1000),"state":"1"},
	{"_id":"2","articleid":"10001","contet":"我夏天空腹喝凉开水，冬天喝温开水","userid":"1005","nickname":"伊人憔悴","createdatatime":new Date("2019-08-05T22:58:51.485Z","likenum":NumberInt(888),"state":"1"},
	{"_id":"3","articleid":"10001","contet":"我一直喝凉开水，冬天夏天都喝","userid":"1004","nickname":"杰克船长","createdatatime":new Date("2019-08-05T22:05:51.485Z","likenum":NumberInt(888),"state":"1"}
])
```

提示：

插入时指定了_id，则主键就是该值

如果某条数据插入失败，将会终止插入，但已经插入成功的数据不会回滚掉

因为批量插入由于数据较多容易出现失败，因为，可以使用try catch进行异常捕获处理，测试的时候可以不处理，如(了解)：

```shell
> try{
	db.comment.insertMany([
	{"_id":"1","articleid":"10001","contet":"我们不应该把清晨浪费在手机上，健康很重要，一杯温水幸福你我他","userid":"1002","nickname":"相忘于江湖","createdatatime":new Date("2019-08-05T22:08:15.522Z","likenum":NumberInt(1000),"state":"1"},
	{"_id":"2","articleid":"10001","contet":"我夏天空腹喝凉开水，冬天喝温开水","userid":"1005","nickname":"伊人憔悴","createdatatime":new Date("2019-08-05T22:58:51.485Z","likenum":NumberInt(888),"state":"1"},
	{"_id":"3","articleid":"10001","contet":"我一直喝凉开水，冬天夏天都喝","userid":"1004","nickname":"杰克船长","createdatatime":new Date("2019-08-05T22:05:51.485Z","likenum":NumberInt(888),"state":"1"}
	])
}catch(e){
	print(e);
}
```

```shell
> try{db.comment.insertMany([{"_id":"1","articleid":"10001"},{"_id":"2","articleid":"10001"},{"_id":"3","articleid":"10001"}])}catch(e){print(e)}
{ "acknowledged" : true, "insertedIds" : [ "1", "2", "3" ] }
>
```

-  测试-嫌麻烦添加了比较少的数据

## 文档的基本查询

查询数据的语法格式如下：

```shell
> db.collection.find(<query>,[projection])
```

参数：

| Parameter  | Type     | Description                                                  |
| ---------- | -------- | ------------------------------------------------------------ |
| query      | document | 可选，使用查询运算符指定选择筛选器，若要返回集合中的所有文档，请省略此参数或传递空文档({}) |
| projection | document | 可选，指定要在与查询筛选器匹配的文档中返回的字段(投影)，诺要返回匹配文档中的所有字段，请省略此参数 |

[示例]

### 1.查询所有

如果我们要查询spit集合的所有文档，我们输入以下命令：

```shell
db.comment.find()
或
db.comment.find({})
```

这里你会发现每条文档会有一个叫_id的字段，这个相当于我们原来关系型数据库中表的主键，当你在插入文档记录时没有 指定该字段，MongoDB会自动创建，其类型是ObjectID类型

如果我们在插入文档记录时指定该字段也可以，其类型可以是ObjectID类型，也可以是MongoDB支持的任意类型

如果我想按一定条件来查询，比如我想查询userid为1003的记录，怎么办？很简单！只要在find()中添加参数即可，参数也是json格式，如下：

```shell
> db.comment.find({"articleid":"10001"})
{ "_id" : "1", "articleid" : "10001" }
{ "_id" : "2", "articleid" : "10001" }
{ "_id" : "3", "articleid" : "10001" }
> db.comment.find({"articleid":"10001"})2{ "_id" : "1", "articleid" : "10001" }3{ "_id" : "2", "articleid" : "10001" }4{ "_id" : "3", "articleid" : "10001" }5>db.comment.find({userid:'1003'})
```

如果你只需要返回符合条件的第一条数据，我们可以使用findOne命令来实现，语法和find一样。

如：查询用户编号是1003的记录，但只最多返回符合条件的第一条记录：

```shell
> db.comment.findOne({"articleid":"10001"})
{ "_id" : "1", "articleid" : "10001" }
>
```

2.投影查询(Projection Query)

如果要查询结果返回部分字段，则需要使用投影查询(不显示所有字段，只显示指定的字段)

如：查询结果只显示`_id，aticleid，nickname` 

```
> db.comment.find({aticleid:"10001"},{aticleid:1,nickname:1})
{ "_id" : "1", "aticleid" : "10001", "nickname" : "Rose" }
>
```

默认`_id`会显示

如：查询结果只显示，aticleid，nickname，不显示`_id` :

```shell
> db.comment.find({aticleid:"10001"},{aticleid:1,nickname:1,_id:0})
{ "aticleid" : "10001", "nickname" : "Rose" }
>
```

再例如：查询所有数据，但只显示_id，aticleid，nickname；

```shell
> db.comment.find({},{aticleid:1,nickname:1})
{ "_id" : "1", "aticleid" : "10001", "nickname" : "Rose" }
>
```

## 文档的更新

更新文档的语法：

```shell
db.collection.update(query,update,options)
或
db.collection.update(
	<query>,
	<update>,
	{
		upsert: <boolean>,
		multi: <boolean>,
		writeConcern: <document>,
		collation: <document>,
		arrayFilters: [<filterdocument1>,...]
		hint: <document|string> //Available starting in MongoDB 4.2
	}
)
```

参数：

| Parameter    | Type                 | Description                                                  |
| ------------ | -------------------- | ------------------------------------------------------------ |
| query        | document             | 更新的选择条件，可以使用与find()方法中相同的查询选择器，类似sql update查询内where后面的，在3.0版中进行了更改：当使用upsert:true执行update()时，如果查询使用点表示法存在_id字段上指定条件，则MongoDB将拒绝插入新文档 |
| update       | document or pipeline | 要应用的修改，该值可以是：包含更新运算符表达式的文档，或仅包含<field1>: <value>对的替换文档，或在MongoDB4.2中启动聚合管道，管道可以由以下阶段组成：$addFIelds及其别名`$`set`$`project及其别名`$`unset`$`replaceroot及其别名`$`replaceWith，换句话说：它是update的对象和一些更新的操作符(如`$`,`$`inc...)等，也可以理解为sql update查询内set后面的值 |
| upsert       | boolean              | 可选，如果设置为true，则在没有与查询条件匹配的文档时创建新文档，默认值为false，如果找不到匹配项，则不会插入新文档 |
| multi        | boolean              | 可选，如果设置为true，则更新符合查询条件的多个文档，如果设置为false，则更新一个文档，默认值为fasle |
| writeConcern | document             | 可选，表示写问题的文档，抛出异常的级别                       |
| collation    | document             | 可选<br>指定要用于操作的校对规则<br>校对规则允许用户为字符串比较指定特定与语言的规则，例如字母大小写和重音标记的规则<br>校对规则选项具有以下语句：<br>校对规则：{<br>区域设置:<string>,<br>caseLevel:<boolean>,<br>caseFirst:<string>,<br>强度:<int>,<br>numericordering:<boolean>,<br>替代:<string>,<br>最大变量:<string>,<br>向后:<boolean><br>}<br>指定校对规则时，区域设置字段是必须得，所有其它校对规则字段都是可选的，有关字段的说明，请参阅校对规则文档<br>如果未指定校对规则，但集合具有默认校对规则(请参见db.createCollection())，则该操作将使用为集合指定的校对规则。<br>如果没有为集合或操作指定校对规则，MongoDB将使用以前的版本中使用的简单二进制比较进行字符串比较，不能为一个操作指定多个校对规则，例如，不能为每个字段指定不同的校对规则，或者如果使用排序执行查找，则不能将一个校对规则用于查找，另一个校对规则用于排序<br>3.4版新增 |
| arrayFilters | array                | 可选，一个筛选文档数组，用于确定要为数组字段上的更新操作修改那些数组元素，在更新文档中，使用[`$`[](){}]筛选的位置运算符来定义标识符，然后咋子数组过滤器文档中应用，如果标识符未包含在更新文档中，则不能有标识符的数组筛选器文档，注意，<identifier>必须以小写字母开头，并且只包含字母数字字符，您可以在更新文档中多次包含同一标识符：但是，对于r在更新文档中，每个不同的标识符(`$`[标识符])都必须指定一个对应的数组筛选器文档，也就是说，不能为同一个标识符指定多个数组筛选器文档，3.6版+ |
| hint         | Document or string   | 可选，指定用于支持查询谓词的索引的文档或字符串，该选项可以采用索引规范文档或索引名称字符串，如果指定的索引不存在，则说明操作错误，例如：版本4中的 未更新操作指定提示 |

提示：

主要关注前四个参数即可

[示例]

### 1.覆盖的修改

如果我们想修改_id为1的记录，点赞量为1001，输入以下语句

```shell
> db.comment.update({_id:"1"},{likenum:NumberInt(1001)})
```

```shell
> db.comment.update({_id:1},{likenum:NumberInt(1001)})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.comment.find()
#注意这里的_id为1的我不小心设置为了数据型的，而_id为2设置为字符串型更改时也要对应数据类型才能找得到
{ "_id" : 1, "likenum" : 1001 }
{ "_id" : "2", "articleid" : "10001", "likenum" : 889, "nickname" : "凯撒大帝", "userid" : "1003" }
>
```

执行后，我们会发现，这条文档除了likenum字段其它字段都不见了

### 2.局部修改

为了解决这个问题，我们需要使用修改器`$`set来实现，命令如下：

我们想修改_id为2的记录，浏览量为889，输入以下语句：

```shell
> db.comment.update({_id:2},{$set:{likenum:NumberInt(889)}})
```

```shell
> db.comment.update({_id:"2"},{$set:{likenum:NumberInt(889)}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.comment.find()
{ "_id" : 1, "articleid" : "10001", "likenum" : 2, "nickname" : "刘桑", "userid" : "1003" }
{ "_id" : "2", "articleid" : "10001", "likenum" : 889, "nickname" : "小花", "userid" : "1003" }
>
```

这样就OK了

### 3.批量的修改

更新所有用户为1003的用户的昵称为 凯撒大帝

```shell
#默认只修改第一条数据
> db.comment.update({userid:"1003"},{$set:{nickname:"凯撒2"}})
#修改所有符合条件的数据
> db.comment.update({userid:"1003"},{$set:{nickname:"凯撒大帝"}},{multi:true})
```

```shell
> db.comment.update({userid:"1003"},{$set:{nickname:"凯撒大帝"}},{multi:true})
WriteResult({ "nMatched" : 2, "nUpserted" : 0, "nModified" : 2 })
> db.comment.find()
{ "_id" : 1, "articleid" : "10001", "likenum" : 2, "nickname" : "凯撒大帝", "userid" : "1003" }
{ "_id" : "2", "articleid" : "10001", "likenum" : 889, "nickname" : "凯撒大帝", "userid" : "1003" }
>
```

提示：

如果不加后面的参数，则只更新符合条件的第一条记录

### 3.列值增长的修改

如果我们想实现对某列值在原有值的基础上进行增加或减少，可以使用`$`inc运算符来实现。

需求：对3号数据的点赞数，每次递增1

```
> db.comment.update({_id:1},{$inc:{likenum:NumberInt(1)}})
```

```shell
> db.comment.update({_id:1},{$inc:{likenum:NumberInt(1)}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.comment.find()
{ "_id" : 1, "likenum" : 1002 }
{ "_id" : "2", "articleid" : "10001", "likenum" : 889, "nickname" : "凯撒大帝", "userid" : "1003" }
>
```

```shell
> db.comment.update({userid:"1003"},{$inc:{"likenum":NumberInt(1)}},{multi:true})
WriteResult({ "nMatched" : 2, "nUpserted" : 0, "nModified" : 2 })
> db.comment.find()
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003" }
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003" }
>
```

## 删除文档

删除文档的语法结构：

```shell
> db.集合名称.remove(条件)
```

以下语句可以将数据全部删除，请慎用

```shell
> db.comment.remove({})
```

如果删除_id=1的记录，输入以下语句，删除条件有多个会删除多个

```shell
> db.comment.remove({_id:"1"})
```

```shell
> db.comment.find()
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003" }
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003" }
> db.comment.remove({_id:"1"})
WriteResult({ "nRemoved" : 1 })
> db.comment.find()
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003" }
>
```

```shell
> db.comment.find()
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003" }
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003" }
> db.comment.remove({userid:"1003"})
WriteResult({ "nRemoved" : 2 })
> db.comment.find()
>
```

## 文档的分页查询

### 统计查询

统计查询使用count()方法，语法如下：

```shell
> db.collection.count(query,options)
```

参数：

| Parameter | Type     | Description                    |
| --------- | -------- | ------------------------------ |
| query     | document | 查询选择条件                   |
| options   | document | 可选，用于修改统计数的额外选项 |

[示例]

1.统计所有记录数：

统计comment集合的所有的记录数：

```shell
> db.comment.count()
2
>
```

2.按条件统计记录：

例如：统计userid为1003的记录条数

```shell
> db.comment.count({userid:"1003"})
2
>
> db.comment.count({_id:"1"})
1
> db.comment.count({_id:"2"})
1
>
```

提示：

默认情况下count()方法返回符合条件的全部记录条数

### 分页列表查询

可以使用limit()方法来读取指定数量的数据，使用skip()方法来跳过指定数量的数据

基本语法格式如下：

```shell
> db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)
```

```shell
> db.comment.find().limit(2).skip(1)
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003" }
> db.comment.find().limit(2).skip(2)
> db.comment.find().limit(2).skip(0)
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003" }
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003" }
> db.comment.find().limit(1).skip(0)
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003" }
```

如果你想返回指定条数的记录，可以在find方法后调用limit来返回结果(TopN)，默认值20，例如：

```shell
> db.comment.find().limit(2)
```

```shell
> db.comment.find().limit(2)
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003" }
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003" }
>
```

skip方法同样接受一个数字参数作为跳过的记录条数，(前N个不要)，默认值是0

```shell
> db.comment.find().skip(2)
```

```shell
> db.comment.find().skip(2)
> db.comment.find().skip(1)
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003" }
>
```

分页查询：需求：每页2个，第二页开始：跳过前两条数据，接着值显示3和4条数据

```shell
#第一页
> db.comment.find().skip(0).limit(2)
#第二页
> db.comment.find().skip(2).limit(2)
#第三页
> db.comment.find().skip(3).limit(2)
```

## 排序查询

sort()方法对数据进行排序，sort()方法可以通过参数指定排序的字段，并使用1和-1来指定排序方式，其中1为升序排列，而-1是用于降序排列

语法如下所示：

```shell
> db.comment_name.find().sort({KEY:1})
或
> db.集合名称.find().sort(排序方式)
```

例如：

对userid降序排列，并对访问量进行升序排列

```shell
> db.comment.find().sort({userid:1,likenum:1})
```

```shell
> db.comment.find().sort({userid:-1,likenum:1})
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003" }
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003" }
>
```

提示：

skip()，limit()，sort()三个放在一起执行的时候，执行的顺序是先sort()然后是skip()最后是显示的limit()和命令编写顺序无关

sort > skip > limit

## 文档的更多查询

### 正则的复杂条件查询

MongoDB的模糊查询是通过正则表达式的方式实现的，格式为：

```shell
> db.collection.find({field:/正则表达式/})
或
> db.集合名称.find({字段:/正则表达式/})
```

提示：正则表达式是js的语法，直接量的写法

例如：我要查询评论内容包含"开水"的所有文档，代码如下：

```shell
> db.comment.find({content:/开水/})
```

```shell
> db.comment.find()
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "开水" }
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "专家表明" }
{ "_id" : "3", "articleid" : "10003", "likenum" : 100, "nickname" : "123", "userid" : "1003", "content" : "开水" }
> db.comment.find({content:/开水/})
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "开水" }
{ "_id" : "3", "articleid" : "10003", "likenum" : 100, "nickname" : "123", "userid" : "1003", "content" : "开水" }
>
```

如果要查询评论的内容中以"专家"开头的，代码如下：
```shell
> db.comment.find({content:/^专家/})
```

```shell
> db.comment.find({content:/^专家/})
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "专家表明" }
>
```

## 比较查询

<，<=，>，>=这个操作符也是很常用的，格式如下：

```shell
#大于：field > vlaue
> db.集合名称.find({"field":{$gt:value}})
#小于：field < value
> db.集合名称.find({"field":{$lt:value}})
#大于等于：field >= value
> db.集合名称.find({"field":{$gte:value}})
#小于等于：field <= value
> db.集合名称.find({"field":{$lte:value}})
#不等于：field != value
> db.集合名称.find({"field":{$ne:value}})
```

示例：查询评论带你赞数量大于700的记录

```shell
> db.comment.find({likenum:{$gt:NumberInt(700)}})
```

```shell
> db.comment.find({likenum:{$gt:NumberInt(700)}})
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "开水" }
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "专家表明" }
>
```

## 包含查询

包含使用$in操作符

示例：查询评论的集合中userid字段包含1003和1004的文档

```shll
> db.comment.find({userid:{$in:["1003","1004"]}})
```

```shell
> db.comment.find({userid:{$in:["1003","1004"]}})
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "开水" }
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "专家表明" }
{ "_id" : "3", "articleid" : "10003", "likenum" : 100, "nickname" : "凯撒", "userid" : "1003", "content" : "开水" }
>
```

## 条件连接查询

我们如果需要查询同时满足两个以上条件，需要使用$and操作符将条件进行关联(相当于SQL的and)

格式为：

```shell
$and:[{},{},{}]
```

示例：查询评论集合中likenum大于等于700并且小于2000的文档

```shell
> db.comment.find({$and:[{lienum:{$gte:NumberInt(700)}},{likenum:{$lt:NumberInt(2000)}}]})
```

```shell
> db.comment.find({$and:[{likenum:{$gte:NumberInt(700)}},{likenum:{$lt:NumberInt(2000)}}]})
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "开水" }
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "专家表明" }
>
```

如果两个以上条件之间是或者的关系，我们使用操作符进行关联，与前面and的使用方式相同

格式为：

```shell
$or:[{},{},{}]
```

示例：查询评论集合中userid为1003，或者点赞数小于1000的文档记录

```shell
> db.comment.find({$or:[{userid:"1003"},{likenum:{$lt:1000}}]})
```

```shell
> db.comment.find({$or:[{userid:"1003"},{likenum:{$lt:1000}}]})
{ "_id" : "1", "articleid" : "10001", "likenum" : 1003, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "开水" }
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "专家表明" }
{ "_id" : "3", "articleid" : "10003", "likenum" : 100, "nickname" : "凯撒", "userid" : "1003", "content" : "开水" }
> db.comment.find({$and:[{userid:"1003"},{likenum:{$lt:1000}}]})
{ "_id" : "2", "articleid" : "10002", "likenum" : 890, "nickname" : "凯撒大帝", "userid" : "1003", "content" : "专家表明" }
{ "_id" : "3", "articleid" : "10003", "likenum" : 100, "nickname" : "凯撒", "userid" : "1003", "content" : "开水" }
>
```

## 常用命令小结

选择切换数据库：use articledb

插入数据：db.comment.insert({bson数据})

查询所有数据：db.comment.find()

条件查询数据：db.cmoment.find({条件})

查询符合条件的第一条记录：db.comment.findOne({条件})

查询符合条件的前几条记录：db.comment.find({条件}).limit(条数)

查询符合条件的跳过的记录：db.comment.find({条件}).skip(条数)

修改数据：db.comment.update({条件},{修改后的数据})或db.comment.update({条件},{$set:{要修改部分的字段:数据}})

修改数据并自增某字段值：db.comment.update({条件},{$inc:{自增的字段:步进值}})

删除数据：db.comment.remove({条件})

统计查询：db.comment.count({条件})

模糊查询：db.comment.find({字段名:/正则表达式/})

条件比较运算：db.comment.find({字段名:{$gt:值}})

包含查询：db.comment.find({字段名:{`$`in:[值1,值2]}})或db.comment.find({字段名:{`$`nin:[值1,值2]}})

条件连接查询：db.comment.find({`$`and:[{条件1},{条件2}]})或db.comment.find({`$`or:[{条件1},{条件2}])

## 索引的类型

### 单字段索引

MongoDB支持在文档的单个字段上创建用户定义的升序/降序索引，称为单字段索引(Single Field Index)

对于单个字段索引的排序操作，索引键的排序顺序(即升序或降序)并不重要，因为MongoDB可以在任何方向上遍历索引

![image-20230604161713948](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031702130.png)

### 其它索引

地理空间索引(Geospatial Index)，文本索引(Text Indexes)，哈希索引(Hashed Indexes)

#### 地理空间索引(Geospatial Index)

为了支持对地理空间坐标数据的有效查询，MongoDB提供了两种特殊的索引：返回结果是使用平面几何的二维索引和返回结果时使用球面几何的二维球面索引

#### 文本索引(Text Indexes)

MongoDB提供了一种文本索引类型，支持在集合中搜索字符串内容，这些文本索引不存储特定于语言的停止词(例如："the"，"a"，"or")，而将集合中的此作为词干，只存储跟词。

#### 哈希索引(Hashed Indexes)

为了支持基于散列的分片，MongoDB提供了散列索引类型，它对字段值的散列进行索引，这些索引在其范围内的值分布更加随机，但只支持相等匹配，不支持基于范围的查询

### 索引的管理操作

#### 索引的查看

说明：

返回一个集合中的所有索引的数组。

语法：

```shell
> db.collection.getIndexes()
```

提示：该语法命令运行要求是MongoDB 3.0+

[示例]

查看comment集合中所有的索引情况

```shell
> db.comment.getIndexes()
```

```shell
> db.comment.getIndexes()
[ { "v" : 2, "key" : { "_id" : 1 }, "name" : "_id_" } ]
>
```

结果中显示的是默认_id索引

默认_id索引：

MongoDB在创建集合的过程中，在_id字段上创建一个唯一的索引，默认名字为`_id_`，该索引可防止客户端插入两个具有相同值的文档，您不能在`_id`字段上删除此索引

注意：该索引是唯一索引，因此值不能重复，即_id值不能重复的，在分片集群中，通常使用`_id`作为片键

### 索引的创建

说明：

在集合上创建索引

语法：

```shell
> db.collection.createIndex(keys.options)
```

参数：

| Parameter | Type     | Description                                                  |
| --------- | -------- | ------------------------------------------------------------ |
| keys      | document | 包含字段和值对的文档，其中字段是索引键，值描述该字段的索引类型，对于字段上的升序索引，请指定值1；对于降序索引，请指定值-1；比如：{字段:1或-1}，其中1为指定按升序创建索引，如果你想按降序创建索引指定为-1即可。另外MongoDB支持几种不同的索引类型，包括文本，地理空间和哈希索引 |
| options   | document | 可选，包含一组控制索引创建的选项的文档，有关详细信息，请参见选项详情列表 |

options(更多选项)列表：

| Parameter          | Type             | Description                                                  |
| ------------------ | ---------------- | ------------------------------------------------------------ |
| background         | Boolean          | 键索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加"background"可选参数，"background"默认值为false |
| unique             | Boolean          | 建立的索引是否唯一，指定为true创建唯一索引，默认值为false    |
| name               | string           | 索引的名称，如果未指定，MongoDB的通过连接索引的字段名和排序生成一个索引名称 |
| dropDups           | Boolean          | 3.0+版本已经废弃，在建立唯一索引时是否删除重复记录，指定true创建唯一索引，默认值为false |
| sparse             | Boolean          | 对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档，默认值为false |
| expireAfterSeconds | integer          | 指定一个以秒为单位的数值，完成TTL设定，设定集合的生存时间    |
| v                  | index<br>version | 索引的版本号，默认的索引版本取决于mongod创建索引时运行的版本 |
| weights            | document         | 索引权重值，数值在1到99.999之间，表示该索引相对于其它索引字段得分权重 |
| default_language   | string           | 对于文本索引，该参数决定了停用词及词干和词器的规则的列表，默认为英语 |

提示：

注意在3.0.0版本前创建索引方法为：`db.collection.ensureIndex()`，之后的版本使用了`db.collection.createIndex()`方法，`ensureIndex()`还能用，但只是`createIndex()`的别名

[示例]

1.单字段索引示例：对`userid`字段建立索引

```shell
> db.comment.createIndex({userid:1})
```

```shell
> db.comment.createIndex({userid:1})
{
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 2,
        "createdCollectionAutomatically" : false,
        "ok" : 1
}
>
```

参数1：按升序创建索引

可以查看一下：

```shell
> db.comment.getIndexes()
[
        {
                "v" : 2,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_"
        },
        {
                "v" : 2,
                "key" : {
                        "userid" : 1
                },
                "name" : "userid_1"
        }
]
>
```

索引名字为：`userid_1`

compass查看：

![image-20230604171621927](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031702245.png)

复合索引：对`userid`和`nickname`同时建立复合(Compound)索引：

```shell
> db.comment.createIndex({userid:1,nickname:-1})
```

```shell
> db.comment.createIndex({userid:1,nickname:-1})
{
        "numIndexesBefore" : 2,
        "numIndexesAfter" : 3,
        "createdCollectionAutomatically" : false,
        "ok" : 1
}
>
```

查看一下索引：

```shell
> db.comment.getIndexes()
[
        {
                "v" : 2,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_"
        },
        {
                "v" : 2,
                "key" : {
                        "userid" : 1
                },
                "name" : "userid_1"
        },
        {
                "v" : 2,
                "key" : {
                        "userid" : 1,
                        "nickname" : -1
                },
                "name" : "userid_1_nickname_-1"
        }
]
>
```

### 索引的移除

说明：可以移除指定的索引，或移除所有索引

#### 一，指定索引的移除

语法：

```shell
> db.collection.dropIndex(index)
```

参数：

| Parameter | Type               | Description                                                  |
| --------- | ------------------ | ------------------------------------------------------------ |
| index     | string or document | 指定要删除的索引，可以通过索引名称或索引规范文档指定索引，诺要删除文本索引，请指定索引名称 |

[示例]

删除`comment`集合中`userid`字段上的升序索引：

```shell
> db.comment.dropIndex({userid:1})
```

查看已经删除了

```shell
> db.comment.getIndexes()
[
        {
                "v" : 2,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_"
        },
        {
                "v" : 2,
                "key" : {
                        "userid" : 1
                },
                "name" : "userid_1"
        },
        {
                "v" : 2,
                "key" : {
                        "userid" : 1,
                        "nickname" : -1
                },
                "name" : "userid_1_nickname_-1"
        }
]
> db.comment.dropIndex({userid:1})
{ "nIndexesWas" : 3, "ok" : 1 }
> db.comment.getIndexes()
[
        {
                "v" : 2,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_"
        },
        {
                "v" : 2,
                "key" : {
                        "userid" : 1,
                        "nickname" : -1
                },
                "name" : "userid_1_nickname_-1"
        }
]
>
```

#### 二，所有索引的移除

语法：

```shell
> db.collection.dropIndexes()
```

[示例]

删除skip集合中所有索引

```shell
> db.comment.dropIndexes()
{
        "nIndexesWas" : 2,
        "msg" : "non-_id indexes dropped for collection",
        "ok" : 1
}
>
```

提示：`_id`的字段的索引是无法删除的，只能删除非`_id`字段的索引

查看索引：

```shell
> db.comment.getIndexes()
[ { "v" : 2, "key" : { "_id" : 1 }, "name" : "_id_" } ]
>
```

### 索引的使用

#### 执行计划

分析查询性能(Analyze Query Performance)通常使用执行计划，(解释计划，Explin Plan)来查看查询的情况，如查询耗费的时间，是否基于索引查询等

那么，通常，我们想知道，建立的索引是否有效，效果如何，都需要通过执行计划查看

语法：

```shell
> db.collection.find(query,options).explain(options)
```

[示例]

查看根据userid查询数据的情况：

```shell
> db.comment.find({userid:"1003"}).explain()
{
        "explainVersion" : "1",
        "queryPlanner" : {
                "namespace" : "articledb.comment",
                "indexFilterSet" : false,
                "parsedQuery" : {
                        "userid" : {
                                "$eq" : "1003"
                        }
                },
                "queryHash" : "37A12FC3",
                "planCacheKey" : "4A7D843D",
                "maxIndexedOrSolutionsReached" : false,
                "maxIndexedAndSolutionsReached" : false,
                "maxScansToExplodeReached" : false,
                "winningPlan" : {
                        "stage" : "COLLSCAN",
                        "filter" : {
                                "userid" : {
                                        "$eq" : "1003"
                                }
                        },
                        "direction" : "forward"
                },
                "rejectedPlans" : [ ]
        },
        "command" : {
                "find" : "comment",
                "filter" : {
                        "userid" : "1003"
                },
                "$db" : "articledb"
        },
        "serverInfo" : {
                "host" : "localhost.localdomain",
                "port" : 27017,
                "version" : "5.0.18",
                "gitVersion" : "796abe56bfdbca6968ff570311bf72d93632825b"
        },
        "serverParameters" : {
                "internalQueryFacetBufferSizeBytes" : 104857600,
                "internalQueryFacetMaxOutputDocSizeBytes" : 104857600,
                "internalLookupStageIntermediateDocumentMaxSizeBytes" : 104857600,
                "internalDocumentSourceGroupMaxMemoryBytes" : 104857600,
                "internalQueryMaxBlockingSortMemoryUsageBytes" : 104857600,
                "internalQueryProhibitBlockingMergeOnMongoS" : 0,
                "internalQueryMaxAddToSetBytes" : 104857600,
                "internalDocumentSourceSetWindowFieldsMaxMemoryBytes" : 104857600
        },
        "ok" : 1
}
>
```

查看出来的结果是文档的扫描是5条记录，数据是5条，扫描也是5条就代表全文档扫描

![image-20230604214503129](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031702609.png)

想用上索引就需要给数据 加上索引

```shell
> db.comment.createIndex({userid:1})
{
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 2,
        "createdCollectionAutomatically" : false,
        "ok" : 1
}
```

```shell
> db.comment.find({userid:"1003"}).explain()
{
        "explainVersion" : "1",
        "queryPlanner" : {
                "namespace" : "articledb.comment",
                "indexFilterSet" : false,
                "parsedQuery" : {
                        "userid" : {
                                "$eq" : "1003"
                        }
                },
                "queryHash" : "37A12FC3",
                "planCacheKey" : "3B74CBDE",
                "maxIndexedOrSolutionsReached" : false,
                "maxIndexedAndSolutionsReached" : false,
                "maxScansToExplodeReached" : false,
                "winningPlan" : {
                        "stage" : "FETCH",
                        "inputStage" : {
                                "stage" : "IXSCAN",
                                "keyPattern" : {
                                        "userid" : 1
                                },
                                "indexName" : "userid_1",
                                "isMultiKey" : false,
                                "multiKeyPaths" : {
                                        "userid" : [ ]
                                },
                                "isUnique" : false,
                                "isSparse" : false,
                                "isPartial" : false,
                                "indexVersion" : 2,
                                "direction" : "forward",
                                "indexBounds" : {
                                        "userid" : [
                                                "[\"1003\", \"1003\"]"
                                        ]
                                }
                        }
                },
                "rejectedPlans" : [ ]
        },
        "command" : {
                "find" : "comment",
                "filter" : {
                        "userid" : "1003"
                },
                "$db" : "articledb"
        },
        "serverInfo" : {
                "host" : "localhost.localdomain",
                "port" : 27017,
                "version" : "5.0.18",
                "gitVersion" : "796abe56bfdbca6968ff570311bf72d93632825b"
        },
        "serverParameters" : {
                "internalQueryFacetBufferSizeBytes" : 104857600,
                "internalQueryFacetMaxOutputDocSizeBytes" : 104857600,
                "internalLookupStageIntermediateDocumentMaxSizeBytes" : 104857600,
                "internalDocumentSourceGroupMaxMemoryBytes" : 104857600,
                "internalQueryMaxBlockingSortMemoryUsageBytes" : 104857600,
                "internalQueryProhibitBlockingMergeOnMongoS" : 0,
                "internalQueryMaxAddToSetBytes" : 104857600,
                "internalDocumentSourceSetWindowFieldsMaxMemoryBytes" : 104857600
        },
        "ok" : 1
}
>
```

此时的"stage" : "FETCH"就变成了FETCH

此时文档的扫描就是2条数据了

![image-20230604215307694](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031703658.png)

它是先通过IXSCAN查询索引里面的集合，索引也是一个小集合

![image-20230604215400607](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031703416.png)

再通过抓取，去抓取指定查回来的2条数据

![image-20230604215440187](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031703567.png)

## 涵盖的查询

Convered Queries

当查询条件和查询的投影仅包含索引字段时，MongoDB直接从索引返回结果，而不扫描任何文档或将文档带入内存，这些覆盖的查询可以非常有效

![image-20230604220253854](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031703796.png)

如果查询的数据在索引中已经有了就不会再去文档中去找了，而是直接从索引中 取

![image-20230604220509616](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031703030.png)

```shell
> db.comment.find({userid:"1003"},{userid:1,_id:0}).explain()
{
        "explainVersion" : "1",
        "queryPlanner" : {
                "namespace" : "articledb.comment",
                "indexFilterSet" : false,
                "parsedQuery" : {
                        "userid" : {
                                "$eq" : "1003"
                        }
                },
                "queryHash" : "8177476D",
                "planCacheKey" : "F842FA57",
                "maxIndexedOrSolutionsReached" : false,
                "maxIndexedAndSolutionsReached" : false,
                "maxScansToExplodeReached" : false,
                "winningPlan" : {
                        "stage" : "PROJECTION_COVERED",
                        "transformBy" : {
                                "userid" : 1,
                                "_id" : 0
                        },
                        "inputStage" : {
                                "stage" : "IXSCAN",
                                "keyPattern" : {
                                        "userid" : 1
                                },
                                "indexName" : "userid_1",
                                "isMultiKey" : false,
                                "multiKeyPaths" : {
                                        "userid" : [ ]
                                },
                                "isUnique" : false,
                                "isSparse" : false,
                                "isPartial" : false,
                                "indexVersion" : 2,
                                "direction" : "forward",
                                "indexBounds" : {
                                        "userid" : [
                                                "[\"1003\", \"1003\"]"
                                        ]
                                }
                        }
                },
                "rejectedPlans" : [ ]
        },
        "command" : {
                "find" : "comment",
                "filter" : {
                        "userid" : "1003"
                },
                "projection" : {
                        "userid" : 1,
                        "_id" : 0
                },
                "$db" : "articledb"
        },
        "serverInfo" : {
                "host" : "localhost.localdomain",
                "port" : 27017,
                "version" : "5.0.18",
                "gitVersion" : "796abe56bfdbca6968ff570311bf72d93632825b"
        },
        "serverParameters" : {
                "internalQueryFacetBufferSizeBytes" : 104857600,
                "internalQueryFacetMaxOutputDocSizeBytes" : 104857600,
                "internalLookupStageIntermediateDocumentMaxSizeBytes" : 104857600,
                "internalDocumentSourceGroupMaxMemoryBytes" : 104857600,
                "internalQueryMaxBlockingSortMemoryUsageBytes" : 104857600,
                "internalQueryProhibitBlockingMergeOnMongoS" : 0,
                "internalQueryMaxAddToSetBytes" : 104857600,
                "internalDocumentSourceSetWindowFieldsMaxMemoryBytes" : 104857600
        },
        "ok" : 1
}
>
```

## 文章评论

### 需求分析

某头条的文章评论业务如下：

需要实现以下功能：

1.  基本增删改查API
2.  根据文章id查询评论
3.  评论点赞

### 表结构分析

数据库：articledb

| 专栏文章评论   | comment        |                  |                           |
| -------------- | -------------- | ---------------- | ------------------------- |
| 字段名称       | 字段含义       | 字段类型         | 备注                      |
| _id            | ID             | ObjectId或String | Mongo的主键的字段         |
| articleid      | 文章ID         | String           |                           |
| content        | 评论内容       | String           |                           |
| userid         | 评论人ID       | String           |                           |
| nickname       | 评论人昵称     | String           |                           |
| createdatetime | 评论的日期时间 | Date             |                           |
| likenum        | 点赞数         | int32            |                           |
| replynum       | 回复数         | int32            |                           |
| state          | 状态           | String           | 0:不可见；1:可见；        |
| parentid       | 上级ID         | String           | 如果为0表示文章的顶级评论 |

### 技术选型

mongodb-driver

mongodb-driver是MongoDB官方推出的java连接mongoDB的驱动包，相当于JDBC驱动，我们通过一个入门的案例来了解mongodb-driver的基本使用

官方驱动说明和下载：https://mongodb.github.io/mongo-java-driver/

官方驱动示例文档：https://mongodb.github.io/mongo-java-driver/3.8/driver/getting-started/quick-start/

### SpringDataMongoDB

SpringData家族成员之一，用于操作MongoDB的持久层框架，封装了底层的MongoDB-Driver

官方主页：https://spring.io/projects/spring-data-mongodb/

我们十次方项目的吐槽微服务就采用SpringDataMongoDB框架

1.创建好SpringBoot的项目结构

2.导入相关依赖

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-mongodb</artifactId>
</dependency>
```

3.配置mongodb连接配置

```yaml
server:
  port: 80
spring:
  #数据源配置
  data:
    mongodb:
      #主机地址
      host: 192.168.244.142
      #数据库
      database: articledb
      #端口号
      port: 27017
      #也可以使用uri连接
      #uri: mongodb://192.168.244.142:27017/articledb
```

4.启动SpringBoot查看是否有报错

![image-20230605084426815](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031703699.png)

一切顺利

### 文章评论实体类的编写

创建实体类

```java
import lombok.Getter;
import lombok.Setter;
import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.index.CompoundIndex;
import org.springframework.data.mongodb.core.index.Indexed;
import org.springframework.data.mongodb.core.mapping.Document;
import org.springframework.data.mongodb.core.mapping.Field;

import java.io.Serializable;
import java.time.LocalDateTime;
import java.util.Date;

@Getter
@Setter
//对应collection名
@Document(collection = "comment")//可以省略，如果省略，则默认使用类名小写映射集合
//复合索引
@CompoundIndex(def = "{'userid':1,'nickname':-1}")
public class Comment implements Serializable {
//    主键标识，该属性的值会自动对应mongodb的主键字段_id，如果该属性名就叫id，则该注解可以省略，否则必须写
    @Id
    private String id;//主键
//    该属性对应mongodb的字段名字，如果一致，则无需该注解
    @Field("content")
    private String content;//吐槽内容
    private Date publishtime;//发布日期
//    添加了一个单字段的索引
    @Indexed
    private String userid;//发布人ID
    private String nickname;//昵称
    private LocalDateTime createdatatime;//评论的日期时间
    private Integer likenum;//点赞数
    private Integer replynum;//回复数
    private String state;//状态
    private String parentid;//上级ID
    private String articleid;//文章ID
}
```

### 文章评论的基本增删改查

创建数据访问接口

```java
import com.dkx.domain.Comment;
import org.springframework.data.mongodb.repository.MongoRepository;
//评论的持久层接口
public interface CommentRepository extends MongoRepository<Comment,String> {

}
```

2.创建业务逻辑类

```java
import com.dkx.dao.CommentRepository;
import com.dkx.domain.Comment;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@SuppressWarnings("all")
@Service
public class CommentService {
    @Autowired
    private CommentRepository commentRepository;

    /**
     * 保存一个评论
     * @param comment
     */
    public void save(Comment comment){
        /**
         * 如果需要自定义主键，可以在这里指定主键；如果不指定主键，MongoDB会自动生成主键
         * 设置一些默认初始值。。。
         * 调用dao
         */
        commentRepository.save(comment);
    }

    /**
     * 更新评论
     * @param comment
     */
    public void updateComment(Comment comment){
        //调用dao
        commentRepository.save(comment);
    }

    /**
     * 根据id删除评论
     * @param id
     */
    public void deleteCommentById(String id){
        //调用dao
        commentRepository.deleteById(id);
    }

    /**
     * 查询所有评论
     * @return
     */
    public List<Comment> findCommentList(){
        return commentRepository.findAll();
    }

    /**
     * 根据id查询评论
     * @param id
     * @return
     */
    public Comment findCommentById(String id){
        return commentRepository.findById(id).get();
    }
}
```

测试代码：

获取全部数据

```java
@SpringBootTest(classes = App.class)
@RunWith(SpringJUnit4ClassRunner.class)
public class AppTest {
    @Autowired
    private CommentService commentService;
    @Test
    public void test() {
        List<Comment> list = commentService.findCommentList();
        for(Comment i:list){
            System.out.println(i);
        }
    }
}
----------------------RUN Result ----------------------------
Comment(id=1, content=多喝凉水, publishtime=Sat Jan 02 08:00:00 CST 2021, userid=1003, nickname=山海经, createdatatim=2021-01-02T08:00, likenum=10001, replynum=3, state=1, parentid=1, articleid=1001)
```

添加一条数据

```java
    @Test
    public void test1(){
        Comment comment = new Comment();
        comment.setId("2");
        comment.setContent("我天天喝");
        comment.setPublishtime(new Date());
        comment.setUserid("1001");
        comment.setNickname("凯撒");
        comment.setCreatedatatim(LocalDateTime.now());
        comment.setLikenum(10003);
        comment.setReplynum(4);
        comment.setState("1");
        comment.setParentid("1");
        comment.setArticleid("1001");
        commentService.save(comment);
    }
------------------RUN Result Check Terminal Mongo DataStatement-------------------
> db.comment.find()
{ "_id" : "1", "articleid" : "1001", "content" : "多喝凉水", "userid" : "1003", "nickname" : "山海经", "createdatatim" : ISODate("2021-01-02T00:00:00Z"), "likenum" : "10001", "replynum" : "3", "state" : "1", "parentid" : "0", "publishtime" : ISODate("2021-01-02T00:00:00Z") }
{ "_id" : "2", "content" : "我天天喝", "publishtime" : ISODate("2023-06-05T02:26:15.397Z"), "userid" : "1001", "nickname" : "凯撒", "createdatatim" : ISODate("2023-06-05T02:26:15.405Z"), "likenum" : 10003, "replynum" : 4, "state" : "1", "parentid" : "1", "articleid" : "1001", "_class" : "com.dkx.domain.Comment" }
>
```

### 根据上级ID查询文章评论的分页列表

1.CommentRepository新增方法定义

```java
import com.dkx.domain.Comment;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.mongodb.repository.MongoRepository;
//评论的持久层接口
@SuppressWarnings("all")
public interface CommentRepository extends MongoRepository<Comment,String> {
//    方法名必须是findBy语法Parentid对应集合的字段的不能写错或者乱写都是不行的
    Page<Comment> findByParentid(String parentid,Pageable pageable);
}
```

2.编写service代码

```java
/**
 * 分页查询上级ID
 * @param parentid
 * @param page
 * @param size
 * @return
 */
public Page<Comment> findCommentListByParentid(String parentid,int page,int size){
   return commentRepository.findByParentid(parentid, PageRequest.of(page - 1,size));
}
```

3.测试代码：

先查看集合的数据

```shell
> db.comment.find()
{ "_id" : "1", "articleid" : "1001", "content" : "多喝凉水", "userid" : "1003", "nickname" : "山海经", "createdatatim" : ISODate("2021-01-02T00:00:00Z"), "likenum" : "10001", "replynum" : "3", "state" : "1", "parentid" : "0", "publishtime" : ISODate("2021-01-02T00:00:00Z") }
{ "_id" : "2", "content" : "我天天喝", "publishtime" : ISODate("2023-06-05T02:26:15.397Z"), "userid" : "1001", "nickname" : "凯撒", "createdatatim" : ISODate("2023-06-05T02:26:15.405Z"), "likenum" : 10003, "replynum" : 4, "state" : "1", "parentid" : "1", "articleid" : "1001", "_class" : "com.dkx.domain.Comment" }
{ "_id" : ObjectId("647d5340b50522086f5836bb"), "userid" : "1001", "parentid" : "1", "nickname" : "刘桑" }
{ "_id" : ObjectId("647d5340b50522086f5836bc"), "userid" : "1002", "parentid" : "1", "nickname" : "张三" }
{ "_id" : ObjectId("647d5340b50522086f5836bd"), "userid" : "1004", "parentid" : "1", "nickname" : "李四" }
>
```

parentid为1的数据有4条，测试代码中传入查询2条数据

```java
@Test
public void test2(){
   Page<Comment> list = commentService.findCommentListByParentid("1",1,2);
   for(Comment i:list){
      System.out.println(i);
   }
}
-----------------RUN Result-----------------
Comment(id=647d5340b50522086f5836bb, content=null, publishtime=null, userid=1001, nickname=刘桑, createdatatim=null, likenum=null, replynum=null, state=null, parentid=1, articleid=null)
```

### MongoTemplate实现评论点赞

我们看一下以下点赞的临时示例代码：CommentService新增updateThumbup方法

```java
/**
 * 点赞-效率低
 * @Param id
 */
public void updateCommentThumbupToIncrementingOld(String id){
   Comment comment = CommentRepository.findById(id).get();
   comment.setLikenum(comment.getLikenum()+1);
   CommentRepository.save(comment);
}
```

以上方法虽然实现起来比较简单，但是执行效率并不高，因为我只需要将点赞数加1就可以了，没必要查询出所有字段修改后再更新所有字段(蝴蝶效应)

我们可以使用MongoTemplate类来实现对某列的操作

1.修改CommentService

```java
@Autowired
private MongoTemplate mongoTemplate;

public void updateCommentLikenum(String id){
   //        查询对象
   Query query = Query.query(Criteria.where("_id").is(id));
   //        更新对象
   Update update = new Update();
   //        递增$inc
   update.inc("likenum");
   /**
         * 参数1：查询对象 query
         * 参数2：更新对象 update
         * 参数3：集合的名字或实体类的类型Comment.class
         */
   mongoTemplate.updateFirst(query,update,"comment");
}
```

2.测试代码：

```java
@Test
public void test4(){
   commentService.updateCommentLikenum("2");
}
----------------RUN Result mongo Terminal statment----------------
修改前
{ "_id" : "2", "content" : "我天天喝", "publishtime" : ISODate("2023-06-05T02:26:15.397Z"), "userid" : "1001", "nickname" : "凯撒", "createdatatim" : ISODate("2023-06-05T02:26:15.405Z"), "likenum" : 10003, "replynum" : 4, "state" : "1", "parentid" : "1", "articleid" : "1001", "_class" : "com.dkx.domain.Comment" }
修改后
{ "_id" : "2", "content" : "我天天喝", "publishtime" : ISODate("2023-06-05T02:26:15.397Z"), "userid" : "1001", "nickname" : "凯撒", "createdatatim" : ISODate("2023-06-05T02:26:15.405Z"), "likenum" : NumberLong(10004), "replynum" : 4, "state" : "1", "parentid" : "1", "articleid" : "1001", "_class" : "com.dkx.domain.Comment" }
```



