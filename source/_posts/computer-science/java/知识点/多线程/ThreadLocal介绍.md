---
title: ThreadLocal介绍
categories:
    - [计算机学科,java,知识点,多线程]
tags:
    - 知识点
    - 多线程
---

# 一、ThreadLocal介绍

## 1.1 介绍

从Java官方文档中的描述：ThreadLocal类用来提供线程内部的局部变量。这种变量在多线程环境下访问(通过get和set方法访问)时能保证各个线程的变量相对独立于其它线程内的变量。ThreadLocal实例通常来说都是private static类型的，用于关联线程和线程上下文。

我们可以得知ThreadLocal的作用是：提供线程内的局部变量，不同的线程之间不会相互干扰，这种变量在线程的生命周期内起作用，减少同一个线程内多个函数或组件之间一些公共变量传递的复杂度。

总结：

1.  线程并发：在多线程并发的场景下
2.  传递数据：我们可以通过ThreadLocal在同一线程，不同组件中传递公共变量
3.  线程隔离：每个线程的变量都是独立的，不会互相影响

## 1.2 基本使用

### 1.2.1 常用方法

在使用之前，我们先来认识几个ThreadLocal的常用方法

| 方法声明                 | 描述                       |
| ------------------------ | -------------------------- |
| ThreadLocal()            | 创建ThreadLocal对象        |
| public void set(T value) | 设置当前线程绑定的局部变量 |
| public T get()           | 获取当前线程绑定的局部变量 |
| public void remove()     | 移除当前线程绑定的局部变量 |

代码案例：

```java
/**
 * 需求：线程隔离
 *      在多线程并发的场景下，每个线程中的变量都是互相独立
 *      线程A：设置(变量1) 获取(变量1)
 *      线程A中设置了变量1后来获取的只能是变量1它不能获取线程B的变量2
 *      线程B：设置(变量2) 获取(变量2)
 */
public class MyDemo01 {
    private String content;
    private String getContent() {
        return content;
    }
    private void setContent(String content) {
        this.content = content;
    }
    public static void main(String[] args) {
        MyDemo01 demo = new MyDemo01();
        for (int i = 0; i < 5; i++) {
            Thread thread = new Thread(new Runnable() {
                @Override
                public void run() {
                    // 每个线程都在做：存一个变量，过一会儿取出变量
                    demo.setContent(Thread.currentThread().getName() + " 的数据");
                    System.out.println("-----------------------------");
                    System.out.println(Thread.currentThread().getName() + "--->" + demo.getContent());
                }
            });
            thread.setName("线程" + i); // 线程 0 ~ 4
            thread.start();
        }
    }
}
```

打印结果：

```
-----------------------------
线程0--->线程2 的数据
-----------------------------
-----------------------------
-----------------------------
线程2--->线程3 的数据
线程3--->线程3 的数据
-----------------------------
线程1--->线程3 的数据
线程4--->线程3 的数据
```

我们可以看到打印的结果是乱的，线程0去 取出了线程 2 的数据。这就是线程不隔离的情况

### 1.2.2 通过ThreadLocal解决线程不隔离情况

使用案例：

还是上面的案例，我们进行修改来感受一下ThreadLocal线程隔离的特点：

```java
/**
 * 需求：线程隔离
 *      在多线程并发的场景下，每个线程中的变量都是互相独立
 *      线程A：设置(变量1) 获取(变量1)
 *      线程A中设置了变量1后来获取的只能是变量1它不能获取线程B的变量2
 *      线程B：设置(变量2) 获取(变量2)
 *      ThreadLocal：
 *          1.set()：将变量绑定到当前线程中
 *          2.get()：获取当前线程绑定的变量
 *          也就是说将某个线程个它的变量绑定死了，等线程n获取变量只会获取到它所绑定的变量n,就不会线程乱串了
 */
public class MyDemo01 {
    ThreadLocal<String> threadLocal = new ThreadLocal<String>();
    private String content;
    private String getContent() {
        //  获取当前线程绑定的变量
        return threadLocal.get();
    }
    private void setContent(String content) {
        // 将content变量绑定到当前线程上
        threadLocal.set(content);
    }
    public static void main(String[] args) {
        MyDemo01 demo = new MyDemo01();
        for (int i = 0; i < 5; i++) {
            Thread thread = new Thread(new Runnable() {
                @Override
                public void run() {
                    // 每个线程都在做：存一个变量，过一会儿取出变量
                    demo.setContent(Thread.currentThread().getName() + " 的数据");
                    System.out.println("-----------------------------");
                    System.out.println(Thread.currentThread().getName() + "--->" + demo.getContent());
                }
            });
            thread.setName("线程" + i); // 线程 0 ~ 4
            thread.start();
        }
    }
}
```

打印结果：

```
-----------------------------
-----------------------------
-----------------------------
线程2--->线程2 的数据
-----------------------------
线程3--->线程3 的数据
-----------------------------
线程0--->线程0 的数据
线程4--->线程4 的数据
线程1--->线程1 的数据
```

## 1.3 ThreadLocal类与synchronized关键字

### 1.3.1 synchronized同步方式

