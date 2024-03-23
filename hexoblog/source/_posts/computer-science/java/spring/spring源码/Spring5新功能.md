---
title: Spring5框架新功能
categories:
   - [计算机学科,java,spring,spring源码]
tags:
   - 计算机学科
   - spring
   - 源码
---

## Spring5框架新功能

### 1.整个Spring5框架的代码基于Java8，运行时兼容Java9，许多不建议使用的类和方法在代码库中删除

### 2.Spring5.0框架自带了通用的日志封装

1.  Spring5移除了Log4jConfigListener，官方建议使用Log4j2
2.  Spring5框架整合Log4j2

第一步  引入jar包

![image-20230613112126209](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613112126209.png)

第二步  创建log4j2.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->
<!--Configuration后面的status用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，可以看到log4j2内部各种详细输出-->
<configuration status="INFO">
    <!--先定义所有的appender-->
    <appenders>
        <!--输出日志信息到控制台-->
        <console name="Console" target="SYSTEM_OUT">
            <!--控制日志输出的格式-->
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </console>
    </appenders>
    <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
    <!--root：用于指定项目的根日志，如果没有单独指定Logger，则会使用root作为默认的日志输出-->
    <loggers>
        <root level="info">
            <appender-ref ref="Console"/>
        </root>
    </loggers>
