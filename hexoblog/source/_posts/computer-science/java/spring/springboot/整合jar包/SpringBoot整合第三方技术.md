---
title: SpringBoot整合第三方技术[代码生成]
categories:
   - [计算机学科,java,spring,springboot,整合jar包]
tags:
   - 计算机学科
   - springboot
   - 整合jar包
   - SSM
---

# 整合Junit

SpringBoot中的启动类,这个类起到了配置类的作用这个类在什么位置,它就会把它所在的包以及子包全部扫描一遍,所以说service业务层的bean才会被加载成配置bean

- 启动类

```java
package com.dkx.springboot;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.SpringApplication;

/**
 * Hello world!
 *
 */
@SpringBootApplication
public class App {
    public static void main( String[] args ) {
        SpringApplication.run(App.class,args);
    }
}
```

- service

```java
@SuppressWarnings("all")
public interface BookService {
    public void save();
}
----------------impl--------------------
@SuppressWarnings("all")
@Service
public class BookServiceImpl implements BookService {
    @Override
    public void save(){
        System.out.println("BookService save running...");
    }
}
```

目录结构

![image_2023-03-10-17-00-09](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040853095.png)

## @SpringBootTest

- 类型:测试类注解

- 位置:测试类定义上方

- 作用:设置JUnit加载的SpringBoot启动类

- **相关属性**:
    - classes:设置SpringBoot启动类

**注意事项:** 如果测试类在SpringBoot启动类的包或子包中,可以省略启动类的设置,也就是省略classes的设定

![image_2023-03-10-17-16-04](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040853922.png)

- Test

```java
@SpringBootTest
public class SpringTest {
    @Autowired
    private BookService bookService;
    @Test
    public void test(){
        bookService.save();
    }
}
```

- 测试类会默认加载启动类,初始化Spring环境

- 如果测试类所在的包位置与启动类的不符合那么就找不到启动类了

![image_2023-03-10-17-04-01](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040853139.png)

Run console Result

![image_2023-03-10-17-04-44](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040853150.png)

- 报错原因测试:无法找到启动类

## 指定启动类参数

- @SpringBootTest(classes = Class.class)

- 如果测试类与启动类不是在相同的目录下但是也想用怎么办?

- 解决方式:使用clasees参数来指定启动类

```java
@SpringBootTest(classes = App.class)
public class SpringTest {
    @Autowired
    private BookService bookService;
    @Test
    public void test(){
        bookService.save();
    }
}
---------------------Run console Result----------------------------
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.9)

2023-03-10 17:06:56.257  INFO 11444 --- [           main] com.dkx.springboot1.SpringTest           : Starting SpringTest using Java 11.0.16.1 on Administrantor-Dkx with PID 11444 (started by Administrator in E:\apache-maven-3.6.3\maven-work-space\space-idea01\maven-springboot-demo16)
2023-03-10 17:06:56.261  INFO 11444 --- [           main] com.dkx.springboot1.SpringTest           : No active profile set, falling back to 1 default profile: "default"
2023-03-10 17:06:58.531  INFO 11444 --- [           main] com.dkx.springboot1.SpringTest           : Started SpringTest in 3.637 seconds (JVM running for 7.217)
BookService save running...
```

# SpringBoot整合SSM

- SpringBoot整合Spring(不存在)

- SpringBoot整合SpringMVC(不存在)

## SpringBoot整合MyBatis(主要)

- SpringConfig
    - 导入JdbcConfig
    - 导入MyBatisConfig

- JDBCConfig
    - 定义数据源(加载properties配置项:driver,url,username,pssword)

- MyBatisConfig
    - 定义SqlSessionFactoryBean
    - 定义映射配置

**以上的工作都不用干了**

### 创建步骤

- pom.xml文件中导入myabtis整合springboot的起步依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artfactId>mybatis-spring-boot-starter</artifactId>
        <version>2.2.0</version>
    </dependency>