这里可能有的朋友会觉得在上述例子中我们完全可以通过加锁来实现这个功能。我们首先来看一下用synchronized代码块实现的效果：

```java
public class MyDemo02 {
    private String content;
    private String getContent() {
        //  获取当前线程绑定的变量
        return content;
    }
    private void setContent(String content) {
        // 将content变量绑定到当前线程上
        this.content = content;
    }
    public static void main(String[] args) {
        MyDemo02 demo = new MyDemo02();
        for (int i = 0; i < 5; i++) {
            Thread thread = new Thread(new Runnable() {
                @Override
                public void run() {
                    // 每个线程都在做：存一个变量，过一会儿取出变量
                    // 对当前类进行加锁，让其同步执行
                    synchronized(MyDemo02.class) {
                        demo.setContent(Thread.currentThread().getName() + " 的数据");
                        System.out.println("-----------------------------");
                        System.out.println(Thread.currentThread().getName() + "--->" + demo.getContent());
                    }
                }
            });
            thread.setName("线程" + i); // 线程 0 ~ 4
            thread.start();
        }
    }
}
```

打印结果：

```
-----------------------------
线程0--->线程0 的数据
-----------------------------
线程4--->线程4 的数据
-----------------------------
线程2--->线程2 的数据
-----------------------------
线程1--->线程1 的数据
-----------------------------
线程3--->线程3 的数据
```

#### 问题

虽然解决了线程乱取变量的情况，但是我们失去了程序的性能，因为加锁会造成程序的性能降低，之所以会降低是因为加锁的代码线程只能排队去执行，这样程序也失去了并发性

这就是非常重要的区别，这也证明了这个案例中是不合适使用synchronized关键字的，应该使用ThreadLocal

### 1.3.2 ThreadLocal与synchronized的区别

虽然ThreadLocal模式与synchronized关键字都用于处理多线程并发访问变量的问题，不过两者处理问题的角度和思路不同

|        | synchronized                                                 | ThreadLocal                                                  |
| ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 原理   | 同步机制采用 "以时间换空间" <br />的方式，只提供了一份变量，让不同的线程排队访问 | ThreadLocal采用 "以空间换时间" <br />的方式，为每一个线程都提供了一份变量的<br />副本，从而实现同时访问而互不干扰 |
| 则重点 | 多个线程之间访问资源的同步                                   | 多线程中让每个线程之间的数据相互隔离                         |

#### 总结：

>  在刚刚的案例中，虽然使用ThreadLocal和synchronized都能解决问题，但是使用ThreadLocal更为合适，因为这样可以使程序拥有更高的并发性。

# 二、运用场景-事务案例

通过以上介绍，我们已经基本了解ThreadLocal的特点。但是它具体是运用在什么场景中呢？接下来让我们看一个案例：事务操作

## 2.1 转账案例-常规解决方案

### 2.1.1 场景构建

这里我们先构建一个简单的转账场景：有一个数据表account，里面有两个账户jack和rose，用户jack给用户rose转账。

案例的实现主要用mysql数据库，JDBC和C3P0框架。以下是详细代码：

1.  项目结构

    ![image-20240311150504806](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240311150504806.png)

    dao

    ```java
    public class AccountDao {
        // 转出
        public void out(String outUser, int money) throws SQLException {
            String sql = "update account set money = money - ? where name = ?";
            Connection connection = JdbcUtils.getConnection();
            PreparedStatement pstm = connection.prepareStatement(sql);
            pstm.setInt(1, money);
            pstm.setString(2, outUser);
            pstm.executeUpdate();
            JdbcUtils.release(pstm, connection);
        }
    
        // 转入
        public void in(String inUser, int money) throws SQLException {
            String sql = "update account set money = money + ? where name = ?";
            Connection connection = JdbcUtils.getConnection();
            PreparedStatement pstm = connection.prepareStatement(sql);
            pstm.setInt(1, money);
            pstm.setString(2, inUser);
            pstm.executeUpdate();
            JdbcUtils.release(pstm, connection);
        }
    }
    ```

    service

    ```java
    public class AccountService {
        // 转账
        public boolean transfer(String outUser, String inUser, int money) {
            AccountDao ad = new AccountDao();
            try {
                // 转出
                ad.out(outUser, money);
                // 转入
                ad.in(inUser, money);
            } catch (Exception e) {
                return false;
            }
            return true;
        }
    }
    ```

    util

    ```java
    public class JdbcUtils {
        // c3p0数据库连接池对象属性
        private static final ComboPooledDataSource ds = new ComboPooledDataSource();
        // 获取连接
        public static Connection getConnection() {
            try {
                return ds.getConnection();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    
        // 释放资源
        public static void release(AutoCloseable... ios) {
            for (AutoCloseable io : ios) {
                if (io != null) {
                    try {
                        io.close();
                    } catch (Exception e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        }
    
        // 提交事务并释放资源
        public static void commitAndClose(Connection conn) {
            if (conn != null) {
                try {
                    // 提交事务
                    conn.commit();
                    // 释放资源
                    conn.close();
                } catch (SQLException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    
        // 回滚事务并释放资源
        public static void rollbackAndClose(Connection conn) {
            if (conn != null) {
                try {
                    // 回滚事务
                    conn.rollback();
                    // 释放资源
                    conn.close();
                } catch (SQLException e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }
    ```

    web

    ```java
    public class AccountWeb {
        public static void main(String[] args) {
            // 模拟数据：Jack给Rose转账100元
            String outUser = "Jack";
            String inUser = "Rose";
            int money = 100;
            AccountService service = new AccountService();
            boolean result = service.transfer(outUser, inUser, money);
            if (result == false) {
                System.out.println("转账失败");
            } else {
                System.out.println("转账成功");
            }
        }
    }
    ```

    执行结果：

    ```
    转账成功
    ```

    数据库信息

    ![image-20240311150649176](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240311150649176.png)

    我们可以看到执行时没有任何毛病，但是我们模拟一个报错在转账的过程中间

    把数据库的用户金额数据恢复一下

    修改的代码：

    service

    ```java
    public class AccountService {
        // 转账
        public boolean transfer(String outUser, String inUser, int money) {
            AccountDao ad = new AccountDao();
            try {
                // 转出
                ad.out(outUser, money);
                // 算数异常，模拟转出成功，转入失败场景
                int i = 1 / 0;
                // 转入
                ad.in(inUser, money);
            } catch (Exception e) {
                return false;
            }
            return true;
        }
    }
    ```

    执行结果：

    ```
    转账失败
    ```

    查看数据库

    可以看到数据库中用户Jack的钱少了 但是 Rose的钱并没有增多，这就出问题了

    ![image-20240311153423602](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240311153423602.png)

