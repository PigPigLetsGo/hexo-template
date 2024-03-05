---
title: JDBC模式实现
categories:
    - [计算机学科,database,JDBC]
tags:
    - 计算机学科
    - database
    - JDBC
---

1.  创建Java JDBC接口规范

```java
package com.ar.www;

public interface JavaJDBC {
    public Object getConnectot();
    public Object getClose();
    public Object getDlc();
}
```

2.  创建MySQL模拟类并实现Java的JDBC接口规范

```java
package com.ar.www;

public class MySQL implements JavaJDBC{
    @Override
    public Object getClose() {
        System.out.println("关闭MySQl数据库");
        return null;
    }

    @Override
    public Object getDlc() {
        System.out.println("对MySQL进行了增删改查");
        return null;
    }

    @Override
    public Object getConnectot() {
        System.out.println("连接了MySQL数据库");
        return null;
    }
}
```

3.  创建test类,模拟MySQL数据库连接Java JDBC

```java
package com.ar.www;

public class JavaTest {
    public static void main(String[]args){
        //对MySQl数据库的操作
        JavaJDBC javajdbc = new MySQL();
        javajdbc.getConnectot();
        javajdbc.getDlc();
        javajdbc.getDlc();
    }
}
```

4.  再创建一个Orcale数据库并实现JDBC规范

```java
package com.ar.www;

public class Orcale implements JavaJDBC{

    @Override
    public Object getConnectot() {
        System.out.println("连接了Orcale数据库");
        return null;
    }

    @Override
    public Object getClose() {
        System.out.println("关闭了Orcale数据库");
        return null;
    }

    @Override
    public Object getDlc() {
        System.out.println("对Orcale数据库进行了增删改查");
        return null;
    }
}
```

5.  test类

```java
package com.ar.www;

public class JavaTest {
    public static void main(String[]args){
        //对MySQl数据库的操作
        JavaJDBC javajdbc = new MySQL();
        javajdbc.getConnectot();
        javajdbc.getDlc();
        javajdbc.getDlc();
        //对Orcale数据库的操作
        javajdbc = new Orcale();
        javajdbc.getConnectot();
        javajdbc.getDlc();
        javajdbc.getClose();
    }
}
```

执行结果:

```java
连接了MySQL数据库
对MySQL进行了增删改查
对MySQL进行了增删改查
连接了Orcale数据库
对Orcale数据库进行了增删改查
关闭了Orcale数据库
```