</configuration>
```

### 3.Spring5框架核心容器支持@Nullable注解

1.  @Nullable注解可以使用在方法，属性，参数中，方法中表示该方法可以返回为空，属性表示可以为空，参数表示值可以为空

    ```java
    //注解用在方法中
    @Nullable
    String getId();
    //注解用在参数中
    public void getId(@Nullable String name,String sex,@Nullable String address){ ... }
    //注解用在属性中
    @Nullable
    private String name;
    ```

### 4.Spring5核心容器支持函数式风格GenericApplicationContext

```java
@Test
public void test1(){
   //函数式风格创建对象
   //创建GenericApplicationContext对象
   GenericApplicationContext context = new GenericApplicationContext();
   //调用refresh方法注册对象
   context.refresh();
   context.registerBean("user",User.class,()-> new User());
   //获取在Spring注册的对象
   User user = (User) context.getBean("user");
   System.out.println(user);
}
```

### 5.Spring5整合JUnit

#### 整合JUnit4

第一步  引入Spring测试相关依赖

![image-20230613144601393](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613144601393.png)

![image-20230613145846005](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613145846005.png)

测试代码：

```java
//单元测试框架
@RunWith(SpringJUnit4ClassRunner.class)
//加载配置文件
@ContextConfiguration("classpath:application.xml")
public class SpringJUnitTest {
    @Autowired
    private UserService userService;
    @Test
    public void test() throws IOException {
        boolean flag = userService.accountMoney();
        System.out.println(flag == true ? "OK":"NO");
    }
}
```

测试结果：

```
Jun 13, 2023 2:56:47 PM org.springframework.test.context.support.AbstractTestContextBootstrapper getDefaultTestExecutionListenerClassNames
INFO: Loaded default TestExecutionListener class names from location [META-INF/spring.factories]: [org.springframework.test.context.web.ServletTestExecutionListener, org.springframework.test.context.support.DirtiesContextBeforeModesTestExecutionListener, org.springframework.test.context.support.DependencyInjectionTestExecutionListener, org.springframework.test.context.support.DirtiesContextTestExecutionListener, org.springframework.test.context.transaction.TransactionalTestExecutionListener, org.springframework.test.context.jdbc.SqlScriptsTestExecutionListener, org.springframework.test.context.event.EventPublishingTestExecutionListener]
Jun 13, 2023 2:56:47 PM org.springframework.test.context.support.AbstractTestContextBootstrapper getTestExecutionListeners
INFO: Using TestExecutionListeners: [org.springframework.test.context.support.DirtiesContextBeforeModesTestExecutionListener@3fee9989, org.springframework.test.context.support.DependencyInjectionTestExecutionListener@73ad2d6, org.springframework.test.context.support.DirtiesContextTestExecutionListener@7085bdee, org.springframework.test.context.transaction.TransactionalTestExecutionListener@1ce92674, org.springframework.test.context.jdbc.SqlScriptsTestExecutionListener@5700d6b1, org.springframework.test.context.event.EventPublishingTestExecutionListener@6fd02e5]
2023-06-13 14:56:50.630 [main] INFO  com.alibaba.druid.pool.DruidDataSource - {dataSource-1} inited
OK
2023-06-13 14:56:54,840 SpringContextShutdownHook WARN Unable to register Log4j shutdown hook because JVM is shutting down. Using SimpleLogger
```

#### 整合JUnit5

第一步   引入JUnit5相关依赖jar包

![image-20230613150341255](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613150341255.png)

![image-20230613150427634](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613150427634.png)

测试代码：

```java
//单元测试框架
@ExtendWith(SpringExtension.class)
//加载配置文件
@ContextConfiguration("classpath:application.xml")
public class Spring5JUnit5Test {
    @Autowired
    private UserService userService;
    @Test
    public void test() throws IOException {
        boolean flag = userService.accountMoney();
        System.out.println(flag == true ? "OK" : "NO");
    }
}
```

![image-20230613150624611](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613150624611.png)

测试结果：

```
Jun 13, 2023 3:09:11 PM org.springframework.test.context.support.AbstractTestContextBootstrapper getDefaultTestExecutionListenerClassNames
INFO: Loaded default TestExecutionListener class names from location [META-INF/spring.factories]: [org.springframework.test.context.web.ServletTestExecutionListener, org.springframework.test.context.support.DirtiesContextBeforeModesTestExecutionListener, org.springframework.test.context.support.DependencyInjectionTestExecutionListener, org.springframework.test.context.support.DirtiesContextTestExecutionListener, org.springframework.test.context.transaction.TransactionalTestExecutionListener, org.springframework.test.context.jdbc.SqlScriptsTestExecutionListener, org.springframework.test.context.event.EventPublishingTestExecutionListener]
Jun 13, 2023 3:09:11 PM org.springframework.test.context.support.AbstractTestContextBootstrapper getTestExecutionListeners
INFO: Using TestExecutionListeners: [org.springframework.test.context.support.DirtiesContextBeforeModesTestExecutionListener@12e61fe6, org.springframework.test.context.support.DependencyInjectionTestExecutionListener@7ee955a8, org.springframework.test.context.support.DirtiesContextTestExecutionListener@1677d1, org.springframework.test.context.transaction.TransactionalTestExecutionListener@48fa0f47, org.springframework.test.context.jdbc.SqlScriptsTestExecutionListener@6ac13091, org.springframework.test.context.event.EventPublishingTestExecutionListener@5e316c74]
2023-06-13 15:09:13.822 [main] INFO  com.alibaba.druid.pool.DruidDataSource - {dataSource-1} inited
OK
2023-06-13 15:09:17,130 SpringContextShutdownHook WARN Unable to register Log4j shutdown hook because JVM is shutting down. Using SimpleLogger
```

### 6.使用一个复合注解替代上面两个注解完成整合

```java
//单元测试框架
//@ExtendWith(SpringExtension.class)
//加载配置文件
//@ContextConfiguration("classpath:application.xml")
@SpringJUnitConfig(locations = "classpath:application.xml")
public class Spring5JUnit5Test {
    @Autowired
    private UserService userService;
    @Test
    public void test() throws IOException {
        boolean flag = userService.accountMoney();
        System.out.println(flag == true ? "OK" : "NO");
    }
}
```

## Spring5WebFlux

![image-20230613151643191](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613151643191.png)

### 1.介绍

![image-20230613152245164](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613152245164.png)

1.WebFlux是Sprting5添加新的模块，用于web开发的，功能和SpringMVC类似的，WebFlux使用当前一种比较流行的响应式编程出现的框架

2.使用传统web框架，比如SpringMVC，这些基于Servlet容器，WebFlux是一种异步非阻塞的框架，异步非阻塞的框架在Servlet3.1以后才支持，核心是基于Reactor的相关API实现的

3.解释什么是异步非阻塞

-  异步和同步
-  非阻塞和阻塞

上面都是针对对象不一样

-  异步和同步针对调用者：调用者发送请求，如果等着对方回应之后才去做其它事情就是同步，如果发送请求之后不等着对方回应就去做其它事情就是异步
-  阻塞和非阻塞针对被调用者：被调用者收到请求之后，做完请求任务之后才给出反馈就是阻塞，收到请求之后马上给出反馈然后再去做事情就是非阻塞

4.WebFlux特点

第一 非阻塞式：在有限资源下，提高系统吞吐量和伸缩性，以Reactor为基础实现响应式编程

第二 函数式编程：Spring5框架基于java8，WebFlux使用java8函数式编程方式实现路由请求

5.比较SpringMVC

![image-20230613155433703](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613155433703.png)

第一  两个框架都可以使用注解方式，都运行在Tomcat等容器中

第二  SpringMVC采用命令式编程，WebFlux采用异步响应式编程

### 2.响应式编程

什么是响应式编程

>  响应式编程是一种面向数据流和变化传播的编程范式，这意味着可以在编程语言中很方便的表达静态或动态的数据流，而相关的计算模型会自动将变化的值通过数据流进行传播。
>
>  电子表格程序就是响应式编程的一个例子，单元格可以包含字面值或类似"=B1+C1"的公式，而包含公式的单元格的值会依据其它单元格的值的变化而变化

电子表格例子：

![image-20230613160621952](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613160621952.png)

Java8及其之前版本

-  提供观察者模式两个类`Observer`和`Observable`。

```java
public class ObserverDemo extends Observable {
    public static void main(String ... args){
        ObserverDemo demo = new ObserverDemo();
        //添加观察者
        demo.addObserver((o,arg)->{
            System.out.println("发生了变化");
        });
        demo.addObserver((o,arg)->{
            System.out.println("观察者通知，准备改变");
        });
        demo.setChanged(); //数据变化
        demo.notifyObservers(); //通知
    }
}
```

执行结果：

```
观察者通知，准备改变
发生了变化
```

#### 3.1响应式编程(Reactor实现)

1.响应式编程操作中，Reactor是满足Reactive规范框架

2.Reactor有两个核心类，Mono和Flux，这两个类实现接口Publisher，提供丰富操作符，Flux对象实现发布者，返回N个元素；Mono实现发布者，返回0或者1个元素

3.Flux和Mono都是数据流的发布者，使用Flux和Mono都可以发出三种数据信号：

==元素值==，==错误信号==，==完成信号== 

错误信号和完成信号都代表终止信号，终止信号用于告诉订阅者数据流结束了，错误信号终止数据流同时把错误信息传递给订阅者

![image-20230613165850914](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613165850914.png)

4.代码演示Flux和Mono

第一步   引入Reactor依赖

```xml
<dependency>
   <groupId>io.projectreactor</groupId>
   <artifactId>reactor-core</artifactId>
   <version>3.1.5.RELEASE</version>