学过数据库的事务特性的就知道可以使用事务来解决，让转账的操作具备原子性，要么同时成功，要么同时失败

JDBC中关于事务的操作api

| Connection接口的方法      | 作用                       |
| ------------------------- | -------------------------- |
| void setAutoCommit(false) | 禁用事务自动提交(改为手动) |
| void comit()              | 提交事务                   |
| void rollback()           | 回滚事务                   |

2.  开启事务的注意点：

    -  为了保证所有的操作在一个事务中，案例中使用的连接必须是同一个：service层开启事务的connection需要跟dao层访问数据库的connection保持一致
    -  线程并发情况下，每个线程只能操作各自的connection

    service与dao都有获取connection对象，我们要保证这两个connection对象是一致的，因为不一致的话，我相当于在service层开启事务的连接不是dao层访问数据库的连接，如果两个对象不一样的话我们在service层设置的事务操作就白设置了，也就是说我们在dao层用的连接必须要和service层的connection对象保持一致

    ![image-20240311172843875](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240311172843875.png)
    
    代码：
    
    dao
    
    ```java
    /**
     * 常规方案：
     *  1.方法添加一个参数connection
     *  2.不能从连接池中获取连接，直接使用参数connection
     *  注意：dao层不能释放连接
     */
    public class AccountDao {
        // 转出
        public void out(String outUser, int money, Connection connection) throws SQLException {
            String sql = "update account set money = money - ? where name = ?";
            // Connection connection = JdbcUtils.getConnection();
            PreparedStatement pstm = connection.prepareStatement(sql);
            pstm.setInt(1, money);
            pstm.setString(2, outUser);
            pstm.executeUpdate();
            // 在dao层不能释放连接，否则报错
            // JdbcUtils.release(pstm, connection);
        }
    
        // 转入
        public void in(String inUser, int money, Connection connection) throws SQLException {
            String sql = "update account set money = money + ? where name = ?";
            // Connection connection = JdbcUtils.getConnection();
            PreparedStatement pstm = connection.prepareStatement(sql);
            pstm.setInt(1, money);
            pstm.setString(2, inUser);
            pstm.executeUpdate();
            // JdbcUtils.release(pstm, connection);
        }
    }
    ```
    
    service
    
    ```java
    /**
     * 事务的使用注意点：
     *  1.service层和dao层的连接对象需要保持一致
     *  2.每个线程的connection对象必须前后一致，线程隔离
     * 常规的解决方案
     *  1.传参：将service层的connection对象直接传递到dao层
     *  2.加锁
     */
    public class AccountService {
        // 转账
        public boolean transfer(String outUser, String inUser, int money) {
            AccountDao ad = new AccountDao();
            Connection connection = null;
            try {
                synchronized(AccountService.class) {
                    // 开启事务
                    connection = JdbcUtils.getConnection();
                    // 设置事务为手动提交
                    connection.setAutoCommit(false);
                    // 转出
                    ad.out(outUser, money, connection);
                    // 算数异常，模拟转出成功，转入失败场景
                    int i = 1 / 0;
                    // 转入
                    ad.in(inUser, money, connection);
                    // 运行到这里没有出现异常提交事务
                    // 提交并释放资源
                    JdbcUtils.commitAndClose(connection);
                }
            } catch (Exception e) {
                // 发生错误了，回滚事务并释放资源
                JdbcUtils.rollbackAndClose(connection);
                return false;
            }
            return true;
        }
    }
    ```

执行结果：

![image-20240312153122419](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240312153122419.png)

