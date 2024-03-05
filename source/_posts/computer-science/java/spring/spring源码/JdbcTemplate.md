---
title: JdbcTemplate
categories:
   - [计算机学科,java,spring,spring源码]
tags:
   - 计算机学科
   - spring
   - 源码
---

## JdbcTemplate

**概念和准备** 

1.什么是JdbcTemplate

-  Spring框架对JDBC进行封装，使用JdbcTemplate方便实现对数据库操作

2.准备工作

1.  引入相关jar包

    1.  连接数据库需要的依赖

    ![image-20230612145428113](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612145428113.png)

    2.  Spring操作数据库需要的依赖
        1.  jdbc操作数据库
        2.  tx 事务，还需要aop

    ![image-20230612145521750](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612145521750.png)

    ![image-20230612145554117](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612145554117.png)
    
    ![image-20230612161733001](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612161733001.png)
    
    ​						aop需要的一些依赖不添加在使用xml配置事务的时候会用到aop会报错
    
    ![image-20230613102918363](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613102918363.png)

orm整合其它依赖所需要的jar包比如整合mybatis需要这个jar包，没有整合其它依赖可以不加

![image-20230612145740815](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612145740815.png)

完整的jar包引入：

![image-20230612161753310](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612161753310.png)

2.在spring配置文件配置数据库连接池

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
</beans>
```

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test?characterEncoding=utf-8&serverTimezone=UTC&useUnicode=true
jdbc.username=root
jdbc.password=dkx
```

3.配置JdbcTemplage对象，注入DataSource

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
    <!--JdbcTemplate对象-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!--注入DataSource-->
        <property name="dataSource" ref="dataSource"></property>
    </bean>
</beans>
```

4.创建service类，创建dao类，在dao注入jdbcTemplate对象，在service注入dao对象

```java
/**
 * dao接口
 */
public interface BookDao {
   
}

/**
 * dao接口实现类
 */
@SuppressWarnings("all")
@Repository
public class BookDaoImpl implements BookDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;
}

/**
 * service接口
 */
public interface BookService {
   
}

/**
 * service接口实现类
 */
@SuppressWarnings("all")
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;
}
```

## JdbcTemplate操作数据库(添加)

编写实体类

```java
@SuppressWarnings("all")
@Getter
@Setter
@ToString
@AllArgsConstructor
@NoArgsConstructor
public class Spring5demo {
    private Integer userId;
    private String username;
    private String ustatus;
}
```

编写service和dao

-  在dao进行数据库添加操作

调用JdbcTemplate对象里面update方法实现添加操作

```java
update(Spring sql,Object ... args)
```

-  有两个参数
   -  第一个参数：sql语句
   -  第二个参数：可变参数，设置sql语句值

目录结构：

![image-20230612161932383](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612161932383-1686557974247-3.png)

代码：

```java
/**
 * dao接口
 */
public interface BookDao {
    int add(Spring5demo spring5demo);
}
//-----------------------------------------------------------------------------
/**
 * dao接口实现类
 */
@SuppressWarnings("all")
@Repository
public class BookDaoImpl implements BookDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public int add(Spring5demo spring5demo){
        Object[] objs = {spring5demo.getUsername(),spring5demo.getUstatus()};
        String sql = "insert into spring5demo (username,ustatus) values (?,?)";
        int update = jdbcTemplate.update(sql,objs);
        return update;
    }
}
//-----------------------------------------------------------------------------
/**
 * service接口
 */
public interface BookService {
    boolean add(Spring5demo spring5demo);
}
//-----------------------------------------------------------------------------
/**
 * service接口实现类
 */
@SuppressWarnings("all")
@Service(value = "bookService")
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;

    public boolean add(Spring5demo spring5demo){
        int flag = bookDao.add(spring5demo);
        return flag > 0 ? true : false;
    }
}
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext
               ("application.xml");
       BookService service = context.getBean("bookService", BookService.class);
       Spring5demo spring = new Spring5demo();
       spring.setUsername("张三");
       spring.setUstatus("1");
       boolean flag = service.add(spring);
       System.out.println(flag == true ? "添加成功" : "添加失败");
    }
}
```

测试结果：

```
Jun 12, 2023 4:21:08 PM com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
INFO: {dataSource-1} inited
添加成功
```

![image-20230612162138627](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612162138627.png)

## JdbcTemplate操作数据库(修改和删除)

dao

```java
/**
 * dao接口
 */
public interface BookDao {
    int add(Spring5demo spring5demo);
    int update(Spring5demo spring5demo);
    int delete(String username);
}
```

daoImpl

```java
/**
 * dao接口实现类
 */