</dependencies>
```

- MyBatis操作数据库所以需要导入mysql-connector和druid的坐标依赖

## @Mapper

- 作用: **在接口类上添加了@Mapper,在编译之后会生成响应的接口实现类** 

- 添加位置:接口类上面

## @MapperScan

- 作用: **指定要变成实现类的接口所在的包,包下面的所有接口在编译之后都会生成相对应的实现类** 

- 添加位置:在SpringBoot启动类上面添加

**注意事项:** 

SpringBoot不建议使用XML文件配置,MyBatis则有点犯难了,官方推荐使用mybatis-boot-starter与SpringBoot整合

MyBatis官方建议:直接在Mapper类中采用注解的形式操作数据库,通过@MapperScan扫描指定的映射器存放路径,最终不需要加任何注解,也不需要对应的XML文件类配置sql语句

1. 创建新模块,选择Spring初始化,并配置模块相关基础信息

![image_2023-03-10-21-23-08](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040853277.png)

2. 选择当前模块需要使用的技术集(MyBatis,MySQL)

![image_2023-03-10-21-25-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040853042.png)

3. 设置数据源参数

![image_2023-03-10-21-29-22](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040853018.png)

配置数据库连接池

```xml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/demo2?characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: dkx
```

配置读取Mapper.xml文件

```xml
mybatis:
  mapper-locations: classpath:mapper/*.xml
```

**注意事项:**  

SpringBoot版本低于2.4.3(不含),MySQL驱动版本大于8.0时,需要在url连接串配置时区

`jdbc:mysql://localhost:3306/ssm_db?characterEncoding=utf-8&serverTimezone=UTC` 

或在MySQL数据库端配置时区解决此问题

4. 定义数据层接口与映射配置

```java
@SuppressWarnings("all")
@Mapper
public interface BookMapper {
    @Select("select * from book where id = #{id}")
    Book getById(Integer id);
}
```

5. 测试类中注入dao接口,测试功能

```java
@SpringBootTest
public class AppTest {
    @Autowired
    private BookMapper bookMapper;
    @Test
    public void test(){
        Book book = bookMapper.getById(1);
        System.out.println(book);
    }
}
-----------------Run----------------------
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.9)

2023-03-10 21:32:16.874  INFO 13108 --- [           main] com.dkx.springboot.test.AppTest          : Starting AppTest using Java 11.0.16.1 on Administrantor-Dkx with PID 13108 (started by Administrator in E:\apache-maven-3.6.3\maven-work-space\space-idea01\maven-springboot-demo17)
2023-03-10 21:32:16.877  INFO 13108 --- [           main] com.dkx.springboot.test.AppTest          : No active profile set, falling back to 1 default profile: "default"
2023-03-10 21:32:19.793  INFO 13108 --- [           main] com.dkx.springboot.test.AppTest          : Started AppTest in 4.158 seconds (JVM running for 7.305)
2023-03-10 21:32:20.859  INFO 13108 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2023-03-10 21:32:23.551  INFO 13108 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
Book{id=1, type='计算机理论', name='Spring实战', description='Spring入门经典教程'}
2023-03-10 21:32:23.771  INFO 13108 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
2023-03-10 21:32:23.817  INFO 13108 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.

Process finished with exit code 0
```

## 基于SpringBoot的SSM整合案例

1. pom.xml

- <font style="color:red">配置起步依赖,必要的资源坐标(druid)</font>

2. application.yml

- <font style="color:red">设置数据源,端口等</font>


3. 配置类

- <font style="color:red">全部删除</font>

4. dao

- <font style="color:red">设置@Mapper</font>

5. 测试类

6. 页面

- <font style="color:red">放置在resources目录下的static目录中</font>

- <font style="color:red">在resources目录中创建一个index.html页面来作为主页面直接访问地址即可跳转到我们想要访问的页面中</font>

**目录结构** 

![image_2023-03-11-14-41-11](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040853836.png)

[查看代码](https://pigpigletsgo.github.io/computer-science/java/spring/springboot/%E6%95%B4%E5%90%88jar%E5%8C%85/11/)

1. 设置分页拦截器作为Spring管理的Bean

```java
@SuppressWarnings("all")
//配置该类为bean
@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor getMybatisPlusInterceptor(){
//        1.定义MP拦截器
        MybatisPlusInterceptor mpinterceptor = new MybatisPlusInterceptor();
//        2.添加具体拦截器
        mpinterceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return mpinterceptor;
    }
}
```

2. 执行分页查询

```java
	@Test
	void testGetByPage(){
		IPage page = new Page(1,2);
		userMapper.selectPage(page,null);
		System.out.println("当前页数为: "+page.getCurrent());
		System.out.println("一共多少页: "+page.getPages());
		System.out.println("每页展示的数据: "+page.getSize());
		System.out.println("一共多少条数据: "+page.getTotal());
		System.out.println("数据: "+page.getRecords());
	}
--------------------------RUN--------------------------
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.9)

2023-03-11 22:22:33.306  INFO 9448 --- [           main] c.m.MavenMybstisplusDemoApplicationTests : Starting MavenMybstisplusDemoApplicationTests using Java 11.0.16.1 on Administrantor-Dkx with PID 9448 (started by Administrator in E:\apache-maven-3.6.3\maven-work-space\space02\maven-mybstisplus-demo)
2023-03-11 22:22:33.316  INFO 9448 --- [           main] c.m.MavenMybstisplusDemoApplicationTests : No active profile set, falling back to 1 default profile: "default"
Logging initialized using 'class org.apache.ibatis.logging.stdout.StdOutImpl' adapter.
Registered plugin: 'com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor@1a96d94c'
Property 'mapperLocations' was not specified.
 _ _   |_  _ _|_. ___ _ |    _ 
| | |\/|_)(_| | |_\  |_)||_|_\ 
     /               |         
                        3.4.1 
2023-03-11 22:22:43.693  INFO 9448 --- [           main] c.m.MavenMybstisplusDemoApplicationTests : Started MavenMybstisplusDemoApplicationTests in 12.737 seconds (JVM running for 17.761)
Creating a new SqlSession
SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@339f3a55] was not registered for synchronization because synchronization is not active
2023-03-11 22:22:45.905  INFO 9448 --- [           main] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} inited
JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@70c29356] will not be managed by Spring
==>  Preparing: SELECT COUNT(*) FROM plus
==> Parameters: 
<==    Columns: COUNT(*)
<==        Row: 5
<==      Total: 1
==>  Preparing: SELECT id,name,password,age,tel FROM plus LIMIT ?
==> Parameters: 2(Long)
<==    Columns: id, name, password, age, tel
<==        Row: 1, Tom888, tom888, 3, 12987345
<==        Row: 2, Jerry, jerry, 4, 1237568324
<==      Total: 2
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@339f3a55]
当前页数为: 1
一共多少页: 3
每页展示的数据: 2
一共多少条数据: 5
数据: [Plus(id=1, name=Tom888, password=tom888, age=3, tel=12987345), Plus(id=2, name=Jerry, password=jerry, age=4, tel=1237568324)]
2023-03-11 22:22:49.400  INFO 9448 --- [ionShutdownHook] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} closing ...
2023-03-11 22:22:49.418  INFO 9448 --- [ionShutdownHook] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} closed

Process finished with exit code 0
```

![image_2023-03-12-10-12-51](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854039.png)

## 开启日志

```yml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatisplus?characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: dkx
#开启MP的日志(输出到控制台)
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

## DQL编程控制

### 条件查询--设置查询条件

- MyBatisPlus将书写复杂的SQL查询条件进行了封装,使用编程的形式完成查询条件的组合

![image_2023-03-12-10-16-18](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854190.png)

- **格式一**: 常规格式

```java
//        第一种方式:按条件查询,特殊情况才使用
        QueryWrapper q = new QueryWrapper();
//        lt:小于
        q.lt("age",18);
        List<Plus> list = plusMapper.selectList(q);
//        for(Plus i:list){
//            System.out.println(i);
//        }
```

- **格式二**: 链式编程格式

```java
        QueryWrapper<Plus> qo = new QueryWrapper();
        qo.lt("age",10).gt("age",30);
        List<Plus> listo = plusMapper.selectList(qo);
//        for(Plus i:listo){
//            System.out.println(i);
//        }
```

- **格式三**: lambda格式(推荐)

```java
//        第二种方式:lambda格式按条件查询
        QueryWrapper<Plus> q1 = new QueryWrapper();
        q1.lambda().lt(Plus::getAge,18).gt(Plus::getAge,30);
        List<Plus> list1 = plusMapper.selectList(q1);
//        for(Plus i:list1){
//            System.out.println(i);
//        }
```

- **格式四**: lambda格式(推荐)

```java
//        查询多个条件
        LambdaQueryWrapper<Plus> q3 = new LambdaQueryWrapper();
//        q3.lt(Plus::getAge,10);
//        q3.gt(Plus::getAge,30);
//        或者使用链式编程
//        10到30之间
//        q3.lt(Plus::getAge,10).gt(Plus::getAge,30);
//        小于10或大于30
        q3.lt(Plus::getAge,30).or().gt(Plus::getAge,10);
        List<Plus> list3 = plusMapper.selectList(q3);
        for(Plus i:list3){
            System.out.println(i);
        }
```

- 并且(and)

```java
        LambdaQueryWrapper<Plus> l = new LambdaQueryWrapper();
//        查询10到30之间的数据
        l.lt(Plus::getAge,10).gt(Plus::getAge,30);
        List<Plus> listl = plusMapper.selectList(l);
        for(Plus i:listl){
            System.out.println(i);
        }
```

- 或者(or)

```java
        LambdaQueryWrapper<Plus> l = new LambdaQueryWrapper();
//        查询小与30或者大于10的信息
        l.lt(Plus::getAge,30).or().gt(Plus::getAge,10);
        List<Plus> listl = plusMapper.selectList(l);
        for(Plus i:listl){
            System.out.println(i);
        }
```

### 条件查询--null值处理

![image_2023-03-12-12-39-26](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854501.png)

- 通常情况下我们只会设置一部分不会每一个都设置 

- if语句控制条件追加

```java
        LambdaQueryWrapper<Plus> q = new LambdaQueryWrapper();
//        通过if判断getAge1是否为null如果为null则不执行 q.gt(Plus::getAge,query.getAge1());
        if(null != query.getAge()){
            q.lt(Plus::getAge,query.getAge());
        }
//        通过if判断getAge1是否为null如果为null则不执行 q.gt(Plus::getAge,query.getAge1());
        if(null != query.getAge1())
            q.gt(Plus::getAge,query.getAge1());
        List<Plus> list = plusMapper.selectList(q);
        for(Plus i:list){
            System.out.println(i);
        }
```

- 条件参数控制(链式编程)

```java
        LambdaQueryWrapper<Plus> q = new LambdaQueryWrapper();
//        先判断第一个参数是否为true,如果为true连接当前条件,false则不连
        q.lt(null != query.getAge(),Plus::getAge,query.getAge())
//        先判断第一个参数是否为true,如果为true连接当前条件,false则不连
        .gt(null != query.getAge1(),Plus::getAge,query.getAge1());
        List<Plus> list = plusMapper.selectList(q);
        for(Plus i:list){
            System.out.println(i);
        }
```

### 查询投影

- 查询结果包含模型类中部分属性

```java
//        查询投影
        LambdaQueryWrapper<Plus> q = new LambdaQueryWrapper();
        q.select(Plus::getId,Plus::getName,Plus::getAge);
        List<Plus> list = plusMapper.selectList(q);
//        for(Plus i:list){
//            System.out.println(i);
//        }
        QueryWrapper q1 = new QueryWrapper();
        q1.select("id","name","age");
        List<Plus> list1 = plusMapper.selectList(q1);
//        for(Plus i:list1){
//            System.out.println(i);
//        }
---------------------RUN---------------------
Plus(id=1, name=张三, password=null, age=3, tel=null)
Plus(id=2, name=张三, password=null, age=4, tel=null)
Plus(id=3, name=Jock, password=null, age=41, tel=null)
Plus(id=4, name=小日子-刘桑, password=null, age=15, tel=null)
Plus(id=1634728724109651970, name=小六, password=null, age=321, tel=null)

Process finished with exit code 0
```

- 查询结果包含模型类中 未定义的属性

```java
        QueryWrapper q2 = new QueryWrapper();
//        相当于select ... from ? ...
        q2.select("count(*) as size","name");
//        分组
        q2.groupBy("name");
        List<Map<String,Object>> map = plusMapper.selectMaps(q2);
        for(Map<String,Object> i:map){
            System.out.println(i);
        }
-------------------------RUN-------------------------
{size=2, name=张三}
{size=1, name=Jock}
{size=1, name=小日子-刘桑}
{size=1, name=小六}
```

### 查询条件

- 范围匹配(>,=,between)

- 模糊匹配(like)

- 空判定(null)

- 包含型匹配(in)

- 分组(group)

- 排序(order)

- ...

- 用户登入(eq匹配)

```java
        LambdaQueryWrapper<Plus> q3 = new LambdaQueryWrapper();
//        eq等同于 =
//        select * from plus where name = "张三" and password = "法外狂徒";
        q3.eq(Plus::getName,"Jock").eq(Plus::getPassword,"123456");
//        因为eq条件查询出来的有两个张三和法外狂徒我们使用selectOne只输出一个结果的方法时就会报错
        Plus plus = plusMapper.selectOne(q3);
//        System.out.println(plus);
```

- 购物设定价格区间,户籍设定年龄区间(le ge匹配 或 between匹配)

```java
        LambdaQueryWrapper<Plus> q4 = new LambdaQueryWrapper();
//        范围查询lt小于,le小于等于,gt大于,ge大于等于,eq等于
//        查询范围 min --> max
//      方案一:设定上限下限
        q4.le(Plus::getAge,10).ge(Plus::getAge,30);
//      方案二:设定范围
        q4.between(Plus::getAge,10,30);
        List<Plus> list2 = plusMapper.selectList(q4);
        for(Plus i:list2){
            System.out.println(i);
        }
```

- 查信息,搜索新闻(非全文检索版:like匹配)

```java
        LambdaQueryWrapper<Plus> q5 = new LambdaQueryWrapper();
        q5.likeLeft(Plus::getName,"三");
        List<Plus> list3 = plusMapper.selectList(q5);
        for(Plus i:list3){
            System.out.println(i);
        }
```

- 统计报表(分组查询聚合函数)

```java
        QueryWrapper q2 = new QueryWrapper();
//        相当于select ... from ? ...
        q2.select("count(*) as size","name");
//        分组
        q2.groupBy("name");
        List<Map<String,Object>> map = plusMapper.selectMaps(q2);
        for(Map<String,Object> i:map){
            System.out.println(i);
        }
```

## 字段映射与表名映射

![image_2023-03-12-22-03-36](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854461.png)

### 问题一:表字段与编码属性设计不同步

![image_2023-03-12-22-05-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854388.png)

### @TableField

- 类型:属性注解

- 位置:模型类属性定义上方

- 作用:设置当前属性对应的数据库表中的字段关系

- 范例:

```java
public class User{
    @TableField(value="pwd")
    private String password;
}
```

- 相关属性
    - value(默认):设置数据库表字段名称

### 问题二:编码中添加了数据库中未定义的属性

![image_2023-03-12-22-11-10](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854427.png)

解决方式

![image_2023-03-12-22-12-33](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854347.png)

### @TableField

- 类型:属性注解

- 位置:模型类属性定义上方

- 作用:设置当前属性对应的数据库表中的字段关系

- 范例:

```java
public class User{
    @TableField(exist = false)
    private Integer online;
}
```

- 相关属性
    - value:设置数据库表字段名称
    - <font style="color:red">exist:设置属性在数据库表字段中是否存在,默认为true,此属性无法与value合并使用</font> 

### 问题三:采用默认查询开放了更多的字段查看权限

```sql
select id,name,pwd,age,tel,speciality from user
```

-  可能会导致我们原本不想让密码的数据d

![image_2023-03-12-22-17-26](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854894.png)

- 设置字段不参与查询

### @TableField设置字段不参与查询

- 类型:属性注解

- 位置:模型类属性定义上方

- 作用:设置当前属性对应的数据库表中的关系

- 范例:

```java
public class User{
    @TableField(value="pwd",select=false)
    private String password;
}
```

- 相关属性:
    - value:设置数据库表字段名称
    - exist:设置属性在数据库表字段中是否存在,默认为true,此属性无法与value合并使用
    - <font style="color:red">select:设置属性是否参与查询,此属性与select()映射配置不冲突 </font>
    

### 问题四:表名与编码开发设计不同步

![image_2023-03-12-22-21-46](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854031.png)

- 解决方案:

![image_2023-03-12-22-22-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854739.png)

### @TableName

- 类型:类注解

- 位置:模型类定义上方

- 作用:设置当前类对应与数据库表关系

- 范例:

```java
@TableName("tbl_user")
public class User{
    private Long id;
}
```

- 相关属性
    - value:设置数据库表名称

## DML编程控制

**id生成策略控制** 

- 不同的表应用不同的id生成策略
    - 日志:自增(1,2,3,4, ...)
    - 购物订单:特殊规则(FQ2398798AK82)
    - 外卖单:关联地区日期等信息(10 04 20200314 34 91)
    - 关系表:可省略id
    -  ...

### @TableId

- 类型:属性注解

- 位置:模型类中用于表示主键的属性定义上方

- 作用:设置当前类中注解属性的生成策略

- 范例:

```java
public class User{
    @TableId(type = IdType.AUTO)
    private Long id;
}
```

- 相关属性
    - value:设置数据库主键名称
    - type:设置主键属性的生成策略,值参照IdType枚举值

### AUTO

范例:

```java
public class User{
    @TableId(type = IdType.AUTO)
    private Long id;
}
```

使用数据id自增策略控制id生成

**使用sql命令:alter table [表名] auto_increment = n 来设定一下自增的起步值** 

![image_2023-03-13-10-44-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854987.png)

**测试类** 

```java
    @Test
    void insertTest(){
        Plusa p = new Plusa();
        p.setName("刘桑-c");
        p.setPwd("789");
        p.setAge(29);
        p.setTel("3123");
        int i = plusMapper.insert(p);
        System.out.println(i > 0 ? "执行成功":"执行失败");
    }
----------------------------RUN----------------------------
执行成功
----------------------------MYSQL----------------------------
mysql> select * from plus;
+----+-------------+----------------+-----+--------------+
| id | name        | password       | age | tel          |
+----+-------------+----------------+-----+--------------+
|  1 | 张三        | 法外狂徒       |   3 | 1122         |
|  2 | 张三        | 法外狂徒       |   4 | 2233         |
|  3 | Jock        | 123456         |  41 | 867862342    |
|  4 | 小日子-刘桑 | it-小日子-刘桑 |  15 | 912837893759 |
|  5 | 刘桑-a      | 123            |  30 | 3123         |
+----+-------------+----------------+-----+--------------+
```

### INPUT

范例:

```java
public class User{
    @TableId(type = IdType.INPUT)
    private Long id;
}
```

用户手工输入id,如果没有自增条件而又不输入id则报错null

![image_2023-03-13-11-03-52](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854625.png)

用户手工输入id

**测试代码** 

```java
    @Test
    void insertTest(){
        Plusa p = new Plusa();
        p.setId(10L);
        p.setName("刘桑-c");
        p.setPwd("789");
        p.setAge(29);
        p.setTel("3123");
        int i = plusMapper.insert(p);
        System.out.println(i > 0 ? "执行成功":"执行失败");
    }
----------------------------RUN----------------------------
执行成功
----------------------------MYSQL----------------------------
mysql> select * from plus;
+----+-------------+----------------+-----+--------------+
| id | name        | password       | age | tel          |
+----+-------------+----------------+-----+--------------+
|  1 | 张三        | 法外狂徒       |   3 | 1122         |
|  2 | 张三        | 法外狂徒       |   4 | 2233         |
|  3 | Jock        | 123456         |  41 | 867862342    |
|  4 | 小日子-刘桑 | it-小日子-刘桑 |  15 | 912837893759 |
|  5 | 刘桑-a      | 123            |  30 | 3123         |
|  6 | 刘桑-b      | 456            |  31 | 3123         |
|  7 | 刘桑-c      | 789            |  29 | 3123         |
|  8 | 刘桑-c      | 789            |  29 | 3123         |
|  9 | 刘桑-c      | 789            |  29 | 3123         |
| 10 | 刘桑-c      | 789            |  29 | 3123         |
+----+-------------+----------------+-----+--------------+
```

如果有自增值约束那么就不会报错会根据自增的起步值自增

`alter table plus modify id bigint auto_increment;` 

![image_2023-03-13-11-09-17](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854642.png)

**测试代码** 

```java
    @Test
    void insertTest(){
        Plusa p = new Plusa();
        p.setName("刘桑-c");
        p.setPwd("789");
        p.setAge(29);
        p.setTel("3123");
        int i = plusMapper.insert(p);
        System.out.println(i > 0 ? "执行成功":"执行失败");
    }
--------------------------RUN--------------------------
执行成功
--------------------------MYSQL--------------------------
mysql> select * from plus;
+----+-------------+----------------+-----+--------------+
| id | name        | password       | age | tel          |
+----+-------------+----------------+-----+--------------+
|  1 | 张三        | 法外狂徒       |   3 | 1122         |
|  2 | 张三        | 法外狂徒       |   4 | 2233         |
|  3 | Jock        | 123456         |  41 | 867862342    |
|  4 | 小日子-刘桑 | it-小日子-刘桑 |  15 | 912837893759 |
|  5 | 刘桑-a      | 123            |  30 | 3123         |
|  6 | 刘桑-b      | 456            |  31 | 3123         |
|  7 | 刘桑-c      | 789            |  29 | 3123         |
|  8 | 刘桑-c      | 789            |  29 | 3123         |
|  9 | 刘桑-c      | 789            |  29 | 3123         |
+----+-------------+----------------+-----+--------------+
```

### NONE

不设置id生成策略

自动生成主键,使用雪花算法

范例:

```java
public class User{
    @TableId(type = IdType.NONE)
    private Long id;
}
```

**测试代码** 

```java
mysql> select * from plus;
+---------------------+-------------+----------------+-----+--------------+
| id                  | name        | password       | age | tel          |
+---------------------+-------------+----------------+-----+--------------+
| 1635120592362332161 | 刘桑-c      | 789            |  29 | 3123         |
+---------------------+-------------+----------------+-----+--------------+
```

### ASSIGN_ID

雪花算法生成id(可兼容数值与字符串型)

雪花算法:生成一个串,串由64位二进制组成最终得到一个Long值

![image_2023-03-13-11-37-50](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040854878.png)

范例:

```java
public class User{
    @TableId(type = IdType.ASSIGN_ID)
    private Long id;
}
```

**测试代码** 

```java
    @Test
    void insertTest(){
        Plusa p = new Plusa();
//        p.setId(12L);
        p.setName("刘桑-c");
        p.setPwd("789");
        p.setAge(29);
        p.setTel("3123");
        int i = plusMapper.insert(p);
        System.out.println(i > 0 ? "执行成功":"执行失败");
    }
-----------------------RUN-----------------------
执行成功
-----------------------MYSQL-----------------------
mysql> select * from plus;/
+---------------------+-------------+----------------+-----+--------------+
| id                  | name        | password       | age | tel          |
+---------------------+-------------+----------------+-----+--------------+
| 1635119016822366210 | 刘桑-c      | 789            |  29 | 3123         |
+---------------------+-------------+----------------+-----+--------------+
```

### ASSIGN_UUID

以UUID生成算法作为id生成策略

范例:

```java
public class User{
    @TableId(type = IdType.ASSIGN_UUID)
    private Long id;
}
```

### yml中设置全局配置id生成策略

```yml
mybatis-plus:
#  使用 全局配置id生成策略
  global-config:
    db-config:
#      这条语句等同于@TableId(type = IdType.AUTO)
      id-type: auto
#      配置全局实体类名前缀让其与数据库表名称保持一致
#      table-prefix: tab_
```

- id生成策略全局配置

![image_2023-03-13-12-28-49](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855882.png)

- 表名前缀全局配置

![image_2023-03-13-12-28-18](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855003.png)

### 多记录操作

![image_2023-03-13-12-32-22](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855473.png)

**方法** 

- 按照主键删除多条记录

<font style="color:red">deleteBatchIds</font>

- 根据主键查询多条记录

<font style="color:red">selectBatchIds</font>

- 该方法接收的是List集合,来进行多条记录操作

**测试类** 

#### Delete

```java
    @Test
    void deleteTest(){
        List<Long> list = new ArrayList<>();
        list.add(4L);
        list.add(5L);
        list.add(6L);
        list.add(7L);
        int i = plusMapper.deleteBatchIds(list);
        System.out.println(i > 0 ? "执行成功":"执行失败");
    }
---------------------------RUN---------------------------
执行成功
---------------------------MYSQL---------------------------

---------------------------MYSQL--Before---------------------------
mysql> select * from plus;
+----+-------------+----------------+-----+--------------+
| id | name        | password       | age | tel          |
+----+-------------+----------------+-----+--------------+
|  1 | 张三        | 法外狂徒       |   3 | 1122         |
|  2 | 张三        | 法外狂徒       |   4 | 2233         |
|  3 | Jock        | 123456         |  41 | 867862342    |
|  4 | 小日子-刘桑 | it-小日子-刘桑 |  15 | 912837893759 |
|  5 | 刘桑-a      | 123            |  30 | 3123         |
|  6 | 刘桑-b      | 456            |  31 | 3123         |
|  7 | 刘桑-c      | 789            |  29 | 3123         |
+----+-------------+----------------+-----+--------------+
7 rows in set (0.00 sec)

---------------------------MYSQL--After---------------------------

mysql> select * from plus;
+----+------+----------+-----+-----------+
| id | name | password | age | tel       |
+----+------+----------+-----+-----------+
|  1 | 张三 | 法外狂徒 |   3 | 1122      |
|  2 | 张三 | 法外狂徒 |   4 | 2233      |
|  3 | Jock | 123456   |  41 | 867862342 |
+----+------+----------+-----+-----------+
```

#### Select

```java
    @Test
    void deleteTest(){
        List<Long> list = new ArrayList<>();
        list.add(1L);
        list.add(2L);
        list.add(3L);
        List<Plusa> plusas = plusMapper.selectBatchIds(list);
        for(Plusa i:plusas){
            System.out.println(i);
        }
    }
--------------------------RUN--------------------------
Plusa(id=1, name=张三, pwd=法外狂徒, age=3, tel=1122)
Plusa(id=2, name=张三, pwd=法外狂徒, age=4, tel=2233)
Plusa(id=3, name=Jock, pwd=123456, age=41, tel=867862342)
```

### 逻辑删除

- 删除操作业务问题:业务数据从数据库中丢弃

![image_2023-03-13-13-59-30](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855215.png)

- 逻辑删除:为数据设置是否可用状态字段,删除时设置状态字段为不可用状态,数据保留在数据库中 

![image_2023-03-13-14-26-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855372.png)

操作步骤:

1. 数据库表中添加逻辑删除标记字段

![image_2023-03-13-15-20-43](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855619.png)

2. 实体类中添加对应字段,并设定当前字段为逻辑删除字段

```java
public class User{
    private Long id;
    @TableLogic
    private Integer deleted;
}
```

3. 配置逻辑删除字面值

```yml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      logic-delete-field: deleted
      #1 代表已经删除
      logic-delete-value: 1
      #0 代表没有删除
      logic-not-delete-value: 0
```

**一旦把逻辑删除功能开启后下面就要注意了** 

原来执行的删除语句就变成了一个更新语句:

执行的java代码

```java
    @Test
    void deleteTest1(){
        int i = plusMapper.deleteById(1L);
        System.out.println(i > 0 ? "删除成功":"删除失败");
    }
```

```sql
Preparing: UPDATE plus SET deleted=1 WHERE id=? AND deleted=0
```

**执行数据结果** 

```
mysql> select * from plus;
+----+------+----------+-----+-----------+---------+
| id | name | password | age | tel       | deleted |
+----+------+----------+-----+-----------+---------+
|  1 | 张三 | 法外狂徒 |   3 | 1122      |       1 |
|  2 | 张三 | 法外狂徒 |   4 | 2233      |       1 |
|  3 | Jock | 123456   |  41 | 867862342 |       0 |
+----+------+----------+-----+-----------+---------+
3 rows in set (0.00 sec)
```

- domain

```java
@SuppressWarnings("all")
@Data
//设置数据库表的名称,解决实体类与数据库表名不一致情况
@TableName("plus")
public class Plusa {
//    @TableId(type = IdType.NONE)
    private Long id;
    private String name;
//    设置数据库表的字段名称,解决字段名不一致,select=false该字段不参与查询
    @TableField(value="password",select=true)
    private String pwd;
    private Integer age;
    private String tel;
//    是否在线
    @TableField(exist = false)
    private Integer online;
//    逻辑删除字段:标记当前记录是否被删除
    @TableLogic(value = "0",delval = "1")
    private Integer deleted;
}
```

- test

```java
    @Test
    void deleteTest1(){
        int i = plusMapper.deleteById(2L);
        System.out.println(i > 0 ? "删除成功":"删除失败");
    }
------------------------------RUN------------------------------
执行成功
------------------------------MYSQL------------------------------
------------------------------Before------------------------------
mysql> select * from plus;
+----+------+----------+-----+-----------+---------+
| id | name | password | age | tel       | deleted |
+----+------+----------+-----+-----------+---------+
|  1 | 张三 | 法外狂徒 |   3 | 1122      |       0 |
|  2 | 张三 | 法外狂徒 |   4 | 2233      |       0 |
|  3 | Jock | 123456   |  41 | 867862342 |       0 |
+----+------+----------+-----+-----------+---------+
3 rows in set (0.00 sec)

------------------------------After------------------------------

mysql> select * from plus;
+----+------+----------+-----+-----------+---------+
| id | name | password | age | tel       | deleted |
+----+------+----------+-----+-----------+---------+
|  1 | 张三 | 法外狂徒 |   3 | 1122      |       0 |
|  2 | 张三 | 法外狂徒 |   4 | 2233      |       1 |
|  3 | Jock | 123456   |  41 | 867862342 |       0 |
+----+------+----------+-----+-----------+---------+
```

- 执行一下查询看看deleted为1的数据是否会被查询出来

```java
    @Test
    void selectTest1(){
        List<Plusa> list = plusMapper.selectList(null);
        for(Plusa i:list){
            System.out.println(i);
        }
    }
----------------------------RUN----------------------------
Plusa(id=1, name=张三, pwd=法外狂徒, age=3, tel=1122, online=null, deleted=0)
Plusa(id=3, name=Jock, pwd=123456, age=41, tel=867862342, online=null, deleted=0)
==>  Preparing: SELECT id,name,password AS pwd,age,tel,deleted FROM plus WHERE deleted=0
```

**要注意使用deleted而不是delete会报错因为这是个关键字** 

**注意事项:** 

不要查询deleted逻辑删除字段为1的因为被判定为删除查询时deleted = 0导致查询不出结果

## 乐观锁

**注意事项:** 

不要查询deleted逻辑删除字段为1的因为被判定为删除查询时deleted = 0导致查询不出结果

1. 数据库表中添加锁标记字段

![image_2023-03-13-18-34-18](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855713.png)

2. 实体类中添加对应字段,并设定当前字段为逻辑删除标记字段

```java
@SuppressWarnings("all")
//设置数据库表的名称,解决实体类与数据库表名不一致情况
@TableName("plus")
@Data
public class Plusa {
//    @TableId(type = IdType.NONE)
    private Long id;
    private String name;
//    设置数据库表的字段名称,解决字段名不一致,select=false该字段不参与查询
    @TableField(value="password",select=true)
    private String pwd;
    private Integer age;
    private String tel;
//    是否在线
    @TableField(exist = false)
    private Integer online;
//    逻辑删除字段:标记当前记录是否被删除
//    @TableLogic(value = "0",delval = "1")
    private Integer deleted;
    @Version
    private Integer version;
}
```

3. 配置乐观锁拦截器实现锁机制对应的动态SQL语句拼装

```java
@SuppressWarnings("all")
@Configuration
public class MPConfig {
    @Bean
    public MybatisPlusInterceptor getMybatisPlusInterceptor(){
        MybatisPlusInterceptor m = new MybatisPlusInterceptor();
        m.addInnerInterceptor(new PaginationInnerInterceptor());
        m.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return m;
    }
}
```

4. 测试代码

```java
    @Test
    void updateTest(){
        Plusa p = plusMapper.selectById(3L);//version = 2
        Plusa p1 = plusMapper.selectById(3L);//更新失败,因为当这个执行的时候version已经不是之前的了被修改过了

        p.setName("张三aaa");
        int i = plusMapper.updateById(p);
        System.out.println(i > 0 ? "更新成功":"更新失败");

        p1.setName("张三bbb");
        int i1 = plusMapper.updateById(p1);
        System.out.println(i1 > 0 ? "更新成功":"更新失败");
    }
-----------------------------RUN-----------------------------
JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@325162e9] will not be managed by Spring
==>  Preparing: SELECT id,name,password AS pwd,age,tel,deleted,version FROM plus WHERE id=? AND deleted=0
==> Parameters: 3(Long)
<==    Columns: id, name, pwd, age, tel, deleted, version
<==        Row: 3, 李四, 123456, 41, 867862342, 0, 2
<==      Total: 1
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@23121d14]
Creating a new SqlSession
SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@6a8a551e] was not registered for synchronization because synchronization is not active
JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@325162e9] will not be managed by Spring
==>  Preparing: UPDATE plus SET name=?, password=?, age=?, tel=?, version=? WHERE id=? AND version=? AND deleted=0
==> Parameters: 张三aaa(String), 123456(String), 41(Integer), 867862342(String), 3(Integer), 3(Long), 2(Integer)
<==    Updates: 1
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@6a8a551e]
更新成功
Creating a new SqlSession
SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@407b8435] was not registered for synchronization because synchronization is not active
JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@325162e9] will not be managed by Spring
==>  Preparing: UPDATE plus SET name=?, password=?, age=?, tel=?, version=? WHERE id=? AND version=? AND deleted=0
==> Parameters: 张三bbb(String), 123456(String), 41(Integer), 867862342(String), 3(Integer), 3(Long), 2(Integer)
<==    Updates: 0
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@407b8435]
更新失败

Process finished with exit code 0
-----------------------------MYSQL-----------------------------
-----------------------------Before-----------------------------
mysql>  select * from plus;
+----+---------+----------+-----+-----------+---------+---------+
| id | name    | password | age | tel       | deleted | version |
+----+---------+----------+-----+-----------+---------+---------+
|  1 | 张三    | 法外狂徒 |   3 | 1122      |       1 |       1 |
|  2 | 张三    | 法外狂徒 |   4 | 2233      |       1 |       1 |
|  3 | 张三aaa | 123456   |  41 | 867862342 |       0 |       2 |
+----+---------+----------+-----+-----------+---------+---------+
3 rows in set (0.00 sec)
-----------------------------After-----------------------------
mysql>  select * from plus;
+----+---------+----------+-----+-----------+---------+---------+
| id | name    | password | age | tel       | deleted | version |
+----+---------+----------+-----+-----------+---------+---------+
|  1 | 张三    | 法外狂徒 |   3 | 1122      |       1 |       1 |
|  2 | 张三    | 法外狂徒 |   4 | 2233      |       1 |       1 |
|  3 | 张三aaa | 123456   |  41 | 867862342 |       0 |       3 |
+----+---------+----------+-----+-----------+---------+---------+
3 rows in set (0.00 sec)
```

5. 使用乐观锁机制在修改前必须先获取到对应数据的version方可正常进行

```java
    @Test
    void arTest(){
//        先通过id查询出一行数据,也获取到version的数据
        Plusa p = plusMapper.selectById(3L);
        p.setName("刘桑");
//        再进行修改的操作此时对象中setName对name进行了修改而查询中也查出一行的数据其中 包含了version没有version就会报错
        int i = plusMapper.updateById(p);
        System.out.println(i > 0 ? "更新成功":"更新失败");
    }
------------------------------------RUN------------------------------------
JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@2365ea38] will not be managed by Spring
==>  Preparing: SELECT id,name,password AS pwd,age,tel,deleted,version FROM plus WHERE id=? AND deleted=0
==> Parameters: 3(Long)
<==    Columns: id, name, pwd, age, tel, deleted, version
<==        Row: 3, 张三aaa, 123456, 41, 867862342, 0, 3
<==      Total: 1
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@6df4af5]
Creating a new SqlSession
SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@7cd5fcf4] was not registered for synchronization because synchronization is not active
JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@2365ea38] will not be managed by Spring
==>  Preparing: UPDATE plus SET name=?, password=?, age=?, tel=?, version=? WHERE id=? AND version=? AND deleted=0
==> Parameters: 刘桑(String), 123456(String), 41(Integer), 867862342(String), 4(Integer), 3(Long), 3(Integer)
<==    Updates: 1
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@7cd5fcf4]
更新成功

Process finished with exit code 0
```

**执行修改前执行查询语句:** 

```
==>  Preparing: SELECT id,name,password AS pwd,age,tel,deleted,version FROM plus WHERE id=? AND deleted=0
```

**执行修改时使用version字段作为乐观锁检查依据** 

```
==>  Preparing: UPDATE plus SET name=?, password=?, age=?, tel=?, version=? WHERE id=? AND version=? AND deleted=0
```

- 每个人拿到的锁是不一样的,只有自己的那个版本操作完后让版本叠加, 下一个人才可以进行操作

### 需要注意的两点

![image-20230313185146185](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855207.png)

## 快速开发

### 代码生成器

比如造句:

![image_2023-03-13-18-56-08](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855274.png)

用它原始提供的信息加自己填入的内容是不是就可以组成一个新的句子

![image_2023-03-13-19-22-26](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855294.png)

![image_2023-03-13-19-24-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855897.png)

![image_2023-03-13-19-23-16](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855187.png)

![image_2023-03-13-19-24-52](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040855022.png)

- 模板:MyBatisPlus提供

- 数据库相关配置:读取数据库获取信息

- 开发者自定义配置:手工配置

### 代码生成器

```xml
<!--    模板引擎-->
    <dependency>
      <groupId>org.apache.velocity</groupId>
      <artifactId>velocity-engine-core</artifactId>
      <version>2.3</version>
    </dependency>
<!--    代码生成器-->
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-generator</artifactId>
      <version>3.4.1</version>
    </dependency>
```

- 创建代码生成器核心代码类

```java
import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.DataSourceConfig;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;
import com.baomidou.mybatisplus.generator.config.PackageConfig;
import com.baomidou.mybatisplus.generator.config.StrategyConfig;
import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;

@SuppressWarnings("all")
public class CodeGet {
    public static void main(String[] args) {
        //创建代码生成器对象
        AutoGenerator autogenerator = new AutoGenerator();
        DataSourceConfig d = new DataSourceConfig();
        d.setDriverName("com.mysql.cj.jdbc.Driver");
        //记得更改自己的数据信息否则会报错
        d.setUrl("jdbc:mysql://localhost:3306/?characterEncoding=utf-8&serverTimezone=UTC&useUnicode=true");
        d.setUsername("");
        d.setPassword("");
        d.setDbType(DbType.MYSQL);
        autogenerator.setDataSource(d);
        //设置全局配置
        GlobalConfig globalConfig = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        System.out.println(projectPath);
        globalConfig.setOutputDir(projectPath+"\\src\\main\\java");
        globalConfig.setOpen(false);//设置生成完毕后是否打开生成代码所在位置
        globalConfig.setAuthor("dkx");//设置作者
        globalConfig.setFileOverride(true);//设置是否覆盖原始生成的文件
        globalConfig.setMapperName("%sMapper");//设置数据层接口名 %s为占位符,指代模块名称
        globalConfig.setServiceName("%sService");
        globalConfig.setIdType(IdType.AUTO);//设置Id生成策略
        autogenerator.setGlobalConfig(globalConfig);
        //设置包名相关配置
        PackageConfig p = new PackageConfig();
        p.setParent("com.dkx");//设置生成的包名,与代码所在位置不冲突,二者叠加组成完整路径
        p.setModuleName("system");//模块名
        p.setController("controller");
        p.setEntity("domain");//设置实体类包名
        p.setService("service");
        p.setMapper("mapper");//设置数据层包名
        autogenerator.setPackageInfo(p);

        StrategyConfig s = new StrategyConfig();
        //指定生成哪个表
        // s.setInclude("");
        // 表名的生成策略：下划线转驼峰 pms_product -- PmsProduct
        s.setNaming(NamingStrategy.underline_to_camel);
        // 列名的生成策略： 下划线转驼峰 last_name -- lastName
        s.setColumnNaming(NamingStrategy.underline_to_camel);
        s.setTablePrefix("tbl_");//设置当前参与生成的表名,参数为可变参数
        s.setRestControllerStyle(true);//设置数据库表的前缀名称,模块名=数据库表名-前缀名
        s.setVersionFieldName("version");//设置乐观锁字段名
        s.setLogicDeleteFieldName("deleted");//设置逻辑删除字段名
        s.setEntityLombokModel(true);//设置是否启用lombok
        autogenerator.setStrategy(s);
        autogenerator.execute();
    }
}
```

### 代码生成可能遇到的问题

报错信息：

![image-20230906212420817](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309062124108.png)

解决方式：

![image-20230906212435239](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309062124408.png)

去掉对钩即可解决。
