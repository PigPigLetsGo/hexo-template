---
title: spring注解开发
categories:
   - [计算机学科,java,spring]
tags:
   - 计算机学科
   - spring
   - 基础
---

# Spring注解开发

[toc]

## 注解开发定义bean

- 使用@Component定义bean

```java
@SuppressWarnings("all")
//使用注解配置Bean
@Component("bookDao")
public class BookDaoImpl implements BookDao {
    public void save(){
        System.out.println("BookDaoImpl save rning...");
    }
}
```

- 核心配置文件中通过组件扫描加载bean

- base-package:指定包名范围级别越短范围越大,目录递归扫描


```xml
<!--    扫描注解组件 base-package指定到存在的包下即可不用写指定的类-->
    <context:component-scan base-package="com.dkx.spring.dao.impl"/>
<!--    包的全路径为com.dkx.spring.dao.impl，将base-package指定到spring目录下那么spring目录中和子目录都会被递归扫描-->
    <context:component-scan base-package="com.dkx.spring"/>
```

- Spring提供@Component注解的三个衍生注解
    - @Controller:用于表示层(View)bean定义
    - @Service:用于业务层bean定义
    - @Repository:用于数据层(Dao)bean定义

- 这三个注解功能和@Component注解一样,只是在不同的类层使用不同的注解来配置会让我们看到第一时间能想到

```java
@Repository("bookDao")
public class BookDaoImpl implements BookDao{
}
```

```java
@Service
public class BookServiceImpl implements BookService{
}
```

- 如果使用注解配置bean没有写参数名我们需要使用类的类型来进行获取Bean实例化否则报错

- BookService

```java
@Component
public class BookServiceImpl implements BookService {
    public void save(){
        System.out.println("BookServiceImpl save rning...");
    }
}
```

- applicationContext.xml

```xml
<!--    包的全路径为com.dkx.spring.dao.impl，将base-package指定到spring目录下那么spring目录中和子目录都会被递归扫描-->
    <context:component-scan base-package="com.dkx.spring"/>
```

- 测试代码

```java
public void test7(){
   Resource resource = new ClassPathResource("applicationContext.xml");
   BeanFactory bean = new XmlBeanFactory(resource);
   BookService service = bean.getBean(BookService.class);
   service.save();
}
```

### 代码演示

- BookDaoImpl

```java
@SuppressWarnings("all")
//使用注解配置Bean
@Component("bookDao")
public class BookDaoImpl implements BookDao {
    public void save(){
        System.out.println("BookDaoImpl save rning...");
    }
}
```

- applicationContext.xml

```xml
<!--    包的全路径为com.dkx.spring.dao.impl，将base-package指定到spring目录下那么spring目录中和子目录都会被递归扫描-->
    <context:component-scan base-package="com.dkx.spring"/>
```

- 测试代码

```java
    public void test7(){
        Resource resource = new ClassPathResource("applicationContext.xml");
        BeanFactory c = new XmlBeanFactory(resource);
        BookDao dao = c.getBean("bookDao",BookDao.class);
        dao.save();
    }
```

# 纯注解开发

- Spring3.0升级了纯注解开发模式,使用Java类替代配置文件,开启了Spring快速发赛道

- Java类代替Spring核心配置文件

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040859727.png)

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040859925.png)

### @Configuration

-  **作用: 注解用于设定当前类为配置类** 

- <span alt='solid'>注意</span>: @Configuration注解的配置类有如下要求:
    - @Configuration不可以是final类型
    - @Configuration不可以是匿名类
    - 嵌套的Configuration必须是静态类

- 用@Configration加载Spring
    - @Configuration配置Spring并启动Spring容器
    - @Configuration启动容器+@Bean注解Bean
    - @Configuration启动容器+@ComponentScan注解Bean
-  使用@Configuration注解配置的类会遵守Spring单例模式

[点击看案例](./案例/@Configuration的作用.md) 

### @ComponentScan

-  **作用: 注解用于设定扫描Bean路径,此注解只能添加一次,多个数据请用数组格式** 

- 排除扫描Bean

```java
@ComponentScan(value = "com.dkx.spring",
        excludeFilters = @ComponentScan.Filter(
                type = FilterType.ANNOTATION,
                classes = Controller.class
        )
)
```

