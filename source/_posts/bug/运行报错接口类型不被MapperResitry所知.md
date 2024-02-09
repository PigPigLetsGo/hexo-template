---
title: MyBatis-运行报错接口类型不被MapperResitry所知
categories:
    - [问题总汇]
tags:
    - mybatis
    - 问题总汇
---

问题描述:

```
org.apache.ibatis.binding.BindingException: Type interface com.dkx.mybatis01.dao.UserDao is not known to the MapperRegistry.

```

报错原因:缺少了mappers配置注册标签

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="mysql:jdbc//localhost:3306/mybatis?characterEncoding=utf-8&amp;serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="dkx"/>
            </dataSource>
        </environment>
    </environments>
<!--    每一个Mapper.XML都需要在MyBatis核心配置文件中注册! -->
    <mappers>
        <mapper resource="com/dkx/mybatis01/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