</dependency>
```

第二步  编写代码

```java
public class TestReactor {
    public static void main(String ... args){
        //just方法直接声明
        Flux.just(1,2,3,4,5,6);
        Mono.just(1);
        //其它方法
        //使用数组
        Integer[] ar = {1,2,3,4,5,6};
        Flux.fromArray(ar);
        //使用集合
        List<Integer> list = Arrays.asList(ar);
        Flux.fromIterable(list);
        //使用stream流
        Stream<Integer> stream = list.stream();
        Flux.fromStream(stream);
    }
}
```

5.三种信号特点

-  错误信号和完成信号都是终止信号，不能共存的
-  如果没有发送任何元素值，而是直接发送错误或者完成信号，表示是空数据流
-  如果没有错误信号，没有完成信号，表示是无限数据流

```java
//创建一个异常信号
Flux.error(new Exception());
```

6.调用just或者其它方法只是声明数据流，数据流并没有发出，只有进行订阅之后才会出发数据流，不订阅什么都不会发生的。

```java
//just方法直接声明,调用subscribe订阅使用Lambda表达式输出结果
Flux.just(1,2,3,4,5,6).subscribe(System.out::print);
Mono.just(1).subscribe(System.out::print);
```

7.操作符

对数据流进行一道道操作，称为操作符，比如工厂流水线

第一  map元素映射为新元素

Map操作符的图解

![image-20230613172350334](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613172350334.png)

上面是元素，中间经过map处理得到下方处理好的数据流

第二   flatMap元素映射为流

将三个元素，中间经过flatMap处理映射为下面多个组成的大的数据流比如下面

三个元素为: af，be，c  经过flatMap ->   afbec变成了一个大的数据流

![image-20230613173207967](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613173207967.png)

### 3.Spring5WebFlux执行流程和核心API

SpringWebFlux基于Reactor，默认使用容器是Netty，Netty是高性能的NIO框架，异步非阻塞的框架

1.Netty

-  BIO

![image-20230613174331956](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613174331956.png)

-  NIO

![image-20230613174510265](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613174510265.png)

SpringWebFlux执行过程和SpringMVC相似的。

-  SpringWebFlux核心控制器DispatchHandler ，实现接口WebHandler。
-  接口WebHandler有一个 方法

```java
public interface WebHandler{
   Mono<Void> handle(ServerWebExchange var1);
}
```

![image-20230613175317520](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613175317520.png)

SpringWebFlux里面DispatcherHandler，负责请求的处理

-  HandlerMapping：请求查询到处理的方法
-  HandlerAdapter：真正负责请求处理
-  HandlerResultHandler：响应结果处理

SpringWebFlux实现函数式编程，两个接口：RouterFunction(路由处理)和HandlerFunction(处理函数)

### 4.Spring5WebFlux(基于注解编程模型)

SpringWebFlux实现方式有两种：注解编程模型和函数式编程模型

使用注解编程模型方式，和之前SpringMVC使用相似的，只需要把相关依赖配置到项目中SpringBoot自动配置相关运行容器，默认情况下使用Netty服务器

第一步   创建SpringBoot工程，引入WebFlux依赖

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
```