如上方式确实可以解决线程同步操作事务，但是这种方式是存在着一些弊端的

### 2.1.2 常规解决方案的弊端

1.  提高代码的耦合度

    我们直接将service的connection对象传递到dao层，这就让两层代码多耦合了一个connection对象

2.  降低程序性能

    加锁造成的，因为加上锁之后这段代码就失去了并发性，也就是说在多线程并发的情况下我们这段代码它也只能一个线程去执行

我们要解决上面的问题那么就可以使用到本章的主角ThreadLocal

## 2.2 转账案例-ThreadLocal解决方案

怎么使用ThreadLocal查看如下图：

![image-20240312153939795](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240312153939795.png)

我们可以在程序当中将connectio使用ThreadLocal进行管理，ThreadLocal核心作用就是将变量进行线程间的隔离

使用ThreadLocal的话就能解决了，线程间的数据相互隔离的特点又通过ThreadLocal进行在项目中模块之间进行了数据传递

代码：

JdbcUtils

```java
public class JdbcUtils {
    static ThreadLocal<Connection> threadLocal = new ThreadLocal<>();
    // c3p0数据库连接池对象属性
    private static final ComboPooledDataSource ds = new ComboPooledDataSource();
    // 获取连接

    /**
     * 原本：直接从连接池中获取连接
     * 现在：
     *  1.直接获取当前线程绑定的连接对象
     *  2.如果连接对象是空的
     *      2.1再去连接池中获取连接
     *      2.2将此连接对象跟当前线程进行绑定
     */
    public static Connection getConnection() {
        // 从ThreadLocal中获取当前线程绑定的连接对象
        Connection connection = threadLocal.get();
        // 判断是否有绑定过的连接对象
        if (connection == null) {
            try {
                // 如果没有就通过ComboPooledDataSource获取一个连接对象
                connection = ds.getConnection();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
            // 将获取到的连接对象绑定到当前线程中
            threadLocal.set(connection);
        }
        // 返回当前线程绑定的连接对象
        return connection;
    }

    // 释放资源
    public static void release(AutoCloseable... ios) {
        for (AutoCloseable io : ios) {
            if (io != null) {
                try {
                    io.close();
                } catch (Exception e) {
                    throw new RuntimeException(e);
                }
            }
        }
    }

    // 提交事务并释放资源
    public static void commitAndClose(Connection conn) {
        if (conn != null) {
            try {
                // 提交事务
                conn.commit();
                // 释放资源
                conn.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }

    // 回滚事务并释放资源
    public static void rollbackAndClose(Connection conn) {
        if (conn != null) {
            try {
                // 回滚事务
                conn.rollback();
                // 释放资源
                conn.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

service

```java
/**
 * 事务的使用注意点：
 * 1.service层和dao层的连接对象需要保持一致
 * 2.每个线程的connection对象必须前后一致，线程隔离
 * 常规的解决方案
 * 1.传参：将service层的connection对象直接传递到dao层
 * 2.加锁
 */
public class AccountService {
    // 转账
    public boolean transfer(String outUser, String inUser, int money) {
        AccountDao ad = new AccountDao();
        Connection connection = null;
        try {
            // 开启事务
            connection = JdbcUtils.getConnection();
            // 设置事务为手动提交
            connection.setAutoCommit(false);
            // 转出
            ad.out(outUser, money);
            // 算数异常，模拟转出成功，转入失败场景
            int i = 1 / 0;
            // 转入
            ad.in(inUser, money);
            // 运行到这里没有出现异常提交事务
            // 提交并释放资源
            JdbcUtils.commitAndClose(connection);
        } catch (Exception e) {
            // 发生错误了，回滚事务并释放资源
            JdbcUtils.rollbackAndClose(connection);
            return false;
        }
        return true;
    }
}
```

dao

```java
/**
 * 常规方案：
 * 1.方法添加一个参数connection
 * 2.不能从连接池中获取连接，直接使用参数connection
 * 注意：dao层不能释放连接
 */
public class AccountDao {
    // 转出
    public void out(String outUser, int money) throws SQLException {
        String sql = "update account set money = money - ? where name = ?";
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement pstm = connection.prepareStatement(sql);
        pstm.setInt(1, money);
        pstm.setString(2, outUser);
        pstm.executeUpdate();
        // 在dao层不能释放连接，否则报错
        // JdbcUtils.release(pstm, connection);
    }

