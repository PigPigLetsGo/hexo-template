---
title: 安装elasticsearch
categories:
    - [计算机学科,java,spring,springcloud,Elasticsearch]
tags:
    - 计算机学科
    - springcloud
    - Elasticsearch
    - 基础
---

# 安装elasticsearch

# 1.部署单点es

## 1.1.创建网络

因为我们还需要部署kibana容器，因此需要让es和kibana容器互联。这里先创建一个网络：

```sh
docker network create es-net
```

## 1.2.加载镜像

这里我们采用elasticsearch的7.12.1版本的镜像，这个镜像体积非常大，接近1G。不建议大家自己pull。

课前资料提供了镜像的tar包：

![image-20210510165308064](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112020462.png)

大家将其上传到虚拟机中，然后运行命令加载即可：

```sh
# 导入数据
docker load -i es.tar
```

同理还有`kibana`的tar包也需要这样做。

## 1.3.运行

运行docker命令，部署单点es：

```sh
docker run -d \
	--name es \
    -e "ES_JAVA_OPTS=-Xms512m -Xmx512m" \
    -e "discovery.type=single-node" \
    -v es-data:/usr/share/elasticsearch/data \
    -v es-plugins:/usr/share/elasticsearch/plugins \
    --privileged \
    --network es-net \
    -p 9200:9200 \
    -p 9300:9300 \
elasticsearch:7.12.1
```

命令解释：

- `-e "cluster.name=es-docker-cluster"`：设置集群名称
- `-e "http.host=0.0.0.0"`：监听的地址，可以外网访问
- `-e "ES_JAVA_OPTS=-Xms512m -Xmx512m"`：内存大小
- `-e "discovery.type=single-node"`：非集群模式
- `-v es-data:/usr/share/elasticsearch/data`：挂载逻辑卷，绑定es的数据目录
- `-v es-logs:/usr/share/elasticsearch/logs`：挂载逻辑卷，绑定es的日志目录
- `-v es-plugins:/usr/share/elasticsearch/plugins`：挂载逻辑卷，绑定es的插件目录
- `--privileged`：授予逻辑卷访问权
- `--network es-net` ：加入一个名为es-net的网络中
- `-p 9200:9200`：端口映射配置



在浏览器中输入：http://192.168.150.101:9200 即可看到elasticsearch的响应结果：

![image-20210506101053676](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112020815.png)



# 2.部署kibana

kibana可以给我们提供一个elasticsearch的可视化界面，便于我们学习。

## 2.1.部署

运行docker命令，部署kibana

kibana需要和elaticsearch处于同一个网络中

```sh
docker run -d \
--name kibana \
# 因为它们两个为同一个网络，所以可以使用容器名互联
-e ELASTICSEARCH_HOSTS=http://es:9200 \
--network=es-net \
-p 5601:5601  \
kibana:7.12.1
```

- `--network es-net` ：加入一个名为es-net的网络中，与elasticsearch在同一个网络中
- `-e ELASTICSEARCH_HOSTS=http://es:9200"`：设置elasticsearch的地址，因为kibana已经与elasticsearch在一个网络，因此可以用容器名直接访问elasticsearch
- `-p 5601:5601`：端口映射配置

kibana启动一般比较慢，需要多等待一会，可以通过命令：

```sh
docker logs -f kibana
```

查看运行日志，当查看到下面的日志，说明成功：

![image-20210109105135812](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112020301.png)

此时，在浏览器输入地址访问：http://192.168.150.101:5601，即可看到结果

## 2.2.DevTools

kibana中提供了一个DevTools界面：

![image-20210506102630393](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112020356.png)

这个界面中可以编写DSL来操作elasticsearch。并且对DSL语句有自动补全功能。



# 3.安装IK分词器



## 3.1.在线安装ik插件（较慢）

```shell
# 进入容器内部
docker exec -it elasticsearch /bin/bash

# 在线下载并安装
./bin/elasticsearch-plugin  install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v7.12.1/elasticsearch-analysis-ik-7.12.1.zip

#退出
exit
#重启容器
docker restart elasticsearch
```

## 3.2.离线安装ik插件（推荐）

### 1）查看数据卷目录

安装插件需要知道elasticsearch的plugins目录位置，而我们用了数据卷挂载，因此需要查看elasticsearch的数据卷目录，通过下面命令查看:

```sh
docker volume inspect es-plugins
```

显示结果：

```json
[
    {
        "CreatedAt": "2022-05-06T10:06:34+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/es-plugins/_data",
        "Name": "es-plugins",
        "Options": null,
        "Scope": "local"
    }
]
```

说明plugins目录被挂载到了：`/var/lib/docker/volumes/es-plugins/_data `这个目录中。

### 2）解压缩分词器安装包

下面我们需要把课前资料中的ik分词器解压缩，重命名为ik

![image-20210506110249144](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112020922.png)

### 3）上传到es容器的插件数据卷中

也就是`/var/lib/docker/volumes/es-plugins/_data `：

![image-20210506110704293](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112020853.png)

###  4）重启容器

```shell
# 4、重启容器
docker restart es
```

```sh
# 查看es日志
docker logs -f es
```

看到这条信息说明安装成功了

![image-20231011220033681](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112200825.png)

### 5）测试：

IK分词器包含两种模式：

* `ik_smart`：最少切分
   * 可以搜索到的词的范围比较小

* `ik_max_word`：最细切分
   * 可以搜索到的词的范围比较大


```json
GET /_analyze
{
  "analyzer": "ik_max_word",
  "text": "黑马程序员学习java太棒了"
}
```

结果：

ik_smart

```json
{
  "tokens" : [
    {
      "token" : "黑马",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "程序员",
      "start_offset" : 2,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 1
    },
    {
      "token" : "学习",
      "start_offset" : 5,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 2
    },
    {
      "token" : "java",
      "start_offset" : 7,
      "end_offset" : 11,
      "type" : "ENGLISH",
      "position" : 3
    },
    {
      "token" : "太棒了",
      "start_offset" : 11,
      "end_offset" : 14,
      "type" : "CN_WORD",
      "position" : 4
    }
  ]
}
```



ik_max_word

```json
{
  "tokens" : [
    {
      "token" : "黑马",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "CN_WORD",
      "position" : 0
    },
    {
      "token" : "程序员",
      "start_offset" : 2,
      "end_offset" : 5,
      "type" : "CN_WORD",
      "position" : 1
    },
    {
      "token" : "程序",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "CN_WORD",
      "position" : 2
    },
    {
      "token" : "员",
      "start_offset" : 4,
      "end_offset" : 5,
      "type" : "CN_CHAR",
      "position" : 3
    },
    {
      "token" : "学习",
      "start_offset" : 5,
      "end_offset" : 7,
      "type" : "CN_WORD",
      "position" : 4
    },
    {
      "token" : "java",
      "start_offset" : 7,
      "end_offset" : 11,
      "type" : "ENGLISH",
      "position" : 5
    },
    {
      "token" : "太棒了",
      "start_offset" : 11,
      "end_offset" : 14,
      "type" : "CN_WORD",
      "position" : 6
    },
    {
      "token" : "太棒",
      "start_offset" : 11,
      "end_offset" : 13,
      "type" : "CN_WORD",
      "position" : 7
    },
    {
      "token" : "了",
      "start_offset" : 13,
      "end_offset" : 14,
      "type" : "CN_CHAR",
      "position" : 8
    }
  ]
}
```

## 3.3 扩展词词典

随着互联网的发展，“造词运动”也越发的频繁。出现了很多新的词语，在原有的词汇列表中并不存在。比如：“奥力给”，“传智播客” 等。

所以我们的词汇也需要不断的更新，IK分词器提供了扩展词汇的功能。

1）打开IK分词器config目录：

![image-20210506112225508](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112020613.png)

2）在IKAnalyzer.cfg.xml配置文件内容添加：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
        <comment>IK Analyzer 扩展配置</comment>
        <!--用户可以在这里配置自己的扩展字典 *** 添加扩展词典-->
        <entry key="ext_dict">ext.dic</entry>
