---
title: AOP简介
categories:
   - [计算机学科,java,spring]
tags:
   - 计算机学科
   - spring
   - 基础
---

# AOP简介

- AOP(Aspect Oriented Programming)面向切面编程,一种编程范式,指导开发者如何组织程序结构
    - OOP(Object Oriented Programming)面向对象编程

- 作用:在不惊动原始设计的基础上为其进行功能增强

- Spring理念:无入侵式/无侵入式

## AOP核心概念

![image_2023-02-26-14-39-11](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-14-39-11_20230226171157.png)

- 代理(Proxy):SpringAOP的核心本质是采用代理模式实现的

- 连接点(JoinPoint):程序执行过程中的任意位置,粒度为执行方法,抛出异常,设置变量等
    - 在SpringAOP中,理解为方法的执行

- 切入点(Pointcut):匹配连接点的式子
    - 在SpringAOP中,一个切入点可以只描述一个具体方法,也可以匹配多个方法
        - 一个具体方法:com.itheima.dao包下的BookDao接口中的无形参无返回值的save方法
        - 匹配多个方法:所有的save方法,所有的get开头的方法,所有以Dao结尾的接口中的任意方法,所有带有一个参数的方法

- 通知(Advice):在切入点执行的操作,也就是共性功能
    - 在SpringAOP中,功能最终以方法的形式呈现

- 通知类:定义通知的类

- 切面(Aspect):描述通知与切入点的对应关系

- 目标对象(Taraget):被代理的原始对象称为目标对象

### AOP入门案例思路分析

案例设定:测定接口执行效率

简化设定:在接口执行前输出当前系统时间

开发模式:XML or <font style="color:red">注解</font> 

思路分析:

1.导入坐标(pom.xml)

1.1在我们导入Spring-context的时候里面有个依赖是aop已经帮我们导入了

![image_2023-02-26-15-20-20](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-15-20-20_20230226171219.png)

1.2再导入如下依赖

```xml
      <dependency>
          <groupId>org.aspectj</groupId>
          <artifactId>aspectjweaver</artifactId>
          <version>1.9.19</version>
      </dependency>
```

- <font style="color:red">**注意:**</font>将下面的依赖范围给去掉否则不能导包

 < scope> runtime < /scope>

2.制作连接点方法(原始操作,Dao接口与实现类)

- BookDao

```java
@SuppressWarnings("all")
public interface BookDao {
    void save();
    void update();
}
```

- BookDaoImpl

```java
@SuppressWarnings("all")
@Repository
public class BookDaoImpl implements BookDao {
    public void save(){
//        将共性功能抽取出来
        System.out.println(System.currentTimeMillis());
        System.out.println("bookdao save rning...");
    }
    public void update(){
        System.out.println("bookdao update rning...");
    }
}
```

3.制作共性功能

- MyAdvice

```java
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
```

4.定义切入点

@Pointcut("execution(返回值  路径.类名.方法名())")

- MyAdvice

```java
//    配置切入点    执行    返回值    路径...类名.方法名
    @Pointcut("execution(void com.dkx.spring.dao.BookDao.update())")
    private void pt(){}
```

- 说明:切入点定义依托一个不具有实际意义的方法进行,即无参数,无返回值,方法体无实际逻辑

5.绑定切入点与通知关系(切面)

```java
//绑定切入点
@Before("pt()")
//通知
public void method(){
    System.out.println(System.currentTimeMillis());
}
```

常见通知,具体查看<a href="##AOP通知类型">点击查看详细</a>

1. 前置通知 方法执行前调用 对应注解 @Before

2. 后置通知 方法执行后调用 对应注解@After

3. 返回通知 方法返回后调用 对应注解@AfterReturning

4. 异常通知 方法出现异常调用 对应注解@AfterThrowing

5. 环绕通知 动态代理，手动推荐方法运行 对应注解@Around