    // 转入
    public void in(String inUser, int money) throws SQLException {
        String sql = "update account set money = money + ? where name = ?";
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement pstm = connection.prepareStatement(sql);
        pstm.setInt(1, money);
        pstm.setString(2, inUser);
        pstm.executeUpdate();
        // JdbcUtils.release(pstm, connection);
    }
}
```

执行结果：

```
转账失败
```

数据库结果

![image-20240312214544525](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240312214544525.png)

转账失败后 金额没有扣除，这就说明上面的事务设置经过ThreadLocal的改造成功了

但是还有一个需要注意的点，就在service层中有一个commitAndClose当数据库访问完成之后我们需要提交和释放连接

我们需要点进这个方法到JdbcUtils中添加一个remove()方法

```java
// 提交事务并释放资源
public static void commitAndClose(Connection conn) {
   if (conn != null) {
      try {
         // 提交事务
         conn.commit();
         // 新加代码-》
         // 解绑当前线程绑定的连接对象
         // 解绑对象是为了防止内存泄漏
         threadLocal.remove();
         // 释放资源
         conn.close();
      } catch (SQLException e) {
         throw new RuntimeException(e);
      }
   }
}
```

还有rollbackAndClose方法也需要加上

```java
// 回滚事务并释放资源
public static void rollbackAndClose(Connection conn) {
   if (conn != null) {
      try {
         // 回滚事务
         conn.rollback();
         // 新加代码-》
         // 解绑当前线程绑定的连接对象
         // 解绑对象是为了防止内存泄漏
         threadLocal.remove();
         // 释放资源
         conn.close();
      } catch (SQLException e) {
         throw new RuntimeException(e);
      }
   }
}
```

## 2.3 ThreadLocal方案的好处

从上述的案例中我们可以看到，在一些特定场景下，ThreadLocal方案有两个突出的优势：

1.  **传递数据**：保存每个线程绑定的数据，在需要的地方可以直接获取，避免参数直接传递带来的代码耦合问题
2.  **线程隔离**：各线程之间的数据相互隔离却又具备并发性，避免同步方式带来的性能损失

# 三、ThreadLocal的内部结构

通过以上的学习，我们对ThreadLocal的作用有了一定的认识。现在我们一起看一下ThreadLocal的内部结构，探究它能够实现线程数据隔离的原理。

## 3.1 常见的误解

通常，如果我们不去看源代码的话，我猜ThreadLocal是这样子设计的：每个ThreadLocal类都创建一个Map，然后用线程ID threadID作为Map的Key，要存储的局部变量为Map的Value，这样就能达到各个线程的局部变量隔离的效果。这是最简单的设计方法，JDK最早其的ThreadLocal就是这样设计的。

![image-20240312220913892](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240312220913892.png)

## 3.2 现在的设计

但是，JDK后面优化了设计方案，在JDK8中ThreadLocal的设计是：每个Thread维护一个ThreadLocalMap，这个Map的Key是ThreadLocal实例本身，Value才是真正要存储的值Object。

具体的过程是这样的：

1.  每个Thread线程内部都有一个Map(ThreadLocalMap)
2.  Map里面都存储ThreadLocal对象(Key)和线程的变量副本(Value)
3.  Thread内部的Map是由ThreadLocal维护的，由ThreadLocal负责向Map获取和设置线程的变量值。
4.  对于不同的线程，每次获取副本值时，别的线程并不能获取到当前线程的副本值，形成了副本的隔离，互不干扰。

![image-20240312221406799](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240312221406799.png)

### 3.2.1 JDK8设计方案的两个好处

1.  每个Map存储的Entry数量变少
2.  当Thread销毁的时候ThreadLocal也会随着销毁减少内存的使用

# 四、ThreadLocal的核心方法源码

基于ThreadLocal的内部结构，我们继续分析它的核心方法源码，更深入的了解其操作原理。

除了构造方法之外，ThreadLocal对外暴漏的方法有以下4个：

| 方法声明                 | 描述                         |
| ------------------------ | ---------------------------- |
| protected initialValue() | 返回当前线程局部变量的初始值 |
| public void set(T value) | 设置当前线程绑定的局部变量   |
| public T get()           | 获取当前线程绑定的局部变量   |
| public void remove()     | 移除当前线程绑定的局部变量   |

以下是这4个方法的详细源码分析(为了保证思路清晰，ThreadLocalMap部分暂时不展开，下一个知识点详解)

## 4.1 set

```java
public void set(T value) {
  // 获取当前线程对象
  Thread t = Thread.currentThread();
  // 获取此线程对象中维护的 ThreadLocalMap 对象  
  ThreadLocalMap map = getMap(t);
  // 判断是否存在
  if(map != null)
    map.set(this, value);
  else
    // 1) 当前线程 Thread 不存在 ThreadLocalMap 对象.
    // 2) 则调用 createMap 进行ThreadLocalMap 对象初始化.
    // 3) 并将 t 和 value(对应的值) 作为第一个 entry 存放至 ThreadLocalMap 中
    createMap(t, value);
}

ThreadLocalMap getMap(Thread t) {
  return t.threadLocals;
}

void createMap(Thread t, T firstValue) {
    t.threadLocals = new ThreadLocalMap(this, firstValue);
}

/**
    总结：
    A. 首先获取当前线程，并根据当前线程获取一个Map.
    B. 如果获取的Map不为空，则将参数设置到Map中(当前 ThreadLocal 作为 key)
    C. 如果Map为空, 则给该线程创建 Map, 并设置初始值.
**/
```

代码执行流程：

1.  首先获取当前线程，并根据当前线程获取一个Map
2.  如果获取的Map不为空，则将参数设置到Map中(当前ThreadLocal的引用作为Key)
3.  如果Map为空，则给该线程创建Map，并设置初始值

## 4.2 get

```java
public T get() {
   // 获取当前线程对象 和 ThreadLocalMap;
   Thread t = Thread.currentThread();
   ThreadLocalMap map = getMap(t);
   
   // 如果 map 存在, 则直接返回 map 中 ThreadLocal 对应的值
   if(map != null) {
     ThreadLocalMap.Entry e = map.getEntry(this);
     
     if(e != null) {
        T result = (T)e.getValue();
       return result;
     }
   }
   
   // map 不存在 或 map 中没 set 过
   return setInitialValue();
}