第二步  配置启动的端口号

```yaml
server:
	port: 81
```

第三步  创建包和相关类

-  创建实体类

```java
//User实体类
@Getter
@Setter
@ToString
@AllArgsConstructor
public class User {
   private String name;
   private String gender;
   private Integer age;
}
```

-  创建接口定义操作方法

```java
//用户操作接口
public interface UserService {
    //根据id查询用户
    Mono<User> getById(int id);
    //查询所有用户
    Flux<User> getAll();
    //添加用户
    Mono<Void> addUser(Mono<User> user);
}
```

-  接口实现类

```java
public class UserServiceImpl implements UserService {
    //创建Map集合存储数据
    private Map<Integer,User> map = new HashMap<>();

    public UserServiceImpl(){
        map.put(1,new User("张三","男",18));
        map.put(2,new User("李四","男",23));
        map.put(3,new User("刘桑","女",22));
    }
    @Override
    public Mono<User> getById(int id) {
        //justEmpty可以为空
        return Mono.justOrEmpty(this.map.get(id));
    }

    @Override
    public Flux<User> getAll() {
        return Flux.fromIterable(this.map.values());
    }

    @Override
    public Mono<Void> addUser(Mono<User> user) {
        return user.doOnNext(person -> {
            int id = map.size()+1;
            map.put(id,person);
        }).thenEmpty(user.empty());
    }
}
```

-  创建Controller

```java
@SuppressWarnings("all")
@Controller
public class UserController {
    @Autowired
    private UserService userService;

    @RequestMapping(value = "/userById" , method = RequestMethod.GET)
    @ResponseBody
    public Mono<User> getById(Integer id){
        return userService.getById(id);
    }

    @RequestMapping(value = "/userAll" , method = RequestMethod.GET)
    @ResponseBody
    public Flux<User> getAll(){
        return userService.getAll();
    }

    @RequestMapping(value = "/userAdd" , method = RequestMethod.POST)
    @ResponseBody
    public Mono<Void> add(@RequestBody User user){
        Mono<User> userMono = Mono.just(user);
        return userService.addUser(userMono);
    }
}
```

启动项目进行测试：

![image-20230613194152658](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613194152658.png)

```java
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.8.RELEASE)

2023-06-13 19:35:22.707  INFO 10916 --- [           main] demo.Spring5demowebFluxApplication       : Starting Spring5demowebFluxApplication on Administrantor-Dkx with PID 10916 (E:\Idea.2021.2.2\Demo\spring5demowebFlux\target\classes started by Maggie in E:\Idea.2021.2.2\Demo\spring5demowebFlux)
2023-06-13 19:35:22.713  INFO 10916 --- [           main] demo.Spring5demowebFluxApplication       : No active profile set, falling back to default profiles: default
2023-06-13 19:35:27.863  INFO 10916 --- [           main] o.s.b.web.embedded.netty.NettyWebServer  : Netty started on port(s): 81
2023-06-13 19:35:27.869  INFO 10916 --- [           main] demo.Spring5demowebFluxApplication       : Started Spring5demowebFluxApplication in 6.147 seconds (JVM running for 9.365)
```

