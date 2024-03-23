---
title: 切换内置web服务器
categories:
   - [计算机学科,java,spring,springboot,高级]
tags:
   - 计算机学科
   - springboot高级
---

## 切换内置Web服务器

SpringBoot的web环境中默认使用tomcat作为内置服务器，其实SpringBoot提供了4种内置服务器供我们选择，我们可以很方便的进行切换。

查看依赖的路径：

spring-boot-autoconfigure / org / autoconfigure / web / embedded 

其中的文件：

![image-20230910212613275](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309102126343.png)

这四个文件就是对应的web服务器的工厂

第一个：Jetty 服务器

第二个：Netty 服务器

第三个：tomcat 服务器

第四个：Undertow 服务器

排除默认的tomcat启动 服务器，改为 jetty 服务器 启动

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <!--排除tomcat依赖-->
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <!--引入jetty依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jetty</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

启动后查看是使用的jetty启动 的服务器

![image-20230910214302727](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309102143226.png)