private T setInitialValue() {
  // 调用 initialValue 获取初始化的值
  // initialValue 可以被子类重写，如果不重写, 默认返回 null
  T value = initialValue();
  
  // 获取当前线程对象 和 ThreadLocalMap;
  Thread t = Thread.currentThread();
  ThreadLocalMap map = getMap(t);
  
  if(map != null)  // map 存在则设置默认值
    map.set(this, value);
  else 
    // 1) 当前线程 Thread 不存在 ThreadLocalMap 对象.
    // 2) 则调用 createMap 进行ThreadLocalMap 对象初始化.
    // 3) 并将 t 和 value(对应的值) 作为第一个 entry 存放至 ThreadLocalMap 中
    createMap(t, value);
}
```

代码执行流程：

1.  首先获取当前线程，根据当前线程获取一个Map
2.  如果获取的Map不为空，则在Map中以ThreadLocal的引用作为Key来在Map中获取对应的Entry，否则转到第4步
3.  如果e不为null，则返回e.value，否则转到 第4步
4.  Map为空或者e为空，则通过initialValue函数获取初始值Value，然后用ThreadLocal的引用和Value作为firstKey和firstValue创建一个新的Map

总结：先获取当前线程的ThreadLocalMap变量，如果存在则返回值，不存在则创建并返回初始值

## 4.3 remove方法

```java
public void remove() {
  // 获取当前线程中保存 ThreadLocalMap
  ThreadLocalMap map = getMap(Thread.currentThread());
  
  if(map != null)
    // 存在则调用 map.remove 方法
    map.remove(this);
}
```

代码执行流程：

1.  首先后去当前线程，并根据当前线程获取一个Map
2.  如果获取的Map不为空，则移除当前ThreadLocal对象对应的entry

## 4.4 initialValue方法

```java
/**
  * 返回当前线程对应的 ThreadLocal 变量的 初始值.
  
  * 此方法的第一次调用发生在, 当线程通过 get 方法访问此线程的 ThreadLocal 变量时
  * 除非线程先调用 set 方法, 在这种情况下, initialValue 才不会被 get 方法调用
  * 通常情况下, 每个线程最多调用一次这个方法
  
  * <p> 这个方法仅仅简单的返回 null {@code null}
  * 如果程序员想 ThreadLocal 线程局部变量有个除 null 以外的初始值, 
  * 必须通过子类继承 {@code ThreadLocal} 的方式去重写此方法.
  * 通常, 可以通过匿名类的方式实现.
  
  * @return 当前 ThreadLocal 的初始值
  */
protected T initialValue() {
  return null;
}
```

此方法的作用是返回该线程局部变量的初始值

1.  这个方法是一个延迟调用方法，从上面的代码我们得知，在set方法还未调用而先调用了get方法时才执行，并且仅执行1次
2.  这个方法缺省实现直接返回一个null
3.  如果想要一个除null之外的初始值，可以重写次方法。(备注：该方法是一个protected的方法，显然是为了让子类覆盖而设计的)

# 五、ThreadLocalMap源码分析

在分析ThreadLocal方法的时候，我们了解到了ThreadLocal的操作实际上是围绕ThreadLocalMap展开的。

ThreadLocalMap的源码相对比较复杂，我们从以下三个方面进行讨论

## 5.1 基本结构

ThreadLocalMap是ThreadLocal的内部类，没有实现Map接口，用独立的方式实现了Map的功能，其内部Entry也是独立实现。

![image-20240313082856318](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240313082856318.png)

### 5.1.1 成员变量

```java
/**
 * 初始容量, 必须是 2 的整数次幂
 */
public static final int INITIAL_CAPACITY = 16;

/**
 * 存放 table, Entry 类的定义在下面分析
 * 同样, 数组的长度必须是 2 的整数次幂
 */
private Entry[] table;

/**
 * 数组里面 entry 的个数, 可以用于判断 table 当前的使用量是否超过阈值
 */
private int size = 0;

/**
 * 进行扩容的阈值, table 的使用量大于它的时候进行扩容.
 */
private int threshold;
```

跟HashMap类似，INITIAL_CAPACITY代表这个Map的初始容量；table是一个Entry类型的数组，用于存储数据；size代表表中的存储数目；threshold代表需要扩容时对应size的阈值

### 5.1.2 存储结构Entry

```java
static class Entry extends WeakRefernece<ThreadLocal<?>> {
    Object value;
    
