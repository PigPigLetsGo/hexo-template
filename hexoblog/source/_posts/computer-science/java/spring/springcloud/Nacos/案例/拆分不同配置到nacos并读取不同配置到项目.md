---
title: 拆分不同配置到nacos并读取不同配置到项目
categories:
    - [计算机学科,java,spring,springcloud,Nacos,案例]
tags:
    - 计算机学科
    - springcloud
    - Nacos
    - 案例Demo
    - 项目
---

# 拆分不同配置到nacos并读取不同配置到项目

先来看我们原先配置的内容：application.yml，服务名：gulimail-coupon

```yaml
server:
  port: 7000
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://192.168.56.10:3306/gulimall_sms?characterEncoding=utf-8&serverTimezone=UTC&useUnicode=true
    username: root
    password: root
  cloud:
    nacos:
      discovery:
        server-addr: 192.168.56.10:8848
  application:
    name: gulimail-coupon
mybatis-plus:
  global-config:
    db-config:
      id-type: auto
  mapper-locations: classpath:/mapper/**/*.xml
```

我们可以看到这个配置文件中，配置了数据源信息和mybatis还有其它的一些信息我们要将它们进行拆分分别配置到nacos中的同一个环境下不同的配置名称里面

比如，如下：

![image-20240324113845358](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240324113845358.png)

我们将 数据源相关配置信息配置到如下中：

![image-20240324113916733](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240324113916733.png)

我们将mybatis相关配置信息配置到如下中：

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240324113916733.png)

我们将其它的配置信息配置到如下中：

![image-20240324114009300](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240324114009300.png)

我们把这些个配置信息都分开配置了，那我们如何在代码中进行获取呢？如下演示：

```yaml
spring:
  application:
    name: gulimail-coupon
  cloud:
    nacos:
      config:
        server-addr: 192.168.56.10:8848
        file-extension: yaml
        namespace: 2d9e9ebf-1b0f-43d0-9fc8-d7d50057c193
        ext-config:
          - data-id: datasource.yaml
            group: dev
            refresh: true
          - data-id: mybatis.yaml
            group: dev
            refresh: true
          - data-id: other.yaml
            group: dev
            refresh: true
```

我们可以通过 ext-config (扩展配置) 来进行读取nacos中不同的配置内容

