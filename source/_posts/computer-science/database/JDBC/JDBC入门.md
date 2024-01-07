---
title: JDBC入门
categories:
    - [计算机学科,database,JDBC]
tags:
    - 计算机学科
    - database
    - JDBC
    - 基础
---

将mysql-connector-java-8.0.28-1.el7.noarch.rpm传输到Linux后,使用命令解压

```bash
rpm -zxvf mysql-connector-java-8.0.28-1.el7.noarch.rpm
```

一般解压后在路径: usr/java/mysql-connection-java.jar中找到jar包然后进行下面的操作

1.  创建libs文件夹

![image-20240105144307979](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051443047.png)

2.  找到mysql-connector.jar 驱动,将其copy到libs文件夹中

    ![image-20240105144318634](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051443505.png)

拷贝到libs文件夹中,点击ok

![image-20240105144329724](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051443763.png)

3.  右击libs文件夹选择Add as Library...

![image-20221024194150947](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051442929.png)

4.  查看除了项目位置其它默认不用更改查看完毕后点击ok

![image-20221024194254614](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051442703.png)

5.  代码

```java
package com.ar;

import com.mysql.jdbc.Driver;

import java.sql.Connection;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class Jdbc_test {
    public static void main(String[]args) throws SQLException {
        //一,注册驱动
        Driver driver = new Driver();
        //二,得到连接
        //解读:jdbc:mysql:// 规定好表示协议,通过jdbc连接MySQL
        //(1)local表示主机地址,可以是ip地址
        //(2)3306端口号,如果远程端口号发生了变化,那么这个端口号也需要改变
        //(3)dkx表示要连接到的数据库
        //MySQL的连接本质就是sokcet连接
        String url = "jdbc:mysql://localhost:3306/Demo";
        //将用户名和密码放入到Properties配置文件中
        Properties properties = new Properties();
        //user和password是规定好的,第二个值跟据实际情况
        //设置用户
        properties.setProperty("user","root");
        //设置密码
        properties.setProperty("password","dkx");
        //跟据给定的url连接数据库
        Connection connect = driver.connect(url, properties);
        //三,执行sql语句
        String sql = "insert into Demo values (null,'刘德华','男','1970-12-11','110')";
        //statement 用于执行静态SQL语句并返回其生成对象
        Statement statement = connect.createStatement();
        //返回是否添加成功结果
        int i = statement.executeUpdate(sql);
        System.out.println(i > 0 ? "成功" : "失败");
        //四,关闭连接资源
        statement.close();
        connect.close();
    }
}
```

<font color=red>**示意图 **</font> 

![image-20221024194843385](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051442727.png)

执行完成后

![image-20221024194347938](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051442628.png)

查看一下MySQL的变化

![image-20221024194417196](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051442386.png)

完成 !