    Entry(ThreadLocal<?> k, object v) {
        super(k);
        value = v;
    }
}
```

在ThreadLocalMap中，也是用Entry来保存K-V结构数据的。不过Entry中的Key只能是ThreadLocal对象，这点在构造方法中已经限定死了。

另外，Entry继承WeakReference，也就是Key(ThreadLocal)是弱引用，其目的是将ThreadLocal对象的生命周期和线程生命周期解绑。

## 5.2 弱引用和内存泄漏

有些程序员在使用ThreadLocal的过程中会发现有内存泄漏的情况发生，就猜测这个内存泄漏跟Entry中使用了弱引用的KEy有关系。这个理解其实是不对的。

我们先来回顾这个问题中涉及的几个名词概念，再来分析问题。

### 5.2.1 内存泄漏相关概念

-  Memory overflow：内存溢出，没有足够的内存提供申请者使用。
-  Memoty leak：内存泄漏是指程序中已动态分配的堆内存由于某种原因程序未释放或无法释放，造成系统内存的浪费，导致程序运行速度减慢甚至系统崩溃等严重后果。内存泄漏的堆积终将导致内存溢出。

### 5.2.2 弱引用相关概念



Java中的引用有4种类型：强，软，弱，虚。当前这个问题主要涉及到强引用和弱引用：

**强引用**("Strong" Reference)：就是我们最常见的普通对象引用，只要还有强引用指向一个对象，就能表明对象还 "活着"， 垃圾回收器就不会回收这种对象。

**弱引用**(WeakReference)：垃圾回收器一旦发现了只是弱引用的对象，不管当前内存空间足够与否，都会回它的内存。

### 5.2.3 如果Key使用强引用

假设ThreadLocalMap中Key使用了强引用，那么会出现内存泄漏吗？

此时ThreadLocal的内存图(实线表示强引用)如下：

![image-20240313164858485](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240313164858485.png)

1.  业务代码中使用了ThreadLocal，ThreadLocal Ref被回收了

2.  但是因为ThreadLocalMap的Entry强引用了ThreadLocal，造成ThreadLocal无法被回收

3.  在没有手动删除这个Entry以及CurrentThread依然运行的前提下，使用有强引用链thread Ref -> current Thread -> threadLocalMap -> entry，entry就不会被回收(Entry当中包含了ThreadLocal实例和value)，导致Entry的内存泄漏

    也就是说：ThreadLocalMap中的Key使用了强引用，是无法避免内存泄漏的。

### 5.2.4 如果Key使用弱引用

当ThreadLocal中的Key使用了弱引用，会出现内存泄漏么？

![image-20240313170442894](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240313170442894.png)

1.  同样假设业务代码中使用了ThreadLocal，且ThreadLocal Ref被回收了。

2.  由于ThreadLocalMap支持有ThreadLocalMap的弱引用，没有任何强引用指向ThreadLocal实例，所以ThreadLocal就可以顺利被GC回收，此时Entry的Key = null

3.  但是在没有手动删除这个Entry以及CurrentThread依然运行的前提下，存在强引用链thread Ref -> currentThread -> threadLocalMap -> entry -> value，value不会被回收，而这块value永远不会被访问到了，导致value的内存泄漏

    也就是说：ThreadLocalMap的Key使用了弱引用，也可能导致内存泄漏的问题。

## 5.3 出现内存泄漏的真实原因

比较以上两种情况，我们就会发现，内存泄漏的发生跟ThreadLocalMap当中的Key是强还是弱引用没有关系，而造成真正的内存泄漏的原因如下：

1.  没有手动删除这个Entry，(table数组指定位置置为null，断掉强引用链)
2.  CurrentThread依然运行(CurrentThread销毁，则指向ThreadLocalMap的引用链就断了，进而引发后续链条上全部对象GC)

综上：ThreadLocal内存泄漏的根源是：==由于ThreadLocalMap的生命周期跟Thread导致的==(如果存在线程池，Thread复用，则这个ThreadLocalMap永远不会销毁掉)，如果没有手动删除对应的Key，就会导致内存泄漏。

## 5.4 为什么弱引用会导致内存泄漏，还是要使用弱引用呢？

事实上，ThreadLocalMap中的set/ getEntry方法中，会对Key为null(即ThreadLocal为null)进行判断，如果为null的话，那么value也会置为null

这就意味着使用完ThreadLocal，CurrentThread依然运行的前提下，就算忘记调用remove方法，弱引用可以比强引用多一层保障：弱引用的ThreadLocal会被回收，对应的value在下一次调用set，get，remove中任何一个方法的时候会被清除，从而避免内存泄漏

# 六、Hash冲突的解决

hash冲突的解决是Map中的一个重要内容。我们以hash冲突的解决方案为线索，来研究下ThreadLocalMap的核心源码。

![image-20240313203759499](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240313203759499.png)

>  **构造方法**：ThreadLocalMap(ThreadLocal<?> firstKey, Object value)
>
>  有不明白的地方看下述ThreadLocalMap核心源码部分的当中关于成员属性的描述
>
>  构造函数首先创建一个16大小Entry数组，然后计算出firstKey的索引，存储到table中，并设置size和threadold

## 6.1 ThreadLocalMap构造方法

```java
ThreadLocalMap(ThreadLocal<?> firstKey, Object value) {
    // 初始化 table
    table = new ThreadLocal.ThreadLocalMap.Entry[INITIAL_CAPACITY];
    
    // 计算索引(重点)：计算 fistKey 这个 ThreadLocal 在 table 中的存放位置
    int i = firstKey.threadLocalHashCode & ( INITIAL_CAPACITY - 1);
  
    // 创建 Entry 并放置到 table
    table[i] = new ThreadLocal.ThreadLocalMap.Entry(firstKey, value);
    size = 1;  
  
    // 设置阈值
    setThreshold(INITIAL_CAPACITY);
}
```

重点分析：`int i = firstKey.threadLocalHashCode & (INITIAL_CAPACITY - 1);`这一句中hash计算

-  关于ThreadLocal.threadLocalhashCode

```java
private final threadLocalHashCode = nextHashCode();

