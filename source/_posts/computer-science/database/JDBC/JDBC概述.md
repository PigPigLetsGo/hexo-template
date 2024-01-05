---
title: JDBC概述
categories:
    - [计算机学科,databse,JDBC]
tags:
    - 计算机学科
    - databse
    - JDBC
    - 介绍
---

## JDBC概述

>  1.  JDBC为访问不同的数据库提供了统一的接口,为使用者屏蔽了细节问题
>  2.  Java程序员使用JDBC,可以连接任何提供了JDBC驱动程序的数据库系统,从而完成对数据库的各种操作

示意图:

![image-20240105143447191](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051434272.png)

## JDBC带来的好处:

示意图:

![image-20240105143459996](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051435106.png)

>  说明:  JDBC是Java提供一套用于数据库操作的接口API,Java程序员需要面向这套接口编程即可,不同的数据库厂商,需要针对这套接口,提供不同实现

## JDBC  API概述:

示意图:

![image-20240105143511737](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051440767.png)

>  JDBC   API是一系列的接口,它统一规范了应用程序与数据库的链接,执行SQL语句,并得到返回结果等各类操作,相关类和接口在java.sql与javax.sql包中

### JDBC链接数据库方式(路径以Linux为例)

| 方法              | 功能                                                         |
| ----------------- | ------------------------------------------------------------ |
| Driver            | 注册驱动                                                     |
| Connection        | 链接数据库                                                   |
| ResultSet         | 通过next将指针向下移动,通过previous将指针向上移动<br>类似于迭代器,用于访问循环的数据,比如DQL |
| PreparedStatement | 发送SQL到数据库,调用对应的get方法获取结果                    |
| Class.forName     | Java反射机制,记载类,返回Class类型对象                        |
| DriverManager     | 替代Driver进行统一管理,可以调用registerDriver将注册驱动的对象<br>传入构造器中,使期为自身<br>通过调用getConnection获取与MySQL连接 |
| Properties        | 配置文件类,用于读取配置文件写出配置文件                      |

**Java连接数据库**

![image-20240105143526160](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051435224.png)

## 1.传统方式

```java
 @Test
    public void test () throws SQLException {
        //方式一
        //注册驱动
        Driver driver = new Driver();
        //编写连接数据库的url
        String url = "jdbc:mysql://localhost:3306/Demo";
        //创建Properties配置文件类
        Properties properties = new Properties();
        //设置配置文件信息
        properties.setProperty("user","root");
        properties.setProperty("password","dkx");
        //创建Connection通过Driver对象调用connect添加url跟数据库的user和password连接数据库
        Connection connection = driver.connect(url,properties);
        //编写一条sql语句
        String sql = "update `Demo` set name = '刘桑' where id = 28";
        //将sql语句发送给MySQL数据库并返回其对象
        Statement statement = connection.createStatement();
        //通过返回对象调用executeUpdate返回一个int类型的值
        int i = statement.executeUpdate(sql);
        //判断这个值如果大于1则说明执行成功,否则失败
        System.out.println(i > 0 ? "连接成功" : "连接失败");
        //关闭源,释放资源
        connection.close();
        statement.close();
    }
```

## 2.使用反射机制

利用反射机制来实例化Driver对象从而注册驱动,然后向下转型即可调用Driver方法

![image-20240105143538463](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051435545.png)

```java
@Test
    public void testone () throws ClassNotFoundException, InstantiationException, IllegalAccessException, SQLException {
        Class cls = Class.forName("com.mysql.jdbc.Driver");
        Driver driver = (Driver)cls.newInstance();
        String url = "jdbc:mysql://localhost:3306/Demo";
        Properties properties = new Properties();
        properties.setProperty("user","root");
        properties.setProperty("password","dkx");
        Connection connection = driver.connect(url,properties);
        System.out.println("Driver:"+connection);
    }
```

#### 区别:

-  Driver driver = new Driver(  ); 直接使用import.java.sql.Connection;属于静态加载灵活性差依赖强
-  Class cls = Class.forName("com.mysql.jdbc.Driver"); 属于动态加载,灵活性强

## 3. 使用DriverManager替代Driver进行统一管理

```java
@Test
    public void testtwo () throws ClassNotFoundException, InstantiationException, IllegalAccessException, SQLException {
        //Java反射机制实例化Driver类
        Class cls = Class.forName("com.mysql.jdbc.Driver");
        //向下转型
        Driver driver = (Driver)cls.newInstance();
        String url = "jdbc:mysql://localhost:3306/Demo";
        String user = "root";
        String password = "dkx";
        //使用DriverManager注册给定的驱动程序,调用resterDriver使其为自身
        DriverManager.registerDriver(driver);
        //通过Connection提供url,user,password连接数据库
        Connection connection = DriverManager.getConnection(url,user,password);
        System.out.println("Driver:"+connection);
    }
```

## 4.使用反射机制加载Driver类

```java
 @Test
    public void testsan () throws ClassNotFoundException, SQLException {
        //通过反射机制加载Driver类
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/Demo";
        String user = "root";
        String password = "dkx";
        //通过DriverManager调用getConnection方法传入数据库的url,user,password连接到数据库
        Connection connection = DriverManager.getConnection(url,user,password);
        System.out.println(connection);
    }
```

## 5.在第四个方式上进行优化

```java
@Test
    public void testsi () throws ClassNotFoundException, IOException, SQLException {
        //实例化配置文件对象
        Properties properties = new Properties();
        //向配置文件中添加信息
        properties.setProperty("driver","com.mysql.jdbc.Driver");
        properties.setProperty("url","jdbc:mysql://localhost:3306/Demo");
        properties.setProperty("user","root");
        properties.setProperty("password","dkx");
        //通过调用store方法新建输出流将配置文件写出到指定位置中,第二个参数为注释
        properties.store(new FileOutputStream("src//re.Properties"),"LinuxProperties");
        //获取配置文件中的信息
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        String password = properties.getProperty("password");
        String user = properties.getProperty("user");
        //通过反射机制加载Driver类,虽然自动加载但是还是建议写上
        Class.forName(driver);
        //实例化Connection类通过DriverManager调用getConnection方法传入连接数据库的数据
        Connection connection = DriverManager.getConnection(url,user,password);
        //编写一条SQL语句
        String sql = "select * from Demo";
        //向数据库发送SQL语句,并返回其对象
        Statement statement = connection.createStatement();
        //通过Statement对象调用executeQuery执行SQL DQL语句并返回给ResultSet对象
        ResultSet resultset = statement.executeQuery(sql);
        //因为不知道循环多少次,使用while循环来处理resultset.next()每次执行指向下一个
        //当没有可查询列后返回false即结束循环
        while(resultset.next()){
            //id在第一列中为int类型
            int id = resultset.getInt(1);
            String name = resultset.getString(2);
            String sex = resultset.getString(3);
            Date date = resultset.getDate(4);
            String dian = resultset.getString(5);
            System.out.println(id+"\t"+name+"\t"+sex+"\t"+date+"\t"+dian);
        }
        //关闭源,释放资源
        connection.close();
        statement.close();
    }
```