@SuppressWarnings("all")
@Repository
public class BookDaoImpl implements BookDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    public int add(Spring5demo spring5demo){
        Object[] objs = {spring5demo.getUsername(),spring5demo.getUstatus()};
        String sql = "insert into spring5demo (username,ustatus) values (?,?)";
        int update = jdbcTemplate.update(sql,objs);
        return update;
    }
   
	//实现动态修改sql
   public int update(Spring5demo spring5demo){
        Integer id = spring5demo.getUserId();
        String username = spring5demo.getUsername();
        String uStatus = spring5demo.getUstatus();
        String sql = "update spring5demo";
        StringBuffer sb = new StringBuffer(sql);
        if(username != null && !"".equals(username)){
            if(uStatus != null && !"".equals(uStatus)){
                sb.append(" set username = ? ,");
            }else{
                sb.append(" set username = ? ");
            }
        }
        if(uStatus != null && !"".equals(uStatus)){
            if(username != null && !"".equals(username)){
                sb.append(" ustatus = ? ");
            }else{
                sb.append(" set ustatus = ? ");
            }
        }
        sb.append(" where user_id = ?");
        if(username != null && uStatus == null){
            int flag = jdbcTemplate.update(sb.toString(),username,id);
            return flag;
        }
        if(uStatus != null && username == null){
            int flag = jdbcTemplate.update(sb.toString(),uStatus,id);
            return flag;
        }
        if(username != null && uStatus != null){
            int flag = jdbcTemplate.update(sb.toString(),username,uStatus,id);
            return flag;
        }
        return -1;
    }

    public int delete(String username){
        String sql = "delete from spring5demo where username = ?";
        int flag = jdbcTemplate.update(sql,username);
        return flag;
    }
}
```

service

```java
/**
 * service接口
 */
public interface BookService {
    boolean add(Spring5demo spring5demo);
    boolean update(Spring5demo spring5demo);
    boolean delete(String username);
}
```

serviceImpl

```java
/**
 * service接口实现类
 */
