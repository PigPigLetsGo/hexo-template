---
title: 'NoClassDefFoundError: javax/xml/bind/DatatypeConverter'
categories:
    - [问题总汇]
tags:
    - 问题总汇
    - springboot
    - Jwt
    - jdk版本问题
---

# NoClassDefFoundError: javax/xml/bind/DatatypeConverter

问题描述：

>  错误 `NoClassDefFoundError: javax/xml/bind/DatatypeConverter` 通常是因为 `javax.xml.bind` 包不在 Java 11 及更高版本中可用。这个包包含了 `DatatypeConverter` 类。如果你正在使用 Java 11 或更高版本，并且你的代码中使用了 `DatatypeConverter`，你可能会遇到这个错误。你可以考虑使用其他类，如 `java.util.Base64`，来替换 `DatatypeConverter` 的功能。

如果不希望降低jdk版本而解决这个问题可以参考如下：

```xml
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.0</version>
</dependency>
<dependency>
    <groupId>com.sun.xml.bind</groupId>
    <artifactId>jaxb-impl</artifactId>
    <version>2.3.0</version>
</dependency>
<dependency>
    <groupId>com.sun.xml.bind</groupId>
    <artifactId>jaxb-core</artifactId>
    <version>2.3.0</version>
</dependency>
<dependency>
    <groupId>javax.activation</groupId>
    <artifactId>activation</artifactId>
    <version>1.1.1</version>
</dependency>
```

为什么添加该依赖就不会报错了呢？是什么原理？

答：

>  当你在项目中添加这些依赖时，你其实是在你的项目中引入了 `javax.xml.bind.DatatypeConverter` 类所在的库。这样，当你的代码试图访问 `DatatypeConverter` 类时，JVM 就可以在你的项目的类路径中找到它，因此就不会抛出 `NoClassDefFoundError` 错误了。
>
>  具体来说，`javax.xml.bind.DatatypeConverter` 类是在 Java 8 及更早版本的 JDK 中可用的，但在 Java 11 及更高版本中被移除了。这是因为 Java 9 引入了模块系统，一些以前的 Java EE 和 CORBA 模块（包括 `javax.xml.bind`）被标记为 “deprecated” 并在 Java 11 中被移除。
>
>  当你添加了上述的 Maven 依赖后，这些被移除的类会被添加回你的项目中，因此你的代码可以在 Java 11 或更高版本中正常运行。这就是为什么添加这些依赖可以解决 `NoClassDefFoundError: javax/xml/bind/DatatypeConverter` 错误的原理。。