```java
public class MyAdvice {
//    配置切入点    执行    返回值    路径...类名.方法名
    @Pointcut("execution(void com.dkx.spring.dao.BookDao.update())")
    private void pt(){}
//    绑定切入点与通知关系,并指定通知添加到原始连接点的具体执行位置
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
```

绑定切入点与通知关系,并指定通知添加到原始连接点的具体执行<font style="color:red">**位置**</font>

6.定义通知类受Spring管理器管理,并定义当前类为切面类

@Component

@Aspect

```java
@SuppressWarnings("all")
//配置bean
@Component
//配置为AOP
@Aspect
public class MyAdvice {
//    配置切入点    执行    返回值    路径...类名.方法名
    @Pointcut("execution(void com.dkx.spring.dao.BookDao.update())")
    private void pt(){}
//    绑定切入点与通知关系,并指定通知添加到原始连接点的具体执行位置
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

7.开启Spring对AOP注解驱动支持

@EnableAspectJAutoProxy

```java
@SuppressWarnings("all")
@Configuration
@ComponentScan("com.dkx.spring")
//启动配置中的Aspect
@EnableAspectJAutoProxy
public class SpringConfig {
}
```

## AOP作用

利用AOP对业务逻辑的各个部分进行隔离，降低业务逻辑的耦合性，提高程序的可重用性和开发效率

完整代码


- MyAdvice

```java
@SuppressWarnings("all")
//配置bean
@Component
//配置为AOP
@Aspect
public class MyAdvice {
//    配置切入点    执行    返回值    路径...类名.方法名
    @Pointcut("execution(void com.dkx.spring.dao.BookDao.update())")
    public void pt(){}
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

- SpringConfig

```java
@SuppressWarnings("all")
@Configuration
@ComponentScan("com.dkx.spring")
//启动配置中的Aspect
@EnableAspectJAutoProxy
public class SpringConfig {
}
```

- BookDaoImpl

```java
@SuppressWarnings("all")
@Repository
public class BookDaoImpl implements BookDao {
    public void save(){
//        将共性功能抽取出来
        System.out.println(System.currentTimeMillis());
        System.out.println("bookdao save rning...");
    }
    public void update(){
        System.out.println("bookdao update rning...");
    }
}
```

- 测试代码

```java
@SuppressWarnings("all")
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class UserTest {
    @Test
    public void test(){
        ApplicationContext c = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao dao = c.getBean(BookDao.class);
        dao.update();
    }
}
```

## AOP工作流程

1.Spring容器启动

2.读取所有切面配置中的切入点

```java
//    配置切入点
    @Pointcut("execution(void com.dkx.spring.dao.BookDao.save())")
    private void ptx(){}
//    配置切入点    执行    返回值    路径...类名.方法名
    @Pointcut("execution(void com.dkx.spring.dao.BookDao.update())")
    private void pt(){}
//    绑定切入点与通知关系,并指定通知添加到原始连接点的具体执行位置
    @Before("pt()")
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
```

- 读取pt不会读取ptx

3.初始化bean,判定bean对应的类中的方法是否匹配到任意切入点

- 匹配失败,创建对象

- 匹配成功,创建原始对象(<font style="color:red">目标对象</font>)的<font style="color:red">代理</font>对象

4.获取bean执行方法

- 获取bean,调用方法并执行,完成操作

- 获取的bean是代理对象时,根据代理对象的运行模式运行原始方法与增强的内容,完成操作

## 核心概念

- 目标对象(Target):原始功能去掉共性功能对应的类产生的对象,这种对象是无法直接完成最终工作的

- 代理(Proxy):目标对象无法直接完成工作,需要对其进行功能回填,通过原始对象的代理对象实现

## SpringAOP本质

代理模式

## AOP切入点表达式

- 切入点:要进行增强的方法

- 切入点表达式:要进行增强的方法的描述方式
    - 描述实现类和接口都是OK的

### 语法格式

- 切入点表达式标准格式:动作关键字(访问修饰符  返回值  包名.类/接口名.方法名(参数)异常名)

```
execution(public User com.dkx.service.UserService.findById(int))
```

- 动作关键字:描述切入点的行为动作,例如execution表示执行到指定切入点

- 访问修饰符:public,private等,可以省略

- 返回值

- 包名

- 类/接口名

- 方法名

- 参数

- 异常名:方法定义中抛出指定异常,可以省略

### 通配符

- 可以使用通配符描述切入点,快速描述
    - `*` :单个独立的任意符号,可以独立出现,也可以作为前缀或后缀的匹配符出现

```
execution(public *.com.dkx.*.UserService.find*(*))
```

配置com.dkx包下的任意包中的UserService类或接口中所有find开头的带有一个参数的方法

- `..` :多个连续的任意符号,可以独立出现,常用于简化包名与参数的书写

```
execution(public User com..UserService.findById(..))
```

匹配com包下的任意包中的UserService类或接口中所有名为findById的方法

- `+` :专用于匹配子类类型

```
execution(* *..*Service+.*(..))
```

### 书写技巧

- <font style="color:red">**所有代码按照标准规范开发,否则以下技巧全部失败**</font> 

- 描述切入点<font style="color:red">**通常描述接口**</font>,而不描述实现类

- 访问控制修饰符针对接口开发均采用public描述(<font style="color:red">**可省略访问控制修饰符描述**</font>)

- 返回值类型对于曾删改类使用精准类型加速匹配, 对于查询类使用`*` 通配快速描述

- <font style="color:red">**包名**</font>书写<font style="color:red">**尽量不使用`..` 匹配**</font>,效率过低,常用`*`做单个包描述匹配,或精准匹配

- <font style="color:red">**接口名**</font>/类名书写名称与模块相关的<font style="color:red">**采用`*`匹配**</font>,例如UserService书写成`*`Service,绑定业务层接口名

- <font style="color:red">**方法名**</font>书写以<font style="color:red">**动词**</font>进行<font style="color:red">**精准匹配**</font>,名词采用`*` 匹配,例如getById书写成getBy`*` ,selectAll书写成selectAll

- 参数规则较为复杂,根据业务方法灵活调整

- 通常<font style="color:red">**不使用异常**</font>作为<font style="color:red">**匹配**</font>规则

## AOP通知类型

- AOP通知描述了抽取的共性功能,根据共性功能抽取的位置不同,最终运行代码时要将其加入到合理的位置

- AOP通知共分为5种类型
    - 前置通知,Before
    - 后置通知,After
    - 环绕通知[^前后都有] (重点),Around
    - 返回后通知(了解),AfterReturning
    - 抛出异常后通知(了解),AfterThrowing

@Around

- 类型:方法注解

- 位置:通知方法定义上方

- 作用:设置当前通知方法与切入点之间的绑定关系,当前通知方法在原始切入点方法前后运行

@AfterThrowing

- 类型：方法注解

- 位置：通知方法定义上方

- 作用：设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法运行抛出异常后执行

- 相关属性：value(默认)：切入点方法名，格式为 类名.方法名（）

### 前置通知

- Before

```java
@SuppressWarnings("all")
//配置bean
@Component
//配置为AOP
@Aspect
public class MyAdvice {
//    配置切入点    执行    返回值    路径...类名.方法名
    @Pointcut("execution(void com.dkx.spring.dao.BookDao.update())")
    private void pt(){}
//    绑定切入点与通知关系,并指定通知添加到原始连接点的具体执行位置
    @Before("pt()")
    public void method(){
        System.out.println("before method rning...");
        System.out.println("after method rning...");
    }
}
```

Run Result

![image_2023-02-26-19-48-44](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-19-48-44_20230227151144.png)

### 后置通知

- After

```java
@SuppressWarnings("all")
//配置bean
@Component
//配置为AOP
@Aspect
public class MyAdvice {
//    配置切入点    执行    返回值    路径...类名.方法名
    @Pointcut("execution(void com.dkx.spring.dao.BookDao.update())")
    private void pt(){}
//    绑定切入点与通知关系,并指定通知添加到原始连接点的具体执行位置
    @After("pt()")
    public void method(){
        System.out.println("before method rning...");
        System.out.println("after method rning...");
    }
}
```

Run Result

![image_2023-02-26-19-49-47](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-19-49-47_20230226204741.png)

### 环绕通知

- Around

```java
@SuppressWarnings("all")
//配置bean
@Component
//配置为AOP
@Aspect
public class MyAdvice {
//    配置切入点    执行    返回值    路径...类名.方法名
    @Pointcut("execution(void com.dkx.spring.dao.BookDao.update())")
    private void pt(){}
//    绑定切入点与通知关系,并指定通知添加到原始连接点的具体执行位置
    @Around("pt()")
    public void method(ProceedingJoinPoint j) throws Throwable {
        System.out.println("before method rning...");
        //表示对原始数据的调用
        j.proceed();
        System.out.println("after method rning...");
    }
}
```

Run Result

![image_2023-02-26-19-53-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-19-53-05_20230226204757.png)

#### @Around注意事项

1. 环绕通知必须依赖形参ProceedingJoinPoint(必须在第一位,否则报错)才能实现对原始方法的调用，进而实现原始方法调用前后同时添加通知

2. 通知中如果未使用ProceedingJoinPoint对原始方法进行调用将跳过原始方法的执行

3. 对原始方法的调用可以不接受返回值，通过方法设置成void即可，如果接收返回值，必须设定为Object类型

4. 原始方法的返回值如果是void类型，通知方法的返回值类型可以设置成void，也可以设置成Object

5. 由于无法预知原始方法运行后是否会抛出异常，因此环绕通知方法必须抛出Throwable对象

### AOP有返回值类型的方法

- Around

- BookDaoImpl

```java
@SuppressWarnings("all")
@Repository
public class BookDaoImpl implements BookDao {
    public int select(){
        return 100;
    }
}
```

```java
@SuppressWarnings("all")
//配置bean
@Component
//配置为AOP
@Aspect
public class MyAdvice {
    //    配置切入点    执行    返回值    路径...类名.方法名
    @Pointcut("execution(* com.dkx.spring.dao.BookDao.select())")
    private void pt1(){}
//    绑定切入点与通知关系,并指定通知添加到原始连接点的具体执行位置
    @Around("pt1()")
    public void method(ProceedingJoinPoint j) throws Throwable {
        System.out.println("before method rning...");
        j.proceed();
        System.out.println("after method rning...");
    }
}
```

- 测试代码

```java
@SuppressWarnings("all")
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class UserTest {
    @Test
    public void test(){
        ApplicationContext c = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao dao = c.getBean(BookDao.class);
        int i = dao.select();
        System.out.println(i);
    }
}
```

Run Result

![image_2023-02-26-20-08-55](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-20-08-55_20230226204813.png)

报错描述:

```
org.springframework.aop.AopInvocationException: Null return value from advice does not match primitive return type for: public abstract int com.dkx.spring.dao.BookDao.select()
```

具体原因:返回值类型不匹配

解决办法,将共性功能方法的返回值更改

这个方法默认返回值是Object

```java
//    绑定切入点与通知关系,并指定通知添加到原始连接点的具体执行位置
    @Around("pt1()")
    public Object method(ProceedingJoinPoint j) throws Throwable {
        System.out.println("before method rning...");
        Integer proceed = (Integer) j.proceed();
        System.out.println("after method rning...");
        return proceed+1;
    }
```

Run Result

![image_2023-02-26-20-11-48](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-20-11-48_20230226204828.png)

如果是void类型的方法我们获取原始数据的只的话就为null,proceed=null

### 原始方法执行完后执行

- AfterReturning

```java
@SuppressWarnings("all")
//配置bean
@Component
//配置为AOP
@Aspect
public class MyAdvice {
    @Pointcut("execution(* com.dkx.spring.dao.BookDao.save())")
    private void p(){}

    @AfterReturning("p()")
    public void method() throws Throwable {
        System.out.println("rning...1");
        System.out.println("rning...2");
    }
}
```

Run Result

![image_2023-02-26-20-41-21](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-20-41-21_20230226204848.png)

### 抛出异常后通知

- AfterThrowing

```java
@SuppressWarnings("all")
//配置bean
@Component
//配置为AOP
@Aspect
public class MyAdvice {
    @Pointcut("execution(* com.dkx.spring.dao.BookDao.save())")
    private void p(){}

    @AfterThrowing("p()")
    public void method() throws Throwable {
        System.out.println("异常了AfterThrowing rning...");
    }
}
```

- BookDaoImpl

```java
@SuppressWarnings("all")
@Repository
public class BookDaoImpl implements BookDao {
    public void save(){
        System.out.println("save rning...");
    }
}
```

Run Result

![image_2023-02-26-20-44-49](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-20-44-49_20230226204903.png)

只执行了原始方法内容并没有执行AfterThrowing注解的方法

我们添加一个异常在原始方法中

```java
    public void save(){
        System.out.println(1/0);
        System.out.println("save rning...");
    }
```

Run Result

![image_2023-02-26-20-46-17](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-20-46-17_20230226204918.png)

AfterThrowing方法被执行

## AOP通知获取数据

### 获取参数

- JoinPoint:适用于前置，后置，返回后，抛出异常后通知,设置为方法的第一个形参,否则报错

- ProceedingJoinPint:适用于环绕通知

### 获取返回值

- 返回后通知

- 环绕通知

### 获取异常

- 抛出异常后通知

- 环绕通知

## AOP通知获取参数数据

- JoinPoint对象描述了连接点方法的运行状态,可以获取到原始的方法调用参数

```java
    @Before("f()")
    public void method1(JoinPoint j){
//        获取传入目标方法的参数对象
        Object[] args = j.getArgs();
        System.out.println(Arrays.toString(args));
        System.out.println("method1 aop running...");
    }
```

- ProceedingJoinPoint是JoinPoint的子类

```java
//    绑定切入点
    @Around("f()")
    public Object method(ProceedingJoinPoint j) throws Throwable {
//        获取传入目标方法的参数对象
        Object[] args = j.getArgs();
        System.out.println(Arrays.toString(args));
//        篡改args中的数据让其原始方法的数据发生改变
        args[0] = "小日子，刘桑";
        Object proceed = j.proceed(args);
        return proceed;
    }
```

## AOP通知获取返回值数据

- 抛出异常后通知可以获取切入点方法中出现的异常信息,使用形参可以接收对应的异常对象

```java
    @AfterReturning(value="f()",returning="j")
    public void method2(Object j){
        System.out.println(j);
        System.out.println("Class:AOP method:method2 ResultType:Object running..."+j);
    }
```

- 环绕通知中可以手工写出对原始方法的调用,得到的结果即为原始方法的返回值

```java
@Around("pt()")
public Object around(ProceedingJoinPoint j)throws Throwable(){
    Object ret = j.proceed();
    return ret;
}
```

## AOP通知获取异常数据

- 抛出异常后通知可以获取切入点方法中出现的异常信息,使用形参可以接收对应的异常对象

```java
    @AfterThrowing(value="f()",throwing="o")
    public void method3(Throwable o){
        System.out.println("Class:Aop methodAnnotation:AfterThrowing Error Running...your show Exception"+":--->"+o);
    }
```

- 抛出异常后通知可以获取切入点方法运行的异常信息,使用形参可以接收运行时抛出的异常信息

```java
    @Around("f()")
    public Object method4(ProceedingJoinPoint j) throws Throwable {
        Object jr = null;
        try {
            jr = j.proceed();
        } catch (Throwable throwable) {
            throwable.printStackTrace();
        }
        return jr;
    }
```



