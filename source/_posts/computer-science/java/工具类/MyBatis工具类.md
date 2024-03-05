---
title: MyBatis工具类
categories:
   - [计算机学科,java,工具类]
tags:
   - 计算机学科
   - java
   - 工具类
---

```java
package com.dkx.mybatis.util;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.InputStream;

/**
 * @author Dkx
 * @version 1.0
 * @2023/2/1414:20
 * @function
 * @comment
 * sqlsessionfactory --> sqlsession
 */
@SuppressWarnings("all")
public class MybatisUtils {
    private static SqlSessionFactory sqlsessionfactory;
    static{
        try{
//            使用MyBatis第一步,获取SqlSessionFactory对象
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlsessionfactory = new SqlSessionFactoryBuilder().build(inputStream);
        }catch(Exception e){}
    }
//    既然有了sqlSessionFactory,顾名思义,我们就可以从中获得SqlSession的实例了
//    SqlSession完全包含了面向数据库执行,SQL命令所需的所有方法
    public static SqlSession getSqlSession(){
        return sqlsessionfactory.openSession();
    }
}
```