```java
@ComponentScan({"com.dkx.spring.dao","com.dkx.spring.service"})
```

- 读取Spring核心配置文件初始化容器对象切换为读取Java配置类初始化容器对象

```java
//加载配置文件初始化容器
ApplicationContext c = new ClassPathXmlApplicationContext("applicationContext.xml");
//加载配置类初始化容器
ApplicationContext c = new AnnotationConfigApplicationContext(SpringConfig.class);
```

### @Scope

-  **作用: 设置非单例模式** 

- 设置参数:
    - 单例:singleton 非单例:prototype

- 设置为单例:singleton

[查看代码案例](./code/1.md) 

### 生命周期

#### @PostConstruct

-  **作用:  在构造方法之后执行** 

#### @PreDestroy

-  **作用:  彻底销毁前执行** 

[查看代码案例](./code/2.md) 

### 依赖注入

- **自动装配** 

#### @Autowired

-  **作用:  按类型自动装配Spring bean** 

Spring注解开发是让我们加速开发的,所以它对原始的功能做了一些阉割,有些没有必要的功能就没有做,只做了最快速最好用的功能

<font style="color:red">**注意**</font>:我们不能在直接按原始的方式来settre注入Dao类了这样会报NullPointerException异常的

<font style="color:red">**注意**</font>:自动装配基于反射设计创建对象并暴力反射对应属性为私有属性初始化数据,因此无需提供setter方法

<font style="color:red">**注意**</font>:自动装配建议使用无参构造方法创建对象(默认),如果不提供对应无参构造方法则会报错BeanCreateException,请提供唯一的构造方法

<font style="color:red">**注意**</font>:避免自动装配两个名称相同的Dao或者Service否则会发生数据不一致报错,使用<font style="color:green">**@Qualifier**</font>可以指定要自动装配的是哪个bean

[查看代码案例](./code/3.md) 

#### @Qualifier

- **注意事项:  该注解必须依赖@Autowired注解,无法单独使用** 

通过使用@Qualifier注解,我们可以消除需要注入哪个bean的问题

[查看代码案例](./code/4.md) 

#### @Value

- **作用:  使用@Value实现简单类型注入** 

[查看代码案例](./code/5.md) 

- 载入配置文件来引入Value的值

[查看代码案例](./code/6.md) 

### @PropertySource

- **作用:  用于指定资源文件读取的位置,它不仅能读取properties文件,也能读取xml文件,并且通过YAML解析器,配合自定义PropertySourceFactory实现解析YAML文件** 

- 配置多个值需要使用{}中写值以`,` 号分隔

```
@PropertySource({"a.properties","b.properties","c.properties"})
```

- <font style="color:red">**需要注意**</font>此方式不支持使用**`classpath*:*`**来读取配置文件的位置,可使用`classpath:` 

[查看代码案例](./code/7.md) 

### 管理第三方bean

#### @Bean

- 使用独立的配置类管理第三方bean,比如下面的案例(druid案例)

- 加载druid配置

[查看代码案例](./code/8.md) 

#### @Import

- 使用@Import注解手动加入配置类到核心配置,此注解只能添加一次,多个数据请用数据格式
    - @Import({a.class,b.class,c.class})

### 第三方bean注入资源

**==简单类型依赖注入==和==引用类型依赖注入==** 

[查看代码案例](./code/9.md) 

### 注解开发总结

[点击查看](./code/10.md) 

### @Transactional

- 注解式事务

- 可以在方法中或类中,如果定义为类上,那么这个类整个方法都将开启事务

- 将该注解添加到业务层接口中,降低代码的耦合度

使用步骤:

- 在JdbcConfig中类定义PlaformTransactionManager的Bean方法(设置事务管理器)

### @EnableTransactionManagement

- 在SpringCofing中定义注解`@EnableTransactionManagement`注解来告诉配置类去找事务的注解`@Transaction` (开启注解式事务驱动)
- <font style="color:red">注意:</font>**该注解需要导入的依赖:spring-jdbc** 否则不能导包

[点击查看具体事务操作](./事务/SPring事务简介.md) 

## AOP

### @Aspect

- 配置为AOP

- 位置:定义类上方

### @EnableAspectJAutoProxy

- 开启Spring对AOP注解驱动支持

- 位置:定义SpringConfig类放上

### @Pointcut

切入点