-  **提示:**

-  1.  mysql驱动5.1.6可以无需Class.forName("com.mysql.jdbc.Driver");

   2.  从jdk1.5以后使用了jdbc4,不再需要显示调用Class.forName(  );注册驱动而是自动调用驱动jar包下META-INF\serverces\java.sql.Driver 文件中的类名称去注册

       ![image-20240105143556571](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051435643.png)

   3.  建议加上Class.forName("com.mysql.jdbc.Driver");更加明确

   Debug代码查看ResultSet对象结构:
   
   ![image-20240105143624437](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051436500.png)

## Statement

1.  Statement对象用于执行静态SQL语句并返回其生成的结果对象
2.  在连接建立后,需要对数据库进行访问,执行命令或是SQL语句,可以通过

-  Statement (存在SQL注入)
-  PreparedStatement(预处理)
-  CallableStatement(存储过程)

3.  Statement对象执行SQL语句,存在SQL注入风险
4.  SQL注入是利用某些系统没有对用户输入的数据进行充分的检查,而在用户输入数据中注入非法的SQL语句段或命令,恶意攻击数据库
5.  要防范SQL注入,只要用PreparedStatement**(从Statement扩展而来)**取代Statement就可以了

### 演示SQL注入:

表结构和数据:

```sql
-- auto-generated definition
create table admin
(
    name varchar(64) default '' not null,
    pwd  varchar(64) default '' not null
)
    collate = utf8_bin;
```

-  操作:将用户名和密码输入为 name = 1' or password = or '1' = '1  如果不成功建议按顺序打'->1->'

```sql
# 演示SQL注入
# 创建一张表
create table admin (
    `name` varchar(64) not null default '',
    `pwd` varchar(64) not null default ''
)character set utf8 collate utf8_bin;
# 查找某个管理员是否存在,输入用户名为 '1' or' 输入密码为'or = '1' = '1'
select * from admin
where name = '1' or' and pwd = 'or '1' = '1';
```

-  Java程序中演示

```java
Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名与密码");
        System.out.print("请输入用户名:");
        String zhang = sc.nextLine();
        System.out.print("请输入密码:");
        String mima = sc.nextLine();
        Properties properties = new Properties();
        properties.load(new FileInputStream("src//re.Properties"));
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        Class.forName(driver);
        String sql = "select * from admin where name = '"+zhang+"'and pwd = '"+mima+"'";
        Connection connection = DriverManager.getConnection(url,user,password);
        Statement statement = connection.createStatement();
        ResultSet resultset = statement.executeQuery(sql);
        if(resultset.next()){
            System.out.println("登入成功");
        }else{
            System.out.println("登入失败");
        }
        connection.close();
        statement.close();
```

执行输入密码演示:

输入正确密码

![image-20240105143641132](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051436182.png)

输入错误密码或者是不存在的用户名

![image-20240105143654524](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051436670.png)

使用万能密码演示:

![image-20240105143704277](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051440719.png)

## PreparedStatement

1.  PreparedStatement执行的SQL语句中的参数用问号( ? )来表示,调用PreparedStatement对象的setXxx(   );方法来设置这些参数,setXxx(   );方法有两个参数,第一个参数是要设置的SQL语句中的参数的索引(从1开始),第二个是设置的SQL语句中的参数的值
2.  调用executeQuery(  );执行DQL语句,返回ResultSet对象
3.  调用executeUpdate(  );执行DML语句,更新,包括增,删.改

```sql
String sql = "select count(*) from admin where name = ? and password = ?";
```

### PreparedStatement好处

1.  不再使用+拼接SQL语句,减少语句错误
2.  有效的解决了SQL注入问题
3.  大大减少了编译次数,效率较高

代码:登入演示:(这个程序在使用万能密码的时候就不管用了)

演示是否可以使用 `admin name 1' or admin password or '1' = '1`

```java
package com.dkx.xer;

import java.io.FileInputStream;
import java.io.IOException;
import java.sql.*;
import java.util.Properties;
import java.util.Scanner;
@SuppressWarnings({"all"})
public class My_Connection {
    public static void main (String[]args) throws IOException, ClassNotFoundException, SQLException {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入内容");
        System.out.print("请输入用户名:");
        String admin_name = sc.nextLine();
        System.out.print("请输入密码:");
        String admin_neir = sc.nextLine();
        Properties properties = new Properties();
        properties.load(new FileInputStream("src//re.Properties"));
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        Class.forName(driver);
        //Connection连接数据库
        Connection connection = DriverManager.getConnection(url,user,password);
        //得到PreparedStatement
        //1.编写SQL语句 ？ 号相当于是一个占位符
        String sql = "select * from admin where name = ? and pwd = ?";
        //2.PreparedStatement对象实现了PreparedStatement接口的实现类对象
        PreparedStatement preparedstatement = connection.prepareStatement(sql);
        //3.通过PreparedStatement给 ? 赋值,进行了预处理
        preparedstatement.setString(1,admin_name);
        preparedstatement.setString(2,admin_neir);
        //4.执行select   DQL语句使用executeQuery();
        //如果执行的是DML (update,insert,delete) 则使用executeUpdate();
        //这里的参数中不能给sql,这里赋值sql对象的话,相当于将带有?号的语句给了这个对象就会发生报错
        ResultSet resultset = preparedstatement.executeQuery();
        if(resultset.next()){
            System.out.println("登入成功");
        }else{
            System.out.println("抱歉，登入失败！");
        }
        resultset.close();
        preparedstatement.close();
        connection.close();
    }
}
```

## **最常用** jdbc API

-  DriverManager驱动管理类 ---> getConnection(url,user,password)获取到链接

---



-  Connection接口 ---> createStatement创建Statement对象

​		---> preparedStatement(sql)生成预处理对象

---



-  Statement接口 ---> executeUpdate(sql)执行DML语句,返回影响的行数

​		---> executeQuery(sql)执行查询,返回ResultSet对象

​		---> execute(sql)执行任意的sql,返回布尔值一般用于创建表

---



-  PreparedStatement接口 ---> executeUpdate([这里面不能写sql]);执行DML(增删改查)

​		---> executeQuery([不能写sql对象]);执行查询,返回ResultSet   DQL