private static int nextHashCode() {
    return nextHashCode.getAndAdd(HASH_INCREMENT);  // 追加
}

// AutomicInteger 是一个提供原子操作的 Integer 类, 通过线程安全的方式操作加减，适合高并发的情况下使用.
private static AutomicInteger nextHashCode = new AutomicInteger();

// 特殊的 hash 值：和斐波拉契数列，黄金分割数是有关系的。
private static final int HASH_INCREMENT = 0x61c88647;
```

这里定义了一个AutomicInteger类型，每次当前值并加上HASH_INCREMENT，这个HASH_INCREMENT是和斐波那契数列(黄金分割数)有关，其主要目的就是为了让哈希码能均匀的分布在2的n次方的数组里，也就是Entry[] table中，这样做可以尽量避免hash冲突

-  关于 & (INITIAL_CAPACITY - 1)

   计算索引位置的时候采用了hashcode & (size - 1)的方式，相当于取余操作hashCode % size的一个更高效的实现，正是因为这种算法，我们要求size必须是2的整数次幂，这也能==保证在索引不越界的情况下，使得hash发生冲突的次数减小==。

## 6.2 ThreadLocalMap的set方法

```java
private void set(ThreadLocal<?> key, Object value) {
    ThreadLocal.ThreadLocalMap.Entry[] tab = table;
    int len = tab.length;
  
    // 计算索引
    int i = key.threadLocalHashCode & (len - 1);
  
    /**
     * 重点代码：使用线性探测法查找元素
     */
     for(ThreadLocal.ThreadLocalMap.Entry e : tab[i]; e != null; e = tab[i = nextIndex(i, len)]) {
        
         ThreadLocal<?> k = e.get();
       
         // ThreadLocal 对应的key 存在，则覆盖之前的值
         if(k == key) {
             e.value = value;
             return;
         }
       
         // key 为 null, 但是值不为 null (未及时 remove, 但之前放置此位置 ThreadLocal 被回收的情况), 当前数组中的 Entry 是一个成旧的 (stale) 元素.
         if(k == null) {
             // 用新的元素替换成旧的元素，这个进行了不少的垃圾清理动作, 防止内存泄漏
             replaceStaleEntry(key, value, i);
             return;
         }
     }
  
    // ThreadLocal 对应的 key 不存在，且没有找到陈旧的元素，则在空元素的位置创建一个新的 Entry
    tab[i] = new Entry(key, value);
    int sz = ++size;
  
    /**
     * cleanSomeSlots 用于清除那些 e.get() == null 的元素（即, key 为 null 的元素）.
     * 这种数据 key 关联的对象已经被回收了，所以这个 Entry(table[index]) 可以被置为 null,
     * 如果没有清除任何 entry, 并且当前使用量的达到了 负载因子 所定义的 2/3, 那么进行 rehash (执行一次全表的，扫描清理工作)
     */
    if(!cleanSomeSlots(i, sz) && sz >= threshold)
        rehash();
}

/**
 * 获取环形索引的下一个数组
 */
private static int nextIndex(int i, int len) {
    return ((i + 1 < len) ? i + 1 : 0;)
}
```

-  执行流程简述：

   1.  首先根据key计算索引i，然后查找i位置的上Entry
   2.  若是Entry已经存在，并且key等于传入的key，那么这个时候直接给这个Entry赋新的value值
   3.  若是Entry已经存在，并且key为null，则调用的replaceStaleEntry来更换这个key为空Entry
   4.  不断循环检测，直到遇见为null的地方，这个时候要是还没有在循环中return，那么就在这个null的位置新建一个Entry，并且插入 size  + 1
   5.  最后调用cleanSomeSlots，清理key为null的Entry，最后返回是否清理了Entry，接下来判断sz是否 >= thresold达到rehash条件，达到的话就会调用rehash函数执行一次全表的清理工作

   **重点分析**：ThredLocalMap使用线性探测法来解决hash冲突：

   该方法一次探测一个地址，直到有空的地址后插入，若整个空间都找不到空余的地址，则产生溢出。

   举个例子：假设当前的tab长度为16，也就是说如果计算出来key的索引为14，如果table[14]上已经有值，并且key与当前key不一致，那么就发生了hash冲突，这个时候将14 + 1 得到 15 ，取table[15]进行判断，这个时候如果还是冲突就会返回0，取table[0]，以此类推，知道可以插入。按照上面这个描述，可以把Entry[] table看成一个环形数组。
