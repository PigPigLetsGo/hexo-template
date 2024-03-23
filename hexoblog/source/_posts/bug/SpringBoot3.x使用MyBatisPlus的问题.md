---
title: SpringBoot3.x使用MyBatisPlus的问题
categories:
    - [问题总汇]
tags:
    - 问题总汇
    - MyBatisPlus
    - 版本问题
---

问题描述：

>  springboot由3.1.5升级到3.2.0 报Invalid value type for attribute ‘factoryBeanObjectType‘: java.lang.String

目前mybatisplus没有支持SpringBoot3.2.2的所以我们需要使用如下坐标来解决问题：

```.xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-spring-boot3-starter</artifactId>
    <version>3.5.5</version>
</dependency>
```