访问接口：

![image-20230613194232258](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613194232258.png)

![image-20230613194246703](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613194246703.png)

-  说明：
   -  SpringMVC方式实现，同步阻塞的方式，基于SpringMVC+Servlet+Tomcat
   -  SpringWebFlux方式实现，异步非阻塞，基于SpringWebFlux+Reactor+Netty

### 5.Spring5WebFlux(基于函数式编程模型)

1.在使用函数式编程模型操作时候，需要自己初始化服务器

2.基于函数式编程模型时候，有两个核心接口：RouterFunction(实现路由功能，请求转发给对应的handler)和HandlerFunction(处理请求生成相应的函数)，核心任务定义两个函数式接口的实现并且启动需要的服务器

3.SpringWebFlux请求和相应不再是ServletRequest和ServletResponse，而是ServerRequest和ServerResponse

第一步  把注解编程模型工程复制一份并删除Controller层

![image-20230613200019588](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613200019588.png)

第二步   创建Handler(具体实现方法)

```java
public class UserHandler {
    private final UserService userService;
    public UserHandler(UserService userService){
        this.userService = userService;
    }
    //根据id查询用户
    public Mono<ServerResponse> getUserById(ServerRequest request){
        //获取id值
        int userId = Integer.valueOf(request.pathVariable("id"));
        //空值处理
        Mono<ServerResponse> notFound = ServerResponse.notFound().build();
        //调用service方法的到数据
        Mono<User> userMono = this.userService.getById(userId);
        //把userMono进行转换返回
        //使用Reactor操作符flatMap
        return userMono.flatMap(person ->
                ServerResponse.ok().contentType(MediaType.APPLICATION_JSON).body
                        (fromObject(person))).switchIfEmpty(notFound);
    }
    //查询所有用户
    public Mono<ServerResponse> getAllUsers(ServerRequest request){
        //调用service得到结果
        Flux<User> users = this.userService.getAll();
        return ServerResponse.ok().contentType(MediaType.APPLICATION_JSON).body(users,User.class);
    }
    //添加用户
    public Mono<ServerResponse> saveUser(ServerRequest request){
        //得到User对象
        Mono<User> userMono = request.bodyToMono(User.class);
        return ServerResponse.ok().build(this.userService.addUser(userMono));
    }
}
```

第三步   初始化服务器，编写Router

![image-20230613212943433](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613212943433.png)

-  创建路由方法

目录结构：

![image-20230613214338048](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613214338048.png)

```java
public class Server {
    //创建Router路由
    public RouterFunction<ServerResponse> rotingFunction(){
        //创建handler对象
        UserService userService = new UserServiceImpl();
        UserHandler handler = new UserHandler(userService);
        //设置路由
        return RouterFunctions.route(GET("/userById")
                .and(accept(APPLICATION_JSON)),handler::getUserById)
                .andRoute(GET("/userAll").and(accept(APPLICATION_JSON)),handler::getAllUsers);
    }
}
```

-  创建服务完成适配

```java
//创建服务器完成适配
public void createReactorServer(){
   //路由和handler适配
   RouterFunction<ServerResponse> serverResponseRouterFunction = rotingFunction();
   HttpHandler handler = toHttpHandler(serverResponseRouterFunction);
   ReactorHttpHandlerAdapter adapter = new ReactorHttpHandlerAdapter(handler);
   //创建服务器
   HttpServer httpServer = HttpServer.create();
   httpServer.handle(adapter).bindNow();
}
```

-  最终调用

```java
public static void main(String ... args) throws IOException {
   Server server = new Server();
   server.createReactorServer();
   System.out.println("enter to exit");
   System.in.read();
}
```

-  完整server代码：

