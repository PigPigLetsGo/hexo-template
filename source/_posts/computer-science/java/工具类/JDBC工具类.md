---
title: JDBC工具类
categories:
   - [计算机学科,java,工具类]
tags:
   - 计算机学科
   - java
   - 工具类
---

```java
package com.dkx.jdbcutils;

import java.io.FileInputStream;
import java.sql.*;
import java.util.Properties;

public class Jdbcutils {
    //定义相关的属性（4个）因为只需要一份，因此，为static
    private static String driver;
    private static String url;
    private static String user;
    private static String password;

    static {
        try{
        Properties properties = new Properties();
        properties.load(new FileInputStream("src//re.Properties"));
        //读取相关的属性
             driver = properties.getProperty("driver");
             url = properties.getProperty("url");
             user = properties.getProperty("user");
             password = properties.getProperty("password");
             //注册驱动
             Class.forName("driver");
        }catch(Exception e){
            //在实际开发中,可以将编译异常转成运行时异常
            //调用者可以选择捕获该异常,也可以选择默认处理,比较方便
            throw new RuntimeException(e);
        }
    }
    //连接数据库,返回Connection
    public static Connection getConnection () {
        try{
            return DriverManager.getConnection(url,user,password);
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }
    //关闭相关资源
    /*
    1.ResultSet 结果集
    2.Statement 或者 PreparedStatement
    3.Connection
    4.如果需要关闭资源,就传入对象,否则传入null
     */
    public static void close (ResultSet set, PreparedStatement preparestatement, Connection connection) {
        //判断是否为null
        try{
            if(set != null){
                set.close();
            }
            if(preparestatement != null){
                preparestatement.close();
            }
            if(connection != null){
                connection.close();
            }
        }catch(Exception e){throw new RuntimeException(e);}
    }
}
```