​		---> execute([不能写sql对象]); 执行任何sql,返回布尔值一般用于创建表

​		---> setXxx(占位符索引,占位符的值);解决SQL注入[Xxx为对应数据类型]

​		---> setObject(占位符索引,占位符的值);

---



-  ResultSet(结果集) ---> next( );向下移动一行,同时如果没有下一行,返回false

​		---> previous(  );向上移动一行

​		---> getXxx(列的索引  |  列名);返回对应列的值,接口类型是Xxx

​		---> getObject(列的索引   |   列名);返回对应列的值,接收类型为Object类型

## 封装JDBCUtils

示意图:

![image-20240105143722652](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051440736.png)

-  构建JDBCUtils工具类

思路:

>  1.  编写属性(4个)分别为(driver,url,user,password)并且声明字段为私有,静态类型成员变量
>  2.  编写一个static静态代码块,方法体中编写:Properties对象,用于读取相关的属性,(driver,url,user,password),将抛出的异常编写为运行时异常,这样,即可以选择处理也可以选择默认处理,比较方便
>  3.  编写连接数据库的Connection方法并且这个方法为返回值类型返回Connection类,命名为getConnection,方法体中编写return返回DriverManager.getConnection(url,user,password);抛出异常也为运行时异常
>  4.  关闭相关资源,编写方法四个参数分为在调用这个方法时关闭所需要关闭的流,如果有不需要关闭的则置为null即可

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

-  编写执行SQL类

思路:

>  编写一个方法,方法体中编写
>
>  1.  得到连接创建Connection对象
>  2.  编写一条SQL执行语句
>  3.  编写PreparedStatement对象,执行SQL语句
>  4.  得到PreparedStatement对象返回的结果
>  5.  最后通过JDBCUtils类调用close方法来传入需要关闭的流没有则置为null

```java
package com.dkx.jdbcutils;

import java.sql.Connection;
import java.sql.PreparedStatement;

public class Jdbcutils_use {
    public static void main(String[]args){
        //实例化对象
        var ar = new Jdbcutils_use();
        //对象调用方法执行操作数据库
        ar.test_connector();
    }
    public void test_connector () {
        //1.得到连接
        Connection connection = null;
        //2.编写一条SQL语句
        String sql = "insert into admin values (?,?)";
        //3.创建PreparedStatement对象,扩大PrearedStatement的作用域
        PreparedStatement preparedstatement = null;
        try{
            connection = Jdbcutils.getConnection();
            preparedstatement = connection.prepareStatement(sql);
            //给占位符进行赋值
            preparedstatement.setString(1,"张三");
            preparedstatement.setString(2,"123");
            int i = preparedstatement.executeUpdate();
            if(i > 0){
                System.out.println("添加成功~");
            }else{
                System.out.println("添加失败!");
            }
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            //关闭资源
            Jdbcutils.close(null,preparedstatement,connection);
        }
    }
}
```

总结:

>  -  编写两个类一个为工具类,一个为执行SQL语句的类
>
>  -  工具类中定义静态成员变量,并私有化防止外部程序破坏,编写静态代码块用于在类被加载时加载一次,方法体中编写Properties读取配置文件中的相关信息,并返回给私有静态成员变量
>  -  工具类中编写,读取配置文件类,并在静态代码块中注册驱动,编写连接数据库的方法,通过类名调用返回连接数据库的信息,编写关闭相关资源的方法,都为静态方法方便使用类名直接调用而不实例化类
>  -  执行SQL语句类: 编写一个方法,方法体中创建Connection对象由JDBCUtils工具类调用getConnection获取链接数据库的信息,编写PreparedStatemen执行SQL,PreparedStatement对象调用setXxx对占位符进行赋值,最后fianlly代码块中JDBCUtils工具类调用close方法传入需要关闭的相关资源

## 事务

**基本介绍:**

1.  JDBC程序中当一个Connection对象创建时,默认情况下是自动提交事务的:每次执行一个SQL语句时,如果执行成功,就会像数据库自动提交,而不能回滚
2.  JDBC程序中为了让多个SQL语句作为一个整体执行,需要使用事务
3.  调用Connection的setAutoCommit(false)可以取消自动提交事务
4.  在所有的SQL语句都成功执行后,调用commit(  );方法提交事务
5.  在其中某个操作失败或出现异常时,调用rollback(   );方法回滚事务

| 方法                           | 功能声明                                                     |
| ------------------------------ | ------------------------------------------------------------ |
| setAutoCommit(false)           | 开启或禁用事务的自动提交事务                                 |
| commit(  );                    | 使自上次提交/回滚以来所做的所有更改成为永久更改<br>并释放此Connection对象当前持有的所有数据库锁<br>仅在禁用自动提交模式时才应使用此方法 |
| rollback(  );                  | 撤销当前事务中所做的所有更改,并释放此Connection<br>对象当前持有的所有数据库锁,仅在禁用自动提交模式时才<br>应使用此方法<br>异常SQLException :如果发生数据库访问错误,则在参与分布式<br>事务时调用此方法,此方法在已关闭的链接上调用,或者此<br>Connection对象处于自动提交模式 |
| rollback(Savepoint savepoint); | 取消在设置给定的Savepoint对象后所做的所有更改<br>仅在禁用自动提交事务时才应使用此方法<br>异常SQLException :如果发生数据库访问错误,则在参与分布式事务时调用此方法,在关闭的链接上调用此方法,Savepoint对象不再<br>有效,或者此Connection对象当前处于自动提交模式<br>SQLFeatureNotSupporedException - 如果JDBC驱动程序不<br>支持此方法 |

```java
package com.transaction;

import com.music.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.util.Scanner;

public class Salary_test {
    public static void main(String[]args) {
        var ar = new Salary_test();
        ar.test();
    }
    public void test () {
        Scanner sc = new Scanner(System.in);
        System.out.print("转账金额:");
        Double salary = sc.nextDouble();
        Scanner sc1 = new Scanner(System.in);
        System.out.print("转账者:");
        String name = sc1.nextLine();
        System.out.print("收账者:");
        String name1 = sc1.nextLine();
        Connection connection = null;
        PreparedStatement preparedstatement = null;
        String sql = "update `salary` set salary = salary - ? where name = ?";
        String sql1 = "update `salary` set salary = salary + ? where name = ?";
        try{
            connection = JDBCUtils.getConnection();//在默认情况下,Connection是默认自动提交的
            //将connection设置为不自动提交事务
            connection.setAutoCommit(false);//开启了事务
            preparedstatement = connection.prepareStatement(sql);
            preparedstatement.setDouble(1,salary);
            preparedstatement.setString(2,name);
            preparedstatement.executeUpdate();
            //程序报错...
            System.out.println(1/0);
            preparedstatement = connection.prepareStatement(sql1);
            preparedstatement.setDouble(1,salary);
            preparedstatement.setString(2,name1);
            int i = preparedstatement.executeUpdate();
            connection.commit();
            System.out.println(i > 0? "转账成功" : "转账失败");
        }catch(Exception e){
            //这里我们可以进行回滚,即撤销执行的SQL操作
            //默认回滚到事务开始的状态
            System.out.println("执行操作有错误,事务已经回滚");
            try{
                connection.rollback();
            }catch(Exception er){
                er.printStackTrace();
            }
            e.printStackTrace();
        }finally{
            JDBCUtils.close(connection,preparedstatement,null);
        }
    }
}
```

### 批处理:

**基本介绍:**

1.  当需要成批插入或者更新新纪录时,可以采用Java的批量更新机制,这一机制允许多条SQL语句一次性提交给数据库批量处理,通常情况下单独提交处理更有效

2.  JDBC的批量处理语句包括下面方法:

    <font color = red>`addBatch(  );` 添加需要批量处理的语句</font>

    <font color = red>`executeBatch(  );` 执行批量处理语句</font>

    <font color = red>`clearBatch(  );` 清空批处理包的语句</font>

3.  JDBC连接MySQL时,如果要使用批处理功能,请再url中加参数 ?rewriteBatchedStatements=true

    批处理必须指定这个参数否则将会无效

4.  批处理往往和PreparedStatement一起搭配使用,可以既减少编译次数,又减少运行次数,效率大大提高

代码演示:

时间对比:

传统方式 : 执行用时:8423

批量处理方式 : 执行用时:954

```java
public void test () {
        Connection connection = null;
        PreparedStatement preparedstatement = null;
        String sql = "insert into `salary` values (?,?)";
        long start = System.currentTimeMillis();
        try{
            connection = JDBCUtils.getConnection();
            preparedstatement = connection.prepareStatement(sql);
            System.out.println("添加中...");
            for(int i = 0;i < 5000;i ++){
                preparedstatement.setString(1,"张三");
                preparedstatement.setDouble(2,1+i);
                preparedstatement.executeUpdate();
            }
            System.out.println("添加完毕");
            long pause = System.currentTimeMillis();
            System.out.println("执行用时:"+(pause - start));//执行用时:8423
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            JDBCUtils.close(connection,preparedstatement,null);
        }
    }
    public void test1 () {
        Connection connection = null;
        PreparedStatement preparedstatement = null;
        String sql = "insert into `salary` values (?,?)";
        long start = System.currentTimeMillis();
        try{
            connection = JDBCUtils.getConnection();
            preparedstatement = connection.prepareStatement(sql);
            System.out.println("添加中...");
            for(int i = 0;i < 5000;i ++){
                preparedstatement.setString(1,"张三");
                preparedstatement.setDouble(2,1+i);
                preparedstatement.addBatch();
                if(i == 4999){
                    preparedstatement.executeBatch();
                    preparedstatement.clearBatch();
                }
            }
            System.out.println("添加完毕");
            long pause = System.currentTimeMillis();
            System.out.println("执行用时:"+(pause - start));//执行用时:954
        }catch(Exception e){
            e.printStackTrace();
        }
    }
```

## 数据库连接池

传统获取Connection问题分析

1.  传统的JDBC数据库连接使用DriverManager来获取,每次向数据库建立连接的时候都要将Connection加载到内存中,再验证<font color = blue>**IP地址**</font>,<font color = blue>**用户名**</font>和<font color = blue>**密码**</font>,(0.05s~1sTime),需要数据库连接的时候,就向数据库要求一个,频繁的进行数据库连接操作将占用很多的系统资源,容易造成服务器崩溃
2.  每一次数据库连接,使用完后都得断开,如果程序出现异常而未能关闭,将导致数据库<font color =blue>**内存泄露**</font>,最终将导致重启数据库
3.  传统获取连接的方式,不能控制创建的连接数量,如连接过多,也可能导致<font color =blue>内**存泄露**</font>,MySQL崩溃
4.  解决传统开发中的数据库连接问题,可以采用<font color = red>**数据库连接池技术(connection pool)**</font>

****



![image-20221027183545806](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051440613.png)

### 数据库连接池基本介绍

1.  预先在缓冲池中存放入一定数量的连接,当需要建立数据库连接时,只需从"缓冲池"中取出一个,使用完毕之后再放回去

2.  数据库连接池负责分配,管理和释放数据库链接,它允许应用程序<font color = red>**重复使用**</font>一个现有的数据库连接,而不是重新建立一个

3.  当应用程序向连接池请求的连接数超过最大连接数量时,这些请求将被加入到等待队列中

    ![image-20240105143808707](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051438797.png)

### 数据库连接池种类

1.  JDBC的数据库连接池使用javax.sql.DataSource来表示,DataSource只是一个接口,该接口通常由第三方提供实现
2.  <font color = green>C3P0</font>数据库连接池,速度相对较慢,稳定性不错 (hibernate,spring)
3.  DBCP数据库连接池,速度相对C3P0较快,但不稳定
4.  Proxool数据库连接池,有监控连接池状态的功能,稳定性较iCP差一点
5.  BoneCP数据库连接池,速度快
6.  <font color = grenn>Druid (德鲁伊)</font> 是阿里提供的数据库连接池,集DBCP,C3P0,Proxool优点于一身的数据库连接池

### C3P0使用:

1.  将c3p0-0.9.5.5.bin.zip包解压,使用指令

```bash
unzip c3p0-0.9.5.5.bin.zip
```

2.  cd进入解压后的zip包找到lib包进入找到c3p0-0.9.5.5.jar的包和mchange-commons-java-0.2.19.jar包

    ![image-20240105143827227](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051438556.png)

3.  copy将这两个包都paste到IDEA的libs文件夹中

    ![image-20240105143840760](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051438827.png)

4.  再将libs文件夹导入到项目中

右键文件夹点击Add as Library....

![image-20240105143907300](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051441766.png)

确定好要导入的项目后点击OK

![image-20240105143916710](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051441143.png)

c3p0-0.9.5.5-config.xml 文件配置信息

