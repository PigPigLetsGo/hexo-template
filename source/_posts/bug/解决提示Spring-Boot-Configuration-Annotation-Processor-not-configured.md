---
title: 解决提示Spring-Boot-Configuration-Annotation-Processor-not-configured
categories:
    - [问题总汇]
tags:
    - 问题总汇
    - springboot
---

问题描述

进行SpringBoot配置文件部署时,发出警告SpringBoot Configuration Annotation Processor not cofigured,但是不影响运行

![![image_2023-03-09-19-38-58](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-09-19-38-58_20230309200003.png)](%E8%A7%A3%E5%86%B3%E6%8F%90%E7%A4%BASpring-Boot-Configuration-Annotation-Processor-not-configured_md_files/image_2023-03-09-19-38-58_20230309200003.png?v=1&type=image&token=V1:UFZ6SM6wF0-ikbNfHn4v8QibjgxYU0ngoWudlnVPyLE)

问题解决方案:

在pom.xml文件中引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

问题分析

它的意思是"SpringBoot"配置注解执行器没有配置,配置注解执行器的好处是什么.

配置注解执行器配置完成后,当执行类中已经定义了对象和该对象的字段后,在配置文件中对该类赋值时,便会非常方便的弹出提示信息

例如:

![![image_2023-03-09-19-41-28](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-09-19-41-28_20230309200017.png)](%E8%A7%A3%E5%86%B3%E6%8F%90%E7%A4%BASpring-Boot-Configuration-Annotation-Processor-not-configured_md_files/image_2023-03-09-19-41-28_20230309200017.png?v=1&type=image&token=V1:8BVlEIXBCRRjyVLgql4uS_M4uWcFWZIGic1kmSX1Y98)
