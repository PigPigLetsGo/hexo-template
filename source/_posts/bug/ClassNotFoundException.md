---
title: java.lang.ClassNotFoundException:javax.xml.bind.DatatypeConverter
categories:
    - [问题总汇]
tags:
    - 问题总汇
    - jdk版本问题
---

### java.lang.ClassNotFoundException:javax.xml.bind.DatatypeConverter

报错描述：

java.lang.ClassNotFoundException:javax.xml.bind.DatatypeConverter

原因：可能是因为SpringBoot项目结合JWT进行登录时出现的问题，因为jdk版本太高导致的。

#### 解决方案：

##### 一，降低jdk版本

##### 二，在maven中添加依赖

```xml
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.1</version>
</dependency>
```

因为javax.xml.bind在jdk8中 有，但是在更高版本就没有了，所以我们加上就行了。