@SuppressWarnings("all")
@Service(value = "bookService")
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;

    public boolean add(Spring5demo spring5demo){
        int flag = bookDao.add(spring5demo);
        return flag > 0 ? true : false;
    }

    public boolean update(Spring5demo spring5demo){
        int flag = bookDao.update(spring5demo);
        return flag > 0 ? true : false;
    }

    public boolean delete(String username){
        int flag = bookDao.delete(username);
        return flag > 0 ? true : false;
    }
}
```

测试代码：

```java
//测试update
public class AppTest {
    @Test
    public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext
               ("application.xml");
       BookService service = context.getBean("bookService", BookService.class);
       Spring5demo spring = new Spring5demo();
       spring.setUserId(1);
       spring.setUsername("刘桑");
       spring.setUstatus("50");
       boolean flag = service.update(spring);
       System.out.println(flag == true ? "OK" : "NO");
    }
}
------------------------------------Result------------------------------------
Jun 12, 2023 5:17:51 PM com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
INFO: {dataSource-1} inited
OK
//测试delete
public class AppTest {
    @Test
    public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext
               ("application.xml");
       BookService service = context.getBean("bookService", BookService.class);
       boolean flag = service.delete("刘桑");
       System.out.println(flag == true ? "OK" : "NO");
    }
}
------------------------------------Result------------------------------------
Jun 12, 2023 5:19:55 PM com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
INFO: {dataSource-1} inited
OK
```

## JdbcTemplate操作数据库(查询)

### JdbcTemplate操作数据库(查询返回某个值)

1.查询表中有多少条记录，返回是某个值

2.使用JdbcTemplate实现查询返回某个值代码

```java
queryForObject(String sql,Class<?> requiredType)
```

有两个参数：

-  第一个参数：sql语句
-  第二个参数：返回类型Class

dao，service返回值类型都是int 方法名都是selectCount()无参数

```java
public int selectCount(){
   String sql = "select count(*) from spring5demo";
   int count = jdbcTemplate.queryForObject(sql,Integer.class);
   return count;
}
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext
               ("application.xml");
       BookService service = context.getBean("bookService", BookService.class);
       int f = service.selectCount();
        System.out.println("查询返回的总个数："+f);
    }
}
```

测试结果：

```
Jun 12, 2023 5:32:00 PM com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
INFO: {dataSource-1} inited
查询返回的总个数：3
```

### JdbcTemplate操作数据库(查询返回对象)

JdbcTemplate实现查询返回对象

```java
queryForObject(String sql,RowMapper<T> rowMapper,Object ... args)
```

有三个参数：

-  第一个参数：sql语句
-  第二个参数：RowMapper，是接口，返回不同类型数据，使用这个接口里面实现类完成数据封装
-  第三个参数：sql语句值

```java
public Spring5demo findObject(Integer id){
   String sql = "select * from spring5demo where user_id = ?";
   Spring5demo spring = jdbcTemplate.queryForObject(sql,new BeanPropertyRowMapper<Spring5demo>(Spring5demo.class),id);
   return spring;
}
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext
               ("application.xml");
       BookService service = context.getBean("bookService", BookService.class);
       Spring5demo spring = service.findObject(1);
        System.out.println(spring);
    }
}
```

测试结果：

```
Jun 12, 2023 6:27:20 PM com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
INFO: {dataSource-1} inited
Spring5demo(userId=1, username=zhangsna, ustatus=0)
```

### JdbcTemplate操作数据库(查询返回集合)

调用JdbcTemplate方法实现查询返回集合

```java
query(String sql,RowMapper<T> rowMapper,Object ... args)
```

有三个参数：

-  第一个参数：sql语句
-  第二个参数：RowMapper是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装
-  第三个参数：sql语句值

```java
public List<Spring5demo> findAll(){
   String sql = "select * from spring5demo";
   List<Spring5demo> list = jdbcTemplate.query(sql,new BeanPropertyRowMapper<Spring5demo>(Spring5demo.class));
   return list;
}
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext
               ("application.xml");
       BookService service = context.getBean("bookService", BookService.class);
       List<Spring5demo> list = service.findAll();
       System.out.println(list);
    }
}
```

测试结果：

```
Jun 12, 2023 6:37:43 PM com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
INFO: {dataSource-1} inited
[Spring5demo(userId=1, username=zhangsna, ustatus=0), Spring5demo(userId=2, username=lisi, ustatus=0), Spring5demo(userId=3, username=asd, ustatus=0)]
```

## JdbcTemplate操作数据库(批量操作)

批量操作：操作表里面多条记录

JdbcTemplate实现批量添加操作

```java
bathUpdate(String sql,List<Object[]> bathArgs)
```

有两个参数：

-  第一个参数：sql语句
-  第二个参数：List集合，添加多条记录数据

```java
 public int[] findAdd(List<Object[]> batchArgs){
    String sql = "insert into spring5demo (username,ustatus) values (?,?)";
    int[] is = jdbcTemplate.batchUpdate(sql,batchArgs);
    return is;
 }
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext
               ("application.xml");
       BookService service = context.getBean("bookService", BookService.class);
       List<Object[]> list = new ArrayList<>();
       Object[] o1 = {"刘华强","0"};
       Object[] o2 = {"熊大","0"};
       Object[] o3 = {"熊二","0"};
       list.add(o1);
       list.add(o2);
       list.add(o3);
       boolean flag = service.findAdd(list);
        System.out.println(flag == true ? "OK":"NO");
    }
}
```

测试结果：

```
Jun 12, 2023 6:55:34 PM com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
INFO: {dataSource-1} inited
OK
```

![image-20230612185701887](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612185701887.png)

## JdbcTemplate实现批量修改操作

```java
public int[] findUpdate(List<Object[]> batchArgs){
   String sql = "update spring5demo set username = ?,ustatus = ? where user_id = ?";
   int[] is = jdbcTemplate.batchUpdate(sql,batchArgs);
   return is;
}
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext
               ("application.xml");
       BookService service = context.getBean("bookService", BookService.class);
       List<Object[]> list = new ArrayList<>();
       Object[] o1 = {"刘达","500",1};
       Object[] o2 = {"刘达","500",2};
       Object[] o3 = {"刘达","500",3};
       list.add(o1);
       list.add(o2);
       list.add(o3);
       boolean flag = service.findUpdate(list);
        System.out.println(flag == true ? "OK":"NO");
    }
}
```

测试结果：

```
Jun 12, 2023 7:14:11 PM com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
INFO: {dataSource-1} inited
OK
```

![image-20230612191613096](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612191613096.png)

## JdbcTemplate实现批量删除操作

```java
public int[] findDelete(List<Object[]> batchArgs){
   String sql = "delete from spring5demo where user_id = ?";
   int[] is = jdbcTemplate.batchUpdate(sql,batchArgs);
   return is;
}
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
       ApplicationContext context = new ClassPathXmlApplicationContext
               ("application.xml");
       BookService service = context.getBean("bookService", BookService.class);
       List<Object[]> list = new ArrayList<>();
       Object[] o1 = {1};
       Object[] o2 = {2};
       Object[] o3 = {3};
       list.add(o1);
       list.add(o2);
       list.add(o3);
       boolean flag = service.findDelete(list);
        System.out.println(flag == true ? "OK":"NO");
    }
}
```

测试结果：

```
Jun 12, 2023 7:20:00 PM com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
INFO: {dataSource-1} inited
OK
```

![image-20230612192101338](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230612192101338.png)