```xml
<c3p0-config>
    <!-- 使用默认的配置读取连接池对象 -->
    <named-config name="dkx_123">
        <!--  连接参数 -->
        <property name="driverClass">com.mysql.jdbc.Driver</property><!-- 这是你的数据库驱动 -->
        <!-- 这是你的数据库,当然如果是本地连接也可把localhost:3303给删掉 -->
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/Demo?characterEncoding=utf-8&amp;serverTimezone=UTC</property>
        <!-- 这是你的数据库账号 密码 -->
        <property name="user">root</property>
        <property name="password">root</property>

        <!-- 连接池参数 -->
        <property name="initialPoolSize">5</property><!-- 初始化连接数 -->
        <property name="maxPoolSize">10</property><!-- 最大连接数 -->
        <property name="checkoutTimeout">3000</property><!--超时时间      -->
    </named-config>
    <!-- 这是自定义的配置,如果想用这个配置        创建的时候这样写就好了    javax.sql.DataSource com  = new ComboPooledDataSource("outerc3p0");-->
    <!-- 这属性同上 -->
    <named-config name="otherc3p0">
        <!--  连接参数 -->
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/mysql</property>
        <property name="user">root</property>
        <property name="password"></property>

        <!-- 连接池参数 -->
        <property name="initialPoolSize">5</property>
        <property name="maxPoolSize">8</property>
        <property name="checkoutTimeout">1000</property>
    </named-config>
</c3p0-config>
```

-  代码:

```java
package com.testconnection;

import com.mchange.v2.c3p0.ComboPooledDataSource;

import java.beans.PropertyVetoException;
import java.io.FileInputStream;
import java.io.IOException;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;
@SuppressWarnings({"all"})
public class C3P0_test {
    public static void main(String[]args) throws PropertyVetoException, SQLException, IOException {
        //方式1:相关参数,在程序中指定user,url,password等
        ComboPooledDataSource combopooleddatasource = new ComboPooledDataSource();
        //通过配置文件re.Properties获取相关连接的信息
        Properties properties = new Properties();
        properties.load(new FileInputStream("src//re.Properties"));
        //读取相关的属性值
        String driver = properties.getProperty("driver");
        String url = properties.getProperty("url");
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        //给数据源ComboPooledDataSource设置相关参数
        //注意:连接管理是由ComboPooledDataSource来管理
        combopooleddatasource.setDriverClass(driver);
        combopooleddatasource.setJdbcUrl(url);
        combopooleddatasource.setUser(user);
        combopooleddatasource.setPassword(password);
        //设置初始化连接数
        combopooleddatasource.setInitialPoolSize(10);
        //设置最大连接数
        combopooleddatasource.setMaxPoolSize(50);
        //测试连接池的效率   测试对MySQL操作5000次
        System.out.println("连接成功...");
        long start = System.currentTimeMillis();
        for(int i = 0;i < 5000;i++){
            //这个方法就是从DataSource接口实现
            Connection connection = combopooleddatasource.getConnection();
            connection.close();
        }
        long end = System.currentTimeMillis();
        //C3P0 5000 次操作 执行耗时:1002
        System.out.println("C3P0 5000 次操作 执行耗时:"+(end - start));
    }
}
```

-  使用配置文件连接

```java
@Test
    public void test () throws SQLException {
        //创建ComboPooledDataSource对象 构造器中赋值连接的named-config : Name
        ComboPooledDataSource combopooleddatasource = new ComboPooledDataSource("dkx_123");
        System.out.println("连接成功");
        long start = System.currentTimeMillis();
        for(int i = 0;i < 5000;i++){
            //通过ComboPooledDataSource对象调用getConnection进行连接数据库
            Connection connection = combopooleddatasource.getConnection();
            //关闭数据库连接,释放资源
            connection.close();
        }
        long end = System.currentTimeMillis();
        //执行耗时:966
        System.out.println("执行耗时:"+(end - start));
    }
```

### Druid使用:

druid.Properties配置文件

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/Demo?rewriteBathedStatements=true
username=root
password=dkx
initialSize=10
minIdle=5
maxActive=20
maxWait=5000
```

如果使用上述的配置信息出现了无法创建连接则使用下面的配置信息

原因:MySQL版本问题

```properties
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/day14?characterEncoding=utf-8&serverTimezone=UTC
username=root
password=dkx
initialSize=10
minIdle=5
maxActive=20
maxWait=5000
```



-  Druid连接与C3P0连接测试
-  Druid: 执行耗时: 3629



```java
package com.testconnection;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.util.Properties;
@SuppressWarnings({"all"})
public class Druid {
    public static void main(String[]args) throws Exception {
        //1.加入Druid.jar包
        //2.加入配置文件druid.Propertis,将该文件拷贝项目的src目录
        //3.创建Properties对象,读取配置文件
        Properties properties = new Properties();
        properties.load(new FileInputStream("src//druid.Properties"));
        //4.创建一个指定参数的数据库连接池
        DataSource druiddatasourcefactory = DruidDataSourceFactory.createDataSource(properties);
        long start = System.currentTimeMillis();
        for(int i = 0;i < 5000000;i++){
            Connection connection = druiddatasourcefactory.getConnection();
            connection.close();
        }
        long end = System.currentTimeMillis();
        System.out.println("Druid执行耗时:"+(end - start));//Druid执行耗时:3629
        System.out.println("执行结束!");
    }
}
```

-  C3P0 执行耗时: 17491

```java
package com.testconnection;

import com.mchange.v2.c3p0.ComboPooledDataSource;

import java.sql.Connection;
import java.sql.SQLException;
@SuppressWarnings({"all"})
public class C3P0_test {
    public static void main(String[] args) throws SQLException {
        ComboPooledDataSource combopooleddatasource = new ComboPooledDataSource("dkx_123");
        System.out.println("连接成功");
        long start = System.currentTimeMillis();
        for (int i = 0; i < 5000000; i++) {
            //通过ComboPooledDataSource对象调用getConnection进行连接数据库
            Connection connection = combopooleddatasource.getConnection();
            //关闭数据库连接,释放资源
            connection.close();
        }
        long end = System.currentTimeMillis();
        //执行耗时:966
        System.out.println("执行耗时:" + (end - start));//执行耗时:17491
    }
}
```

## ResultSet复用,引出问题

1.  关闭Connection后,ResultSet结果集无法使用
2.  ResultSet不利于数据的管理

示意图:

![image-20240105143932367](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051441188.png)

#### 使用土办法完成封装来解决问题:

-  代码演示:

JDBCUtils_Druid工具类

```java
package com.druidtest;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Properties;

