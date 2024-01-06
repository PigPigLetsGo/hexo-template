---
title: XXL-JOB介绍和使用
categories:
    - [计算机学科,java,spring,springcloud]
tags:
    - 计算机学科
    - springcloud
    - 基础
---

# 一、 XXL-JOB介绍

XXL-JOB是一个轻量级分布式任务调度平台，其核心设计目标是开发迅速、学习简单、轻量级、易扩展。现已开放源代码并接入多家公司线上产品线，开箱即用。

官网：https://www.xuxueli.com/xxl-job/

文档：https://www.xuxueli.com/xxl-job/#%E3%80%8A%E5%88%86%E5%B8%83%E5%BC%8F%E4%BB%BB%E5%8A%A1%E8%B0%83%E5%BA%A6%E5%B9%B3%E5%8F%B0XXL-JOB%E3%80%8B

XXL-JOB主要有调度中心、执行器、任务：

![image-20231102114124616](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021141945.png)

**调度中心：**

​    负责管理调度信息，按照调度配置发出调度请求，自身不承担业务代码；

​    主要职责为执行器管理、任务管理、监控运维、日志管理等

**任务执行器：**

​    负责接收调度请求并执行任务逻辑；

​    只要职责是注册服务、任务执行服务（接收到任务后会放入线程池中的任务队列）、执行结果上报、日志服务等

**任务：**负责执行具体的业务处理。

 

调度中心与执行器之间的工作流程如下：

![image-20231102114151941](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021142092.png)

**执行流程：** 

​    1.任务执行器根据配置的调度中心的地址，自动注册到调度中心

​    2.达到任务触发条件，调度中心下发任务

​    3.执行器基于线程池执行任务，并把执行结果放入内存队列中、把执行日志写入日志文件中

​    4.执行器消费内存队列中的执行结果，主动上报给调度中心

​    5.当用户在调度中心查看任务日志，调度中心请求任务执行器，任务执行器读取任务日志文件并返回日志详情

---

# 二、使用配置

执行命令拉取xxl-job-2.3.1

```sh
docker pull xuxueli/xxl-job-admin:2.3.1
```

拉取成功后，创建挂载配置路径：

```sh
mkdir -p -m 777 /root/tmp/xxl-job/data/applogs
```

创建完成后在 /root/tem/xxl-job/目录中 创建 配置文件 application.properties

![image-20231102102637272](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021026700.png)

配置内容如下：

里面就更改下自己的ip地址和数据库账户和密码即可。

```yml
### web
server.port=8080
server.servlet.context-path=/xxl-job-admin
 
### actuator
management.server.servlet.context-path=/actuator
management.health.mail.enabled=false
 
### resources
spring.mvc.servlet.load-on-startup=0
spring.mvc.static-path-pattern=/static/**
spring.resources.static-locations=classpath:/static/
 
### freemarker
spring.freemarker.templateLoaderPath=classpath:/templates/
spring.freemarker.suffix=.ftl
spring.freemarker.charset=UTF-8
spring.freemarker.request-context-attribute=request
spring.freemarker.settings.number_format=0.##########
 
### mybatis
mybatis.mapper-locations=classpath:/mybatis-mapper/*Mapper.xml
#mybatis.type-aliases-package=com.xxl.job.admin.core.model
 
### xxl-job, datasource
spring.datasource.url=jdbc:mysql://192.168.249.128:3306/xxl_job?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai
spring.datasource.username=root
spring.datasource.password=dkx.
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
 
### datasource-pool
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.hikari.minimum-idle=10
spring.datasource.hikari.maximum-pool-size=30
spring.datasource.hikari.auto-commit=true
spring.datasource.hikari.idle-timeout=30000
spring.datasource.hikari.pool-name=HikariCP
spring.datasource.hikari.max-lifetime=900000
spring.datasource.hikari.connection-timeout=10000
spring.datasource.hikari.connection-test-query=SELECT 1
spring.datasource.hikari.validation-timeout=1000
 
### xxl-job, email
spring.mail.host=smtp.qq.com
spring.mail.port=25
spring.mail.username=xxx@qq.com
spring.mail.from=xxx@qq.com
spring.mail.password=xxx
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true
spring.mail.properties.mail.smtp.starttls.required=true
spring.mail.properties.mail.smtp.socketFactory.class=javax.net.ssl.SSLSocketFactory
 
### xxl-job, access token
xxl.job.accessToken=default_token
 
### xxl-job, i18n (default is zh_CN, and you can choose "zh_CN", "zh_TC" and "en")
xxl.job.i18n=zh_CN
 
## xxl-job, triggerpool max size
xxl.job.triggerpool.fast.max=200
xxl.job.triggerpool.slow.max=100
 
### xxl-job, log retention days
xxl.job.logretentiondays=30
```

到github中 拉取一下项目里面有一个sql文件：https://github.com/xuxueli/xxl-job

或者到码云：https://gitee.com/xuxueli0323/xxl-job 拉取项目。主要是sql文件啊

我们需要找到这个sql文件然后创建一个数据库并导入sql文件，再进行下一步！

![image-20231102103400507](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021034863.png)

