---
title: Mapper.xml
categories:
  - [计算机学科,database,mybatis]
tags:
  - 计算机学科
  - database
  - mybatis
---

Mapper.xml

映射的SQL语句

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace绑定一个对应的Dao(Mapper)接口-->
<mapper namespace="org.mybatis.example.BlogMapper">
<!--SQL语句头标签-->
<!--select:查询语句,id:对应接口的方法名,parameterType:参数类型,resultType:返回值类型-->
  <select id="selectBlog" parameterType="map" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

建议直接设置为模板