```java
public class Server {
    public static void main(String ... args) throws IOException {
        Server server = new Server();
        server.createReactorServer();
        System.out.println("enter to exit");
        System.in.read();
    }

    //创建Router路由
    public RouterFunction<ServerResponse> rotingFunction(){
        //创建handler对象
        UserService userService = new UserServiceImpl();
        UserHandler handler = new UserHandler(userService);
        //设置路由
        return RouterFunctions.route(GET("/userById")
                .and(accept(APPLICATION_JSON)),handler::getUserById)
                .andRoute(GET("/userAll").and(accept(APPLICATION_JSON)),handler::getAllUsers);
    }

    //创建服务器完成适配
    public void createReactorServer(){
        //路由和handler适配
        RouterFunction<ServerResponse> serverResponseRouterFunction = rotingFunction();
        HttpHandler handler = toHttpHandler(serverResponseRouterFunction);
        ReactorHttpHandlerAdapter adapter = new ReactorHttpHandlerAdapter(handler);
        //创建服务器
        HttpServer httpServer = HttpServer.create();
        httpServer.handle(adapter).bindNow();
    }
}
```

-  测试代码：
-  启动server类的main方法-最后一行中有端口号(7621)进行接口的访问，端口号不是固定的是随机的

```java
21:55:18.828 [reactor-http-nio-1] DEBUG reactor.netty.tcp.TcpServer - [id: 0x31bb1aec, L:/0:0:0:0:0:0:0:0:7621] Bound new server
```

访问结果：

![image-20230613220213889](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613220213889.png)

![image-20230613220232199](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230613220232199.png)

4.使用WebClient调用

第一步  启动server类的main方法获取访问路径

```java
22:07:27.448 [main] DEBUG io.netty.buffer.ByteBufUtil - -Dio.netty.allocator.type: pooled
22:07:27.448 [main] DEBUG io.netty.buffer.ByteBufUtil - -Dio.netty.threadLocalDirectBufferSize: 0
22:07:27.448 [main] DEBUG io.netty.buffer.ByteBufUtil - -Dio.netty.maxThreadLocalCharBufferSize: 16384
22:07:27.503 [reactor-http-nio-1] DEBUG reactor.netty.tcp.TcpServer - [id: 0x89d753d8, L:/0:0:0:0:0:0:0:0:8183] Bound new server
```

第二步  将路径填写到webClient中进行访问操作

```java
WebClient webClient = WebClient.create("http://127.0.0.1:8183");
```

完整代码：

```java
public class Client {
    public static void main(String ... args){
        //调用服务器地址
        WebClient webClient = WebClient.create("http://127.0.0.1:8183");
        //根据id查询
        String id = "1";
        //get:请求类型,uri:请求路径，值
        User users = webClient.get().uri("/userById/{id}", id)
                .accept(MediaType.APPLICATION_JSON).
                retrieve().bodyToMono(User.class).block();
        System.out.println(users.getName());
        //查询所有
        Flux<User> results = webClient.get().uri("/userAll")
                .accept(MediaType.APPLICATION_JSON).retrieve().bodyToFlux(User.class);
        results.map(stu -> stu.getName())
                .buffer().doOnNext(System.out::println);
    }
}
```

测试结果：只截取部分

```
22:21:18.840 [reactor-http-nio-4] DEBUG org.springframework.http.codec.json.Jackson2JsonDecoder - [1f2586d6] Decoded [User(name=张三, gender=男, age=18)]
张三
22:21:18.840 [reactor-http-nio-4] DEBUG reactor.netty.resources.PooledConnectionProvider - [id: 0xa194ef27, L:/127.0.0.1:8631 - R:/127.0.0.1:8183] onStateChange(GET{uri=/userById/1, connection=PooledConnection{channel=[id: 0xa194ef27, L:/127.0.0.1:8631 - R:/127.0.0.1:8183]}}, [disconnecting])
22:21:18.841 [reactor-http-nio-4] DEBUG reactor.netty.resources.PooledConnectionProvider - [id: 0xa194ef27, L:/127.0.0.1:8631 - R:/127.0.0.1:8183] Releasing channel
22:21:18.841 [reactor-http-nio-4] DEBUG reactor.netty.resources.PooledConnectionProvider - [id: 0xa194ef27, L:/127.0.0.1:8631 - R:/127.0.0.1:8183] Channel cleaned, now 0 active connections and 1 inactive connections
MonoFlatMapMany
```