- 语法格式:@Pointcut("execution(返回值 路径.接口名.方法名())")

- `*`: 单个独立的任意符号也可以作为前缀或后缀

- .. : 多个连续的任意符号

- `+`:专用于匹配子类类型

### 绑定切入点

#### @Before

- 前置通知

#### @After

- 后置通知

#### @Around

- 环绕通知

- AOP有返回值类型方法,需要在参数列表中定义:ProceedingJoinPoint j使用j来获取原始方法,必须设置为参数列表的第一个参数

#### @AfterReturning

- 返回后通知

#### @AfterThrowing

- 抛出异常后通知

### AOP通知获取数据

- JoinPoint:适用于前置,后置,返回后,异常后通知,设置为方法参数列表的第一个参数否则会报错
    - JoinPoint,对象描述了连接点方法的运行状态,可以获取到原始的方法调用参数

[查看具体内容](./AOP简介.md) 

## SpringMvc

### @Controller

- 类型:类注解

- 位置:SpringMvc控制器类定义上方

- 作用:设定SpringMvc的核心控制器bean

### @RequestMapping

- 类型:方法注解

- 位置:SpringMvc控制器方法定义上方

- 作用:设置当前控制器方法请求访问路径

- 相关属性
    - value(默认):请求访问路径

### @RequestParam

- 问题:如果发送请求参数的key与我们接收参数的参数名不相同则接收不到参数的值

- 类型:形参注解

- 位置:SpringMvc控制器方法形参定义前面

- 作用:绑定请求参数与处理器方法形参间的关系

- 参数:
    - required:是否为必传参数
    - defaultValue:参数默认值

### @EnableWebMvc

- 类型:配置类注解

- 位置:SpringMvc配置类定义上方

- 作用:开启SpringMvc多项辅助功能

- 建议:每次创建SpringMvc配置类时都加上

### @RequestBody

- 类型:形参注解

- 位置:SpringMvc控制器方法形参定义前面

- 作用:将请求中请求体所包含的数据传递给请求参数,此注解一个处理器方法只能使用一次

### @DateTimeFormat

- 类型:形参注解

- 位置:SpringMvc控制器方法形参前面

- 作用:设定日期时间型数据格式

- 属性:pattern:日期时间格式字符串

- <font style="color:red">**使用形式: 入参**</font> 

[查看具体](./SpringMVC/SpringMVC简介.md) 

### @JsonFormat

- 类型:形参注解

- 位置:SpringMvc控制器方法形参前面

- 作用:设定日期时间型数据格式

- 属性:pattern:日期时间格式字符串

- <font style="color:red">**使用形式：出参**</font> 

[查看具体](./code/12.md) 

## REST风格

### @RequestMapping

增加项

- 属性:
    - method:http请求动作,标准动作(GET/POST/PUT/DELETE)

### @PathVariable

- 类型:形参注解

- 位置:SpringMvc控制器方法形参定义前面

- 作用:绑定路径参数与处理器方法形参间的关系,要求路径参数名与形参名一一对应

### @RestController

- 类型:类注解

- 位置:基于SpringMvc的RESTful开发控制器类定义上方

- 作用:设置当前控制器类为RESTful风格,等同于@Controller与@ResponseBody两个注解组合功能

### 简化开发注解

<font style="color:red">**@GetMapping , @PostMapping , @PutMapping , @DeleteMapping**</font> 

- 类型:方法注解

- 位置:基于SpringMvc的RESTful开发控制器方法定义上方

- 作用:设置当前控制器方法请求访问路径与请求动作,每种对应一个请求动作,例如:@GetMapping对应GET请求

- 属性
    - value(默认):请求访问路径 

## SSM整合

### 异常处理器

#### @RestControllerAdvice

- 类型:类注解

- 位置:Rest风格开发的控制器增强类定义上方

- 作用:为Rest风格开发的控制器类做增强

- 说明:
    - 此注解自带@ResponseBody注解与@Component注解,具备对应的功能

#### @ExceptionHandler

- 类型:方法注解

- 位置:专用于异常处理的控制器方法上方

- 作用:设置指定异常的处理方案,功能等同于控制器方法,出现异常后终止原始控制器执行,并转入当前方法执行

- 说明:
    - 此类方法可以根据处理的异常不用,制作多个方法分别处理对应的异常