public class JDBCUtils_Druid {
    //        定义获取数据库连接对象
    private static DataSource druid;
    static {
        try{
            //创建Properties类读取配置文件信息
            Properties properties = new Properties();
            //读取指定位置的配置文件
            InputStream in = JDBCUtils_Druid.class.getClassLoader().getResourceAsStream("druid.Properties");
            properties.load(in);
            //将读取到配置文件的信息赋值到DataSource对象中
            druid = DruidDataSourceFactory.createDataSource(properties);
        }catch(Exception e){
            throw new RuntimeException();
        }
    }
    //定义连接方法返回Connection连接
    public static Connection getConnection () {
        try{
            return druid.getConnection();
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }
    //关闭连接,如果没有需要关闭则赋值为null
    public static void close(Connection connection, ResultSet resultset, PreparedStatement set){
        try{
            if(connection != null){
                connection.close();
            }
            if(resultset != null){
                resultset.close();
            }
            if(set != null){
                set.close();
            }
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

Actor封装ResultSet结果集,解决ResultSet复用问题,因为List和Connection没有任何关联,所以该集合可以复用,即使关闭Connection连接,也不会影响到 List

```java
package com.druidtest;

import java.sql.Date;

//Actor对象对应Demo表
public class Actor {//Javabean,DoJO,Domain对象
    //定义表中对应的字段信息
    private Integer id;
    private String name;
    private String sex;
    private Date borndata;
    private String phone;
    public Actor(){}//一定要给一个无参构造器[底层反射需要]
    //创建一个有参构造用于获取字段的信息
    public Actor(Integer id, String name, String sex, Date borndata, String phone) {
        this.id = id;
        this.name = name;
        this.sex = sex;
        this.borndata = borndata;
        this.phone = phone;
    }
    public Integer getId() {
        return id;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Date getBorndata() {
        return borndata;
    }
    public void setBorndata(Date borndata) {
        this.borndata = borndata;
    }
    public String getPhone() {
        return phone;
    }
    public void setPhone(String phone) {
        this.phone = phone;
    }
    //重写toString返回字符串类型的对象信息
    @Override
    public String toString() {
        return "牛马：{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                ", borndata=" + borndata +
                ", phone='" + phone + '\'' +
                '}';
    }
}
```

测试类

```java
package com.druidtest;

import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.LinkedList;
import java.util.List;

public class Druidtest_two {
    public static void main(String[]args){
        Connection connection = null;
        PreparedStatement statement = null;
        ResultSet resultset = null;
        //创建一个List集合对象用于存储ResultSet获取到的字段的信息,解决ResultSet复用问题
        List<Actor> list = new LinkedList<>();
        //编写一个SQL语句
        String sql = "select * from Demo";
        try{//try将可能存在的异常抛出
            //通过JDBCUtils_Druid类调用getConnecion方法获取连接返回Connection对象
            connection = JDBCUtils_Druid.getConnection();
            //创建PreparedStatement对象将SQL语句发送给数据库
            statement = connection.prepareStatement(sql);
            //通过PreparedStatement对象调用executeQuery来获取返回的结果
            resultset = statement.executeQuery();
            while(resultset.next()){//resultset调用next每次执行指针向下移动获取下一个信息,如果没有信息则返回false
                //通过调用getXxx获取对应的数据类型
                Integer id = resultset.getInt("id");
                String name = resultset.getString("name");
                String sex = resultset.getString("sex");
                Date date = resultset.getDate("borndata");
                String phone = resultset.getString("phone");
                //把得到的ResultSet的记录,封装到Actor对象,放入到List集合中
                list.add(new Actor(id,name,sex,date,phone));
            }
            for(Actor i:list){
                System.out.print(i);
                //通过封装可以完成随意取数据的效果
//                System.out.print(i.getId());
//                System.out.print(i.getName());
                System.out.println();
            }
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            //调用JDBCUtils_Druid的close方法传入需要关闭的连接对象,关闭流释放资源
            JDBCUtils_Druid.close(connection,resultset,statement);
        }
    }
}
```

通过封装可以完成随意取数据的效果,在取数据的时候调用Actor类中的get方法来获取字段的信息

下面介绍Apche-DBUtils工具类解决问题

## Apche-DBUtils

<font color = blue>**基本介绍**</font>

1.  commons-dbutils是Apche组织提供的一个开源JDBC工具类库,它是对JDBC的封装,使用dbutils能极大简化jdbc编码的工作量

<font size = 4,font color >**dbutils类**</font> 

1.  QueryRunner类 : 该类封装了SQL的执行,是线程安全的,可以实现增,删,改,查,批处理
2.  使用QueryRunner类实现查询
3.  ResultSetHandler接口 : 该接口用于处理 java.sql.ResultSet , 将数据按要求转换为另一种形式

<font size = 4,font color = blue>`ArrayHandler:`</font>把结果集的第一行数据转换成对象数组

<font size = 4,font color = blue>`ArrayListHandler:`</font>把结果集中的每一行数据都转成一个数组,再存放到List中

<font size = 4,font color = blue>`BeanHandler:`</font>将结果集中的第一行数据封装到一个对应的JavaBean实例中

<font size = 4,font color = blue>`BeanListHandler:`</font>将结果集中的每一行数据都封装到一个对应的JavaBean实例中,存放到List中

<font size = 4,font color = blue>`ColumnListHandler:`</font>将结果集中某一列的数据存放到List中

<font size = 4,font color = blue>`KeyeHandler(name):`</font>将结果集中的每行数据都封装到Map里,再把这些Map再存到一个Map里,其key为指定的key

<font size = 4,font color = blue>`MapHandler:`</font>将结果集中的第一行数据封装到一个Map里,Key是列名,Value就是对应的值

<font size = 4,font color = blue>`MapListHandler:`</font>将结果集中的每一行数据都封装到一个Map里,然后再存放到 List

**按照之前的方式将jar包导入到编译器的libs包中**

![image-20240105143950011](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051439095.png)

右键libs包点击Add as Library...

-  代码演示:

**JDBCUtils_Druid工具类** 

```java
package com.druidtest.fuxi;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Properties;
/*
这是一个JDBCUtils工具类
 */
public class JDBCUtils_Druid {
    private static DataSource druid;
    static{
        try{
            Properties properties = new Properties();
            properties.load(new FileInputStream("src//druid.Properties"));
            druid = DruidDataSourceFactory.createDataSource(properties);
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }
    public static Connection getConnection(){
        try{
            return druid.getConnection();
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }
    public static void close(Connection connection, ResultSet set, PreparedStatement set1){
        try{
            if(connection != null){
                connection.close();
            }
            if(set != null){
                set.close();
            }
            if(set1 != null){
                set1.close();
            }
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

**Actor实体类:**

```java
package com.druidtest;
import java.time.LocalDateTime;

//Actor对象对应Demo表
public class Actor {//Javabean,DoJO,Domain对象
    //定义表中对应的字段信息
    private Integer id;
    private String name;
    private String sex;
    private LocalDateTime borndata;
    private String phone;
    public Actor(){}//一定要给一个无参构造器[底层反射需要]
    //创建一个有参构造用于获取字段的信息
    public Actor(Integer id, String name, String sex,LocalDateTime borndata, String phone) {
        this.id = id;
        this.name = name;
        this.sex = sex;
        this.borndata = borndata;
        this.phone = phone;
    }
    public Integer getId() {
        return id;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public LocalDateTime getBorndata() {
        return borndata;
    }
    public void setBorndata(LocalDateTime borndata) {
        this.borndata = borndata;
    }
    public String getPhone() {
        return phone;
    }
    public void setPhone(String phone) {
        this.phone = phone;
    }
    //重写toString返回字符串类型的对象信息
    @Override
    public String toString() {
        return "牛马：{" +
                "牛马id=" + id +
                ", 牛马name='" + name + '\'' +
                ", 牛马sex='" + sex + '\'' +
                ", 牛马borndata=" + borndata +
                ", 牛马phone='" + phone + '\'' +
                '}'+"真Tm的牛马报错就TM离谱";
    }
}
```

**测试类:**

```java
package comr.acx.dbutils;

import com.druidtest.Actor;
import com.druidtest.fuxi.JDBCUtils_Druid;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanListHandler;

import java.sql.Connection;
import java.util.List;

public class DBUtils {
    //使用Apache-DBUtils工具类 + Druid完成对表的crud操作
    public static void main (String[]args) {
        //1.得到连接(Druid)
        Connection connection = null;
        try{
            connection = JDBCUtils_Druid.getConnection();
            //2.使用DBUtils类和接口,先引入DBUtils相关的jar,加入到本Project
            //3.创建QueryRunner
            QueryRunner queryrunner = new QueryRunner();
            //编写SQL语句
            String sql = "select * from Demo where id >= ?";
            //4.可以执行相关的方法,返回ArrayList结果集
            //connection:连接
            //sql:SQL语句
            //new BeanListHandler<>(Actor.class):将ResultSet -> Actor对象->封装到ArrayList
            //底层使用反射机制,去获取Actor类的属性,然后进行封装
            //49:给SQL语句中的占位符(?)赋值,可以有多个值,因为是可变参数
            List<Actor> query = queryrunner.query(connection, sql, new BeanListHandler<Actor>(Actor.class),49);
            for(Actor i:query){
                System.out.println(i);
            }
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            //ResultSet,PreparedStatement query底层已经帮我们关闭了所以就不用再次关闭了
            JDBCUtils_Druid.close(connection,null,null);
        }
    }
}
```

查询单行单列 : <font color = blue size = 4>`ScalarHandler`</font>

```java
package comr.acx.dbutils;

import com.druidtest.fuxi.JDBCUtils_Druid;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.ScalarHandler;

import java.sql.Connection;

public class DBUtils_Scalar {
    public static void main(String[]args){
        Connection connection = null;
        try{
            //通过JDBCUtils_Druid调用getConnection方法获取连接
            connection = JDBCUtils_Druid.getConnection();
            //创建QueryRunner实例
            QueryRunner queryrunner = new QueryRunner();
            //返回单行单列,返回的就是Object
            String sql = "select name from Demo where id = ?";
            //查询单行单列的结果使用 ScalarHandler
            //因为返回的是一个对象,使用Handler就是ScalarHandler
            Object query = queryrunner.query(connection, sql, new ScalarHandler(), 49);
            System.out.println(query);
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            //DBUtils底层关闭了ResultSet和PreparedStatement只需要关闭Connection
            JDBCUtils_Druid.close(connection,null,null);
        }
    }
}
```

执行DML语句 : <font size = 4 color = blue>`update`</font>(insert , delete , update)

```java
package comr.acx.dbutils;

import com.druidtest.fuxi.JDBCUtils_Druid;
import org.apache.commons.dbutils.QueryRunner;

import java.sql.Connection;

public class DBUtils_DML {
    public static void main(String[]args){
        Connection connection = null;
        try{
            //通过JDBCUtils_Druid调用getConnection方法获取连接
            connection = JDBCUtils_Druid.getConnection();
            //创建QueryRunner实例
            QueryRunner queryrunner = new QueryRunner();
            //编写SQL语句,并添加占位符
            String sql = "insert into Demo values (?,?,?,?,?)";
            //通过QueryRunner对象调用update传入参数并填写对应的占位符的信息
            int i = queryrunner.update(connection,sql,null,"流桑","男","1998-12-11","111222000");
            System.out.println(i > 0 ? "添加成功" : "添加失败");
        }catch(Exception e){
            e.printStackTrace();
        }finally{
            //通过JDBCUtils_Druid调用close方法传入需要关闭的流对象
            JDBCUtils_Druid.close(connection,null,null);
        }
    }
}
```

Apache-DBUtils

-  表和JavaBean的类型映射关系

![image-20240105144004359](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051440858.png)

## DAO和增删改查通用方法-BasicDao

先分析一个问题

Apache-DBUtils+Druid简化了JDBC开发,但还有不足

1.  SQL语句是固定,不能通过参数传入,通用性不好,需要进行改进,更方便执行<font color = red size = 4>增删改查</font>

2.  对于`select`操作,如果有返回值,返回类型不能固定,需要使用泛型
3.  将来的表很多,业务需求复杂,不可能只靠一个Java类完成

<font color = blue size = 4>示意图:</font>



**DAO和增删改查通用方法-BasicDAO**

**基本说明:**

1.  DAO : data access object访问数据对象
2.  这样的通用类,称为BasicDAO,是专门和数据库交互的,即完成对数据库(表)的crud操作
3.  在BasicDAO的基础上,实现一张表,对应一个DAO,更好的完成功能,比如Customer表-Customer.java类(JavaBean)-CustomerDAO.java

-  定义JDBCUtils_Druid工具类

```java
package comr.acx.uti.utils;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.Properties;

/**
 * @author Dkx
 */
public class JDBCUtils_Druid {
    /**
     * @comments 定义DataSource 变量
     */
    private static DataSource druid;
    /**
     * @comments 定义静态代码块当类加载时加载这个方法执行Properties读取配置文件的信息
     */
    static {
        Properties properties = new Properties();
        try{
            properties.load(new FileInputStream("src//druid.Propeties"));
            druid = DruidDataSourceFactory.createDataSource(properties);
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }

    /**
     * @comments 创建连接方法
     * @return 返回连接结果
     */
    public static Connection getConnection(){
        try{
            return druid.getConnection();
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }

    /**
     * @comments 创建关闭需要关闭的流对象的方法
     * @param connection 关闭Connection对象流
     * @param set 关闭ResultSet对象流
     * @param statment 关闭PreparedStatement对象流
     */
    public static void close(Connection connection, ResultSet set, PreparedStatement statment){
        try{
            if(connection != null){
                connection.close();
            }
            if(set != null ){
                set.close();
            }
            if(statment != null){
                statment.close();
            }
        }catch(Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

-  定义BasicDAO类

```java
package comr.acx.uti.dao;

import comr.acx.uti.utils.JDBCUtils_Druid;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;

import java.sql.Connection;
import java.util.List;

//BasicDAO是其它DAO的父类
/**
 * @author Dkx
 * @param <T> 泛型
 */
public class BasicDAO<T> {
    private QueryRunner que = new QueryRunner();
    //通用的DML方法,针对任意的表
    /**
     *
     * @param sql SQL语句
     * @param parameters 为占位符赋值
     * @return 返回结果,返回执行成功的个数
     */
    public int DML (String sql,Object...parameters) {
        Connection connection = null;
        int i = 0;
        try{
            connection = JDBCUtils_Druid.getConnection();
            i = que.update(connection,sql,parameters);

        }catch(Exception e){
            throw new RuntimeException(e);
        }finally{
            JDBCUtils_Druid.close(connection,null,null);
        }
        return i;
    }
    //返回多个对象(即查询的结果是多行的),针对任意的表
    /**
     *
     * @param sql SQL语句,可以有占位符
     * @param clazz 传入一个类的Class对象,比如Actor.class
     * @param parameters 传入占位符具体的值
     * @return 根据Actor.class类型返回ArrayList集合
     */
    public List<T> DQLtwo (String sql,Class<T> clazz,Object...parameters) {
        Connection connection = null;
        List<T> list = null;
        try{
            connection = JDBCUtils_Druid.getConnection();
            list = que.query(connection, sql, new BeanListHandler<T>(clazz), parameters);
        }catch(Exception e){
            throw new RuntimeException(e);
        }finally{
            JDBCUtils_Druid.close(connection,null,null);
        }
        return list;
    }
    //查询单行结果(i)
    /**
     *
     * @param sql SQL语句
     * @param clazz 传入一个类的Class对象,比如Actor
     * @param parameters 给占位符赋值
     * @return 返回泛型结果
     */
    public T DQLSingle (String sql,Class<T> clazz,Object...parameters) {
        Connection connection = null;
        T i = null;
        try{
            connection = JDBCUtils_Druid.getConnection();
            i = que.query(connection,sql,new BeanHandler<T>(clazz),parameters);
        }catch(Exception e){
            throw new RuntimeException(e);
        }finally{
            JDBCUtils_Druid.close(connection,null,null);
        }
        return i;
    }
    //查询单列结果(一个)
    /**
     *
     * @param sql SQL语句
     * @param parameters 为占位符赋值
     * @return 返回结果
     */
    public Object DQLUniseriate (String sql,Object...parameters) {
        Connection connection = null;
        Object query = null;
        try{
            connection = JDBCUtils_Druid.getConnection();
            query = que.query(connection, sql, new ScalarHandler(), parameters);
        }catch(Exception e){
            throw new RuntimeException(e);
        }finally{
            JDBCUtils_Druid.close(connection,null,null);
        }
        return query;
    }
}
```

-  定义Actor实体类

```java
package comr.acx.uti.domain;

import java.time.LocalDateTime;

/**
 * @author Dkx
 */
public class Actor {
    /**
     * 定义字段对应表的列类型
     */
    private Integer id;
    private String name;
    private String sex;
    private LocalDateTime borndata;
    private String phone;
    /**
     * 定义一个无参构造器初始化 [底层反射需要]
     */
    public Actor(){}
    /**
     * 定义有参构造器为字段进行赋值
     * @param id
     * @param name
     * @param sex
     * @param date
     * @param phone
     */
    public Actor(Integer id,String name,String sex,LocalDateTime date,String phone){
        this.id = id;
        this.name = name;
        this.sex = sex;
        this.borndata = date;
        this.phone = phone;
    }

    /**
     *@Comment 定义get,set方法,方法的返回值类型和字段类型和名称都要对应表中的列数据不然获取不      *到
     * @param id
     */
    public void setId(Integer id){
        this.id = id;
    }
    public Integer getId(){
        return id;
    }
    public void setName(String name){
        this.name = name;
    }
    public String getName(){
        return name;
    }
    public void setSex(String sex){
        this.sex = sex;
    }
    public String getSex(){
        return sex;
    }
    public void setPhone(String phone){
        this.phone = phone;
    }
    public String getPhone(){
        return phone;
    }

    /**
     *
     * @return 返回字符串类型的对象信息
     */
    @Override
    public String toString() {
    return "Actor{" +
            "id=" + id +
            ", name='" + name + '\'' +
            ", sex='" + sex + '\'' +
            ", borndata=" + borndata +
            ", phone='" + phone + '\'' +
            '}';
        }
}
```

-  创建ActorDAO类继承BasicDAO类

```java
package comr.acx.uti.dao;

import comr.acx.uti.domain.Actor;

/**
 * @author Dkx
 */
public class ActorDAO extends BasicDAO <Actor> {
    /**
     * 1.因为ActorDAO继承了BasicDAO就有BasicDAO所有的方法,可以使用方法完成相应的操作
     * 2.如果业务比较复杂也可以再ActorDAO中写特有的方法,根据业务需要可以编写特有的方法
     */
}
```

-  测试类

```java
public class ActorTest {
    public static void main(String[]args){
       //创建DAO类
        ActorDAO dao = new ActorDAO();
       //编写SQL语句
        String sql = "select * from user where username = ? and password = ?";
 		 //通过DAO类对象名调用QueryRunner.query(sql,clazz,parameter);
        List<Actor> actor = dao.DQLtwo(sql, Actor.class,"superbaby","123");
       //遍历出查询借故
        for(Actor i : actor){
            System.out.println(i);
        }
    }
}
```

>  思路:
>
>  -  JDBCUtils_Druid工具类
>
>  编写,读取配置文件方法,连接方法,关闭流方法
>
>  -  Actor实体类
>
>  将从配置文件中读取到的信息赋值到字段中,解决ResultSet复用问题
>
>  -  BasicDAO所有DAO的父类
>
>  定义操作方法,并定义T泛型因为不确定它的类型,它是所有DAO类的父类
>
>  -  ActorDAO继承BasicDAO的子类
>
>  继承BasicDAO类,定义BasicDAO的泛型,可以根据业务需要在这个类中定义特有方法
>
>  -  TestActor测试类
>
>  实例化ActorDAO即可编写SQL语句执行操作表3