![image-20231102103536623](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021035033.png)

配置完成后启动xxl-job-2.3.1并创建容器：

```sh
docker run  -p 18080:8080 -d --name=xxl --restart=always -v /root/tmp/xxl-job/application.properties:/application.properties  -e PARAMS='--spring.config.location=/application.properties' xuxueli/xxl-job-admin:2.3.1
```

启动成功后访问uri：http://192.168.249.128:18080/xxl-job-admin/jobgroup

账号：admin ，密码：123456

访问后页面如下：

![image-20231102102848028](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021028455.png)

创建一个执行器

![image-20231102104642146](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021046055.png)

点击新增，填写执行器信息，appname是前边在nacos中配置xxl信息时指定的执行器的应用名。

![image-20231102104700103](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021047493.png)

添加成功：

![image-20231102104719719](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021047508.png)

在需要用到的业务模块中导入依赖：

```xml
<dependency>
   <groupId>com.xuxueli</groupId>
   <artifactId>xxl-job-core</artifactId>
</dependency>
```

配置application.yml如下，也可以配置到nacos中：

```yml
xxl:
  job:
    admin: 
      addresses: http://192.168.101.65:8088/xxl-job-admin
    executor:
      appname: media-process-service
      address: 
      ip: 
      port: 9999
      logpath: /data/applogs/xxl-job/jobhandler
      logretentiondays: 30
    accessToken: default_token
```

<font color='red'>注意</font>配置中的appname这是执行器的应用名，port是执行器启动的端口，如果本地启动多个执行器注意端口不能重复。

在从github或者gitee中拉取的xxl-job-2.3.1项目中找到

 E:\学成在线项目—资料\day06 断点续传 xxl-job\资料\xxl-job-2.3.1\xxl-job-executor-samples\xxl-job-executor-sample-springboot\src\main\java\com\xxl\job\executor\core\config\XxlJobConfig.java

配置文件

![image-20231102105732814](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021057889.png)

将该配置文件拷贝到 自己项目的该模块的config目录中

![image-20231102105838113](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021058604.png)

启动服务

![image-20231102110215950](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021102864.png)

可以看到服务就自动的注册到了执行器中了

![image-20231102110243545](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021102303.png)

点击 查看(1)

![image-20231102110416038](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021104929.png)

执行器执行任务

从xxl-job-2.3.1项目中先来拿到一个例子就是如下代码：

E:\学成在线项目—资料\day06 断点续传 xxl-job\资料\xxl-job-2.3.1\xxl-job-executor-samples\xxl-job-executor-sample-springboot\src\main\java\com\xxl\job\executor\service\jobhandler\SampleXxlJob.java

![image-20231102111700620](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021117873.png)

然后将这个代码，拷贝到自己项目的模块的service业务中。

```java
/**
 * XxlJob开发示例（Bean模式）
 *
 * 开发步骤：
 *      1、任务开发：在Spring Bean实例中，开发Job方法；
 *      2、注解配置：为Job方法添加注解 "@XxlJob(value="自定义jobhandler名称", init = "JobHandler初始化方法", destroy = "JobHandler销毁方法")"，注解value值对应的是调度中心新建任务的JobHandler属性的值。
 *      3、执行日志：需要通过 "XxlJobHelper.log" 打印执行日志；
 *      4、任务结果：默认任务结果为 "成功" 状态，不需要主动设置；如有诉求，比如设置任务结果为失败，可以通过 "XxlJobHelper.handleFail/handleSuccess" 自主设置任务结果；
 *
 * @author xuxueli 2019-12-11 21:52:51
 */

@Component
public class SampleXxlJob {
    private static Logger logger = LoggerFactory.getLogger(SampleXxlJob.class);

    /**
     * 1、简单任务示例（Bean模式）
     */
    @XxlJob("demoJobHandler")
    public void demoJobHandler() throws Exception {
			System.out.println("视频处理...");
        // default success
    }
}
```

![image-20231102111804565](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021118433.png)

然后就可以在这里面来编写自己的业务逻辑代码了。

那么这个任务执行器中的任务怎么执行呢，我们需要到控制台中的执行任务中进行配置：

![image-20231102112746770](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021127816.png)

其中的bean指的就是 执行任务 业务逻辑代码中注入的bean名称

也可以选择其它的比如Java那么右边的框就不能输入了，下面就是输入要执行的Java代码。

搞完上面的后就重启后端服务

![image-20231102113214411](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021132541.png)

启动服务后那 任务怎么执行呢，我们需要 ，在控制台 中 任务管理的 任务

![image-20231102113310041](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021133078.png)

启动后就可以看到每隔5秒后端控制台就会打印出一句话

```
c.xxl.job.core.executor.XxlJobExecutor   : >>>>>>>>>>> xxl-job regist JobThread success, jobId:2, handler:com.xxl.job.core.handler.impl.MethodJobHandler@1dcca426[class com.xuecheng.media.service.jobhandler.SampleXxlJob#demoJobHandler]
视频处理...
视频处理...
视频处理...
视频处理...
视频处理...
视频处理...
```

在运行报表中可以看到调度情况如下

![image-20231102113642675](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311021136278.png)