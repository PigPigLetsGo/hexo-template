---
title: org-springframework-dao-support-DaoSupport
categories:
    - [问题总汇]
tags:
    - 问题总汇
    - SpringBoot
    - Mybatis
---

问题描述

```
Spring整合Mybatis时，报错 java.lang.ClassNotFoundException: org.springframework.dao.support.DaoSupport
```

报错原因如下:

- 缺少了该依赖的配置

```xml
            <dependency>
                      <groupId>org.springframework</groupId>
                      <artifactId>spring-jdbc</artifactId>
                      <version>5.2.10.RELEASE</version>
            </dependency>
```