</properties>
```

3）新建一个 ext.dic，可以参考config目录下复制一个配置文件进行修改

```properties
传智教育
奥力给
```

4）重启elasticsearch 

```sh
docker restart es

# 查看 日志
docker logs -f elasticsearch
```

![image-20201115230900504](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310112020980.png)

日志中已经成功加载ext.dic配置文件

5）测试效果：

```json
GET /_analyze
{
  "analyzer": "ik_max_word",
  "text": "传智播客Java就业超过90%,奥力给！"
}
```

> 注意当前文件的编码必须是 UTF-8 格式，严禁使用Windows记事本编辑

## 3.4 停用词词典

在互联网项目中，在网络间传输的速度很快，所以很多语言是不允许在网络上传递的，如：关于宗教、政治等敏感词语，那么我们在搜索时也应该忽略当前词汇。

IK分词器也提供了强大的停用词功能，让我们在索引时就直接忽略当前的停用词汇表中的内容。

1）IKAnalyzer.cfg.xml配置文件内容添加：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
        <comment>IK Analyzer 扩展配置</comment>
        <!--用户可以在这里配置自己的扩展字典-->
        <entry key="ext_dict">ext.dic</entry>
         <!--用户可以在这里配置自己的扩展停止词字典  *** 添加停用词词典-->
        <entry key="ext_stopwords">stopword.dic</entry>
</properties>
```

3）在 stopword.dic 添加停用词

```properties
牛逼
的
小
黑
子
IKUN
坤
鸡
```

需要注意：==牛逼==这个词，进行分词时会出现牛，逼 。因为如下解释：

IK查询停用词召回的时候会先将查询词根据分词词库配置进行分词，比如"牛逼"这个词会被分为"牛"，"逼"，之后会根据停用词库去看是否"牛"，"逼"两个词再库内，如在库内就不会召回。

说起来 可能抽象看如下的情况吧：

当我们查询 “牛逼的时候” 结果如下：

这里你肯定回想，我不是停用了吗怎么还有显示呢，那你就错了因为它被分词为 一个 “牛” 一个 “逼” 它并不是 “牛逼”了 ，我们再往下看

![image-20231012084748860](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310120847300.png)

我查询 “小” 的时候结果什么也没有 因为一个字被分词还是一个词而这一个词还是被停用的一次就能匹配到所以就停用看不到了

![image-20231012084916676](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310120849089.png)

4）重启elasticsearch 

```sh
# 重启服务
docker restart elasticsearch
docker restart kibana

# 查看 日志
docker logs -f elasticsearch
```

日志中已经成功加载stopword.dic配置文件

5）测试效果：

```json
GET /_analyze
{
  "analyzer": "ik_max_word",
  "text": "传智播客Java就业率超过95%,习大大都点赞,奥力给！"
}
```

> 注意当前文件的编码必须是 UTF-8 格式，严禁使用Windows记事本编辑

>  **总结**：
>
>  分词器的作用是什么？
>
>  -  创建倒排索引时对文档分词
>  -  用户搜索时，对输入的内容分词
>
>  IK分词器有几种模式？
>
>  -  ik_smart：智能切分，粗粒度
>  -  ik_max_word：最细切分，细粒度
>
>  IK分词器如何扩展词条？如何停用词条？
>
>  -  利用config目录的IkAnalyzer.cfg.xml文件添加扩展词典和停用词典
>  -  在词典中添加扩展词条或者停用词条

# 4.部署es集群

部署es集群可以直接使用docker-compose来完成，不过要求你的Linux虚拟机至少有**4G**的内存空间



首先编写一个docker-compose文件，内容如下：

```sh
version: '2.2'
services:
  es01:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.12.1
    container_name: es01
    environment:
      - node.name=es01
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es02,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data01:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - elastic
  es02:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.12.1
    container_name: es02
    environment:
      - node.name=es02
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es03
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data02:/usr/share/elasticsearch/data
    networks:
      - elastic
  es03:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.12.1
    container_name: es03
    environment:
      - node.name=es03
      - cluster.name=es-docker-cluster
      - discovery.seed_hosts=es01,es02
      - cluster.initial_master_nodes=es01,es02,es03
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - data03:/usr/share/elasticsearch/data
    networks:
      - elastic

volumes:
  data01:
    driver: local
  data02:
    driver: local
  data03:
    driver: local

networks:
  elastic:
    driver: bridge
```

