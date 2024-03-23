---
title: Nacos安装指南
categories:
    - [计算机学科,java,spring,springcloud,Nacos]
tags:
    - 计算机学科
    - springcloud
    - Nacos
    - 基础
---

# Nacos安装指南

# 1.Windows安装

开发阶段采用单机安装即可。

## 1.1.下载安装包

在Nacos的GitHub页面，提供有下载链接，可以下载编译好的Nacos服务端或者源代码：

GitHub主页：https://github.com/alibaba/nacos

GitHub的Release下载页：https://github.com/alibaba/nacos/releases

如图：

![image-20210402161102887](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218392.png)



本课程采用1.4.1.版本的Nacos，课前资料已经准备了安装包：

![image-20210402161130261](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218840.png)

windows版本使用`nacos-server-1.4.1.zip`包即可。



## 1.2.解压

将这个包解压到任意非中文目录下，如图：

![image-20210402161843337](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218669.png)

目录说明：

- bin：启动脚本
- conf：配置文件



## 1.3.端口配置

Nacos的默认端口是8848，如果你电脑上的其它进程占用了8848端口，请先尝试关闭该进程。

**如果无法关闭占用8848端口的进程**，也可以进入nacos的conf目录，修改配置文件中的端口：

![image-20210402162008280](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218946.png)

修改其中的内容：

![image-20210402162251093](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218758.png)



## 1.4.启动

启动非常简单，进入bin目录，结构如下：

![image-20210402162350977](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218561.png)

然后执行命令即可：

- windows命令：

  ```
  startup.cmd -m standalone
  ```


执行后的效果如图：

![image-20210402162526774](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218179.png)



## 1.5.访问

在浏览器输入地址：http://127.0.0.1:8848/nacos即可：

![image-20210402162630427](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218689.png)

默认的账号和密码都是nacos，进入后：

![image-20210402162709515](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218382.png)





# 2.Linux安装

Linux或者Mac安装方式与Windows类似。

## 2.1.安装JDK

Nacos依赖于JDK运行，所以Linux上也需要安装JDK才行。

上传jdk安装包：

![image-20210402172334810](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218243.png)

上传到某个目录，例如：`/usr/local/`



然后解压缩：

```sh
tar -xvf jdk-8u144-linux-x64.tar.gz
```

然后重命名为java



配置环境变量：

```sh
export JAVA_HOME=/usr/local/java
export PATH=$PATH:$JAVA_HOME/bin
```

刷新环境变量：

```sh
source /etc/profile
```





## 2.2.上传安装包

如图：

![image-20210402161102887](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218879.png)

也可以直接使用课前资料中的tar.gz：

![image-20210402161130261](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218287.png)

上传到Linux服务器的某个目录，例如`/usr/local/src`目录下：

![image-20210402163715580](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218025.png)



## 2.3.解压

命令解压缩安装包：

```sh
tar -xvf nacos-server-1.4.1.tar.gz
```

然后删除安装包：

```sh
rm -rf nacos-server-1.4.1.tar.gz
```

目录中最终样式：

![image-20210402163858429](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218677.png)

目录内部：

![image-20210402164414827](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051218771.png)



## 2.4.端口配置

与windows中类似



## 2.5.启动

在nacos/bin目录中，输入命令启动Nacos：

```sh
sh startup.sh -m standalone
```

# 3.Nacos的依赖

父工程：

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-alibaba-dependencies</artifactId>
    <version>2.2.5.RELEASE</version>
    <type>pom</type>
    <scope>import</scope>
</dependency>
```

客户端：

```xml
<!-- nacos客户端依赖包 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```



# 4. Docker安装nacos



通过指定 Nacos 镜像的标签（tag）来选择特定版本。对于 Nacos 1.4.1 版本，你可以使用 `**1.4.1**` 标签。以下是在 Linux 的 Docker 中创建一个 Nacos 1.4.1 容器的步骤：

1. **拉取 Nacos 1.4.1 镜像**:

   打开终端，执行以下命令拉取 Nacos 1.4.1 镜像：

   ```bash
   docker pull nacos/nacos-server:1.4.1
   ```

   这将从 Docker Hub 上下载 Nacos 1.4.1 版本的官方镜像

2. 创建数据库我们到数据库中先创建一个数据库名为nacos或者随意都行

   sql表 到官网的github中copy就行了然后创建即可：https://github.com/alibaba/nacos/blob/master/config/src/main/resources/META-INF/nacos-db.sql

   ![image-20240322085318286](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240322085318286.png)

    2.1 创建挂在目录和配置文件如下

   ```bash
   mkdir -p /mydata/nacos/logs/                      #新建logs目录
   mkdir -p /mydata/nacos/init.d/          
   vim /mydata/nacos/init.d/custom.properties        #修改配置文件
   ```

   修改custom.properties配置文件：

   把下面的数据库连接配置修改为自己的

   ```properties
   server.contextPath=/nacos
   server.servlet.contextPath=/nacos
   server.port=8848
   
   spring.datasource.platform=mysql
   db.num=1
   db.url.0=jdbc:mysql://xx.xx.xx.x:3306/nacos_config? characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true #这里需要修改端口
   db.user=user #用户名
   db.password=pass #密码
   
   nacos.cmdb.dumpTaskInterval=3600
   nacos.cmdb.eventTaskInterval=10
   nacos.cmdb.labelTaskInterval=300
   nacos.cmdb.loadDataAtStart=false
   management.metrics.export.elastic.enabled=false
   management.metrics.export.influx.enabled=false
   server.tomcat.accesslog.enabled=true
   server.tomcat.accesslog.pattern=%h %l %u %t "%r" %s %b %D %{User-Agent}i
   nacos.security.ignore.urls=/,/**/*.css,/**/*.js,/**/*.html,/**/*.map,/**/*.svg,/**/*.png,/**/*.ico,/console-fe/public/**,/v1/auth/login,/v1/console/health/**,/v1/cs/**,/v1/ns/**,/v1/cmdb/**,/actuator/**,/v1/console/server/**
   nacos.naming.distro.taskDispatchThreadCount=1
   nacos.naming.distro.taskDispatchPeriod=200
   nacos.naming.distro.batchSyncKeyCount=1000
   nacos.naming.distro.initDataRatio=0.9
   nacos.naming.distro.syncRetryDelay=5000
   nacos.naming.data.warmup=true
   nacos.naming.expireInstance=true
   ```

   

3. **创建并运行 Nacos 容器**:

   执行以下命令创建并运行 Nacos 容器：

   ```bash
   docker  run \
   --name nacos -d \
   -p 8848:8848 \
   --privileged=true \
   --restart=always \
   -e JVM_XMS=256m \
   -e JVM_XMX=256m \
   -e MODE=standalone \
   -e PREFER_HOST_MODE=hostname \
   -v /mydata/nacos/logs:/home/nacos/logs \
   -v /mydata/nacos/init.d/custom.properties:/home/nacos/init.d/custom.properties \
   nacos/nacos-server
   ```

   可用设置：docker update --restart=always  nacos

   这将在后台运行一个名为 `**nacos-server**` 的容器，并将容器的 8848 端口映射到主机的 8848 端口。

   参数说明：

   -  -e MODE=standalone 单机启动
   -  --restart=always启动docker时自动启动nacos

   
