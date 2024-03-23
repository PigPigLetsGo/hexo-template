---
title: No-DataSource-specified
categories:
    - [问题总汇]
tags:
    - JdbcTemplate
    - 问题总汇
---

问题描述:

```
JdbcTemplate在Dao中的使用以及No DataSource specified问题的出现和解决。
```

查看在定义对象时是否写对了格式

正确格式如下:

```java
private static JdbcTemplate jdbctemplate = new JdbcTemplate(JDBCUtils.getDataSource());
```