将该配置文件上传到Linux中

![image-20231016105943788](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161059412.png)

es运行需要修改一些Linux系统权限，修改 `/etc/sysctl.conf` 文件：

```sh·
vi /etc/sysctl.conf
```

添加下面内容：

```sh
vm.max_map_count=262144
```

然后执行命令，让配置生效：

```sh
[root@localhost ~]# sysctl -p
vm.max_map_count = 262144
[root@localhost ~]#
```

Run `docker-compose` to bring up the cluster:

```sh
[root@localhost ~]# docker-compose up -d
Starting es03 ... done
Starting es01 ... done
Starting es02 ... done
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE                   COMMAND                  CREATED         STATUS                            PORTS                                                                                                                                                 NAMES
c3c4b0e1ffdc   elasticsearch:7.12.1    "/bin/tini -- /usr/l…"   2 minutes ago   Up 13 seconds                     0.0.0.0:9200->9200/tcp, :::9200->9200/tcp, 9300/tcp                                                                                                   es01
073f8e46e296   elasticsearch:7.12.1    "/bin/tini -- /usr/l…"   2 minutes ago   Up 13 seconds                     9300/tcp, 0.0.0.0:9201->9200/tcp, :::9201->9200/tcp                                                                                                   es02
aca2f691e3e2   elasticsearch:7.12.1    "/bin/tini -- /usr/l…"   2 minutes ago   Up 13 seconds                     9300/tcp, 0.0.0.0:9202->9200/tcp, :::9202->9200/tcp                                                                                                   es03
61161df3ec6a   kibana:7.12.1           "/bin/tini -- /usr/l…"   4 days ago      Up 18 minutes                     0.0.0.0:5601->5601/tcp, :::5601->5601/tcp                                                                                                             kibana
318bc511f26c   elasticsearch:7.12.1    "/bin/tini -- /usr/l…"   4 days ago      Exited (137) About a minute ago                                                                                                                                                         es
46756ed1a2ee   rabbitmq:3-management   "docker-entrypoint.s…"   4 days ago      Up 18 minutes                     4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp, :::5672->5672/tcp, 15671/tcp, 15691-15692/tcp, 25672/tcp, 0.0.0.0:15672->15672/tcp, :::15672->15672/tcp   mq
[root@localhost ~]#
```

## 4.1、集群状态监控

kibana可以监控es集群，不过新版本需要依赖es的x-pack功能，配置比较复杂。

这里推荐使用cerebro来监控es集群状态，官方网址：https://github.com/lmenezes/cerebro

下载后进行解压

![image-20231016111307167](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161113552.png)

解压后进入bin目录中

![image-20231016111341471](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161113838.png)

双击运行cerebro.bat即可运行

![image-20231016111523818](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161115237.png)

启动完成后访问：localhost:9000地址，证明正常启动成功了。

![image-20231016111547168](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161115161.png)

这时就可以访问虚拟机中es的哪个共同端口号的地址了：http://192.168.249.128:9200

![image-20231016115434836](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161154359.png)

## 4.2、创建索引库

### 4.2.1、利用kibana的DevTools创建索引库

在DevTools中输入指令：

```json
PUT /indexName
{
   "settings": {
      "number_of_shards": 3, // 分片数量
      "number_of_replicas": 1 // 副本数量
   },
   "mappings": {
      "properties": {
         // mapping 映射定义 ...
      }
   }
}
```



![image-20231016120105950](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161201547.png)

填写完成后点击create创建索引库

回到首页

可以看到数据被成功的分成了三片，名称为itcast

但是还加了一个副本，每一片都加一个副本就是3 * 2 = 6 片

其中实线就是住分片，虚线就是副本分片（就是拷贝了一份）

每一个分片它的备份都在不同的机器上，这样就确保了任意一台机器宕机，那的备份依然还保留着避免出现数据故障

![image-20231016120225681](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310161202468.png)