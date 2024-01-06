---
title: 五、http客户端Feign
categories:
    - [计算机学科,java,spring,springcloud]
tags:
    - 计算机学科
    - springcloud
---

## 五、http客户端Feign:christmas_tree: 

-  Feign替代RestTemplate
-  自定义配置
-  Feign使用优化
-  最佳实践

### 5.1、RestTemplate方式调用存在的问题:deciduous_tree: 

先来看我们以前利用RestTemplate发起远程调用的代码：

```java
// 2.利用RestTemplate发起http请求，查询用户
// 2.1 url路径
String url = "http://userservice/user/" + order.getUserId();
// 2.2 发送请求，实现远程调用
// 默认返回json数据类型，我们可以指定返回的类型为user对象类型
User user = restTemplate.getForObject(url, User.class);
```

<font color='red'>上面存在下面的问题</font>：

-  代码可读性差，编程体验不统一
-  参数复杂URL难以维护

### 5.2、Feign的介绍:deciduous_tree: 

Feign是一个声明式的http客户端，官方地址：https://github.com/OpenFeign/feign

其作用就是帮助我们优雅的实现http请求的发送，解决上面提到的问题。

![image-20231006155457781](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061557922.png)

### 5.3、定义和使用Feign客户端:deciduous_tree: 

使用Feign的步骤如下：

1、引入依赖：

```xml
<!--feign 客户端依赖-->
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

2、在order-service的启动类添加注解开启Feign的功能：

```java
@EnableFeignClients
@MapperScan("cn.itcast.order.mapper")
@SpringBootApplication
public class OrderApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderApplication.class, args);
    }
/*
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate()
    {
        return new RestTemplate();
    }
*/
  /*  @Bean
    public IRule randomRule()
    {
        return new RandomRule();
    }*/
}
```

![image-20231006155927233](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061559467.png)

3、编写Feign客户端

声明一个远程调用，定义了一个接口叫UserClient这个接口将来封装的就是所有对userservice发起的远程调用。

在接口上添加了注解@FeignClient(“服务名称”)并且指定了服务名称

```java
@FeignClient("userservice")
public interface UserClient {
    @GetMapping("user/{id}")
    User findById(@PathVariable Long id);
}
```

主要是基于SpringMVC的注解来声明远程调用的信息，比如：

-  服务名称：userservice
-  请求方式：GET
-  请求路径：/user/{id}
-  请求参数：Login id
-  返回值类型：User

4、修改order-service中的queryOrderById的方法内容：

```java
@Service
public class OrderService {

    @Autowired
    private OrderMapper orderMapper;
    @Autowired
    private UserClient userClient;

    public Order queryOrderById(Long orderId) {
        // 1.查询订单
        Order order = orderMapper.findById(orderId);
        // 2.用Feign远程调用
        User user = userClient.findById(order.getUserId());
        // 3.封装User对象到Order
        order.setUser(user);
        // 4.返回
        return order;
    }
}
```

这里需要注意的是order-service与user-service是否在同一个namespace中，否则访问就会报错500状态码。

报错具体信息为：

```
Load balancer does not have available server for client: userservice
//翻译
负载均衡器没有客户端可用的服务器:userservice
```

<font color='red'>罪魁祸首</font>.

```yml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        cluster-name: HZ # 集群名称
        # 这个namespace就是罪魁祸首
        # namespace: ed32fa6f-e9e3-4483-b860-7bcd9731d0d9 # 命名空间，填 ID dev环境
        ephemeral: true # 设置是否为临时实例 设置为非临时实例
```

上面的报错中可以先去nacos控制台看一下注册中心的情况这是个好习惯不然就是找错半天。

启动order-service服务和user-service服务访问url：http://localhost:8080/order/101

结果：

![image-20231006164032976](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061640421.png)

再将两个user-service启动然后刷新页面访问10次url：http://localhost:8080/order/101查看idea控制台的打印

8081

![image-20231006165128092](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061651107.png)

8082

![image-20231006165138249](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061651416.png)

8083

![image-20231006165147393](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061651654.png)

都访问到了，说明我们不仅仅实现了远程调用而且还实现了负载均衡。Feign非常的强大底层还集成了负载均衡的功能

>  **总结**：
>
>  Feign的使用步骤
>
>  1.  引入依赖
>  2.  添加@EnableFeignClients注解
>  3.  编写FeignClient接口
>  4.  使用FeignClient中定义的方法代替RestTemplate

### 5.4、自定义Feign配置:deciduous_tree: 

Feign运行自定义配置来覆盖默认配置，可以修改的配置如下：

| 类型                                        | 作用             | 说明                                                   |
| ------------------------------------------- | ---------------- | ------------------------------------------------------ |
| <font color='red'>feign.Logger.Level</font> | 修改日志级别     | 包含四种不同的级别：NONE，BASIC，HEADERS，FULL         |
| feign.codec.Decoder                         | 响应结果的解析器 | http远程调用的结果做解析，例如解析json字符串为java对象 |
| feign.codec.Encoder                         | 请求参数编码     | 将请求参数编码，便于通过http请求发送                   |
| feign.contract                              | 支持的注解格式   | 默认是SpringMVC的注解                                  |
| feign.Retryer                               | 失败重试机制     | 请求失败的重试机制，默认是没有，不过会使用Ribbon的重试 |

一般我们需要配置的就是日志级别。

feign.Logger.Level：

NONE：没有任何日志

BASIC：发起一次http请求时，记录请求是什么时候发的，什么时候结束的，耗时等基本信息

HEADERS：除了请求信息还有请求头和响应体信息

FULL：完整的记录日志

#### 5.4.1、配置Feign日志有两种方式：:evergreen_tree: 

##### 方式一:palm_tree: 

方式一：配置文件方式

1、全局生效

```yml
feign:
  client:
    config:
      default: # 这里default就是全局配置，如果是写服务名称，则是针对某个微服务的配置
        loggerLevel: FULL # 完整日志
```

2、局部生效

```yml
feign:
  client:
    config:
      userservice: # 这里default就是全局配置，如果是写服务名称，则是针对某个微服务的配置
        loggerLevel: FULL # 完整日志
```

启动order-service服务访问utl：http://127.0.0.1:8080/order/101

![image-20231007091133634](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310070911106.png)

##### 方式二:palm_tree: 

配置Feign日志的方式二：java代码方式，需要先声明一个Bean：

```java
public class FeignClientConfiguration {
    @Bean
    public Logger.Level feignLogLevel()
    {
        return Logger.Level.BASIC; # 日志级别
    }
}
```

1、而后如果是全局配置，则把它放到@EnableFeignClients这个注解中：

```java
@EnableFeignClients(defaultConfiguration = FeignClientConfiguration.class)
```

2、 如果是局部配置，则把它放到@FeignClient这个注解中：

```java
@FeignClient(value = "userservice", configuration = FeignClientConfiguration.class)
```

重新启动order-service服务访问utl：http://127.0.0.1:8080/order/101

![image-20231007091445317](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310070914510.png)

<font color='red'>注意</font>：如果配置文件方式与代码方式同时开启的话，它会使用配置文件的方式。

>  **总结**：
>
>  Feign的日志配置：
>
>  1.  方式一是配置文件，feign.client.config.xxx.loggerLevel
>
>      1.  如果xxx是default则代表全局
>      2.  如果xxx是服务名称，例如userservice则代表局限于某服务
>
>  2.  方式二是java代码配置Logger.Level这个Bean
>
>      1.  如果在@EnableFeignClients注解声明则代表全局
>      2.  如果在@FeignClient注解中声明则代表局限于某服务
>
>      -  <font color='red'>注意</font>：这两个注解一个是启动类的，一个是远程调用接口的。

### 5.5、Feign的性能优化:deciduous_tree: 

Feign底层的客户端实现：

-  URLConnection：默认实现，不支持连接池
-  Apache HttpClient：支持连接池
-  OKHttp：支持连接池

因此优化Feign的性能主要包括：

1.  使用连接池代替默认的URLConnection
2.  日志级别，最好用basic或none，因为开日志也会拉低性能

#### 5.5.1、Feign的性能优化-连接池配置:evergreen_tree: 

Feign添加HttpClient的支持：

引入依赖：

```xml
<!--httpClient 的依赖-->
<dependency>
   <groupId>io.github.openfeign</groupId>
   <artifactId>feign-httpclient</artifactId>
</dependency>
```

配置连接池：

```yml
feign:
 httpclient:
   enabled: true # 开启feign对httpClient的支持
   max-connections: 200 # 最大的连接数
   max-connections-per-route: 50 # 每个路径的最大连接数
```

>  **总结**：
>
>  Feign的优化：
>
>  1.  日志级别尽量用basic
>  2.  使用HttpClient或OKHttp代替URLConnection
>      1.  引入feign-httpClient依赖
>      2.  配置文件开启httpClient功能，设置连接池参数

### 5.6、Feign的最佳实践:deciduous_tree: 

#### 5.6.1、方式一:evergreen_tree: 

方式一 (继承)：给消费者的FeignClient和提供者的controller定义统一的父接口作为标准

>  在UserClient接口中使用GetMapping声明远程调用所需要的信息，比如请求方式，参数，路径，返回值等。
>
>  这个接口最终的目的是什么：是让消费者基于这些声明信息发送一次http的请求，而这个请求就会达到userservice服务对应的一个实例上UserController的queryById。我们拿这两个做一个对比：请求方式都是GET，请求路径findById是“user/{id}”，而queryById是 “/{id}”原因是 类上已经有 “/user” 了，方法名不一样不用管。参数一样。
>
>  所以两个方法对比结果是：除了方法名外，其它都一样。而这两个方法的声明是必须要一样的否则报错
>
>  order-service基于UserClient访问UserService而UserClient 中声明了请求方式，请求参数，请求路径等信息。那么order-service就基于这些信息发送http请求，而UserService恰好在接收这个请求。如果UserClient中的findById与UserController中的queryById声明的不一样，比如发送的是post请求而接收的是get请求它们就匹配不到了。

![image-20231007095439949](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310070954683.png)

<font color='red'>问题</font>：既然它们两个一模一样那么就意味着可以做一下抽取

假如定义了一个接口叫UserAPI，现在我想定义一个Feign客户端接口类就不用再写了直接继承UserAPI就好了

还有UserController ，直接实现UserAPI这个接口。就是给Feign客户端和Controller定义统一标准，它俩就可以不用再去写了。

![image-20231007101108979](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071011316.png)

但是这种方式也有一个问题如下是Spring官网给出的一段说明：

简单理解下面的说明就是：一般情况下我们不推荐去共享接口在服务端和客户端之间，它会造成紧耦合

什么是紧耦合呢：两个微服务userservice和orderservice都已经实现相同接口了从api层面都已经耦合了为紧耦合，将来UserAPI中声明变了UserClient与UserController都要跟着变

![image-20231007101211588](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071012804.png)

而且这种方案对SpringMVC不起作用

#### 5.6.2、方式二:evergreen_tree: 

方式二 (抽取)：将FeignClient抽取为独立模块，并且把接口有关的POJO，默认的Feign配置都放到这个模块中，提供给所有消费者使用

解释：

UserController对外暴漏查询用户的接口，有两个微服务order-service和pay-service假如它俩都需要去查询用户，之前的方式是各写各自的UserClient然后都去调UserController查询用户接口，如果说将来微服务越来越多十几个都来调UserController那UserClient就等于写了十多遍了这样就是重复开发了，很麻烦。

![image-20231007102445971](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071024820.png)

所以准备一个feign-api(项目-独立模块) ，它为消费者把Client定义好，接口定义过程中的实体类，Feign的配置它都管了

将来消费者要使用就引依赖就可以了，引入了后直接调用UserController查询用户

![image-20231007102717991](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071027366.png)

>  **总结**：
>
>  Feign的最佳实践：
>
>  1.  让controller和FeignClient继承同一接口
>  2.  将FeignClient，POJO，Feign的默认配置都定义到一个项目中(独立模块) 统一抽取出来打jar包，供所有消费者使用

#### 5.6.3、演示方式二:evergreen_tree: 

##### 5.6.3.1、抽取FeignClient:palm_tree: 

**实现最佳实践方式二的步骤如下**：

1、首先创建一个module，命名为feign-api，然后引入feign的starter依赖

![image-20231007105607975](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071056767.png)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>cn.itcast.demo</groupId>
        <artifactId>cloud-demo</artifactId>
        <version>1.0</version>
    </parent>

    <artifactId>feign-api1</artifactId>
    <packaging>jar</packaging>

    <name>feign-api1</name>
    <url>http://maven.apache.org</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

2、将order-service中编写的UserClient，User，FeignConfiguration都复制到feign-api项目中

![image-20231007110150030](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071101541.png)

3、将order-service中的config，clients，User都删除，在order-service中引入feign-api的依赖

![image-20231007110347974](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071103048.png)

```xml
<!--引入feign-api-->
<dependency>
   <groupId>cn.itcast.demo</groupId>
   <artifactId>feign-api1</artifactId>
   <version>1.0</version>
</dependency>
```

4、修改order-service中的所有与上述三个组件有关的import部分，改成导入feign-api中的包

![image-20231007110622216](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071106503.png)

![image-20231007110644248](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071106713.png)

5、重启测试

但是呢这里细心的可能已经注意到了有地方不对劲启动后也会报错！

报错为：找不到UserClient的Bean

![image-20231007111359869](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071114496.png)

这是为什么呢？

>  原因：
>
>  项目启动SpringBoot启动类扫描包的范围是cn.itcast.order。而UserClient导入的包路径却是cn.itcast.feign.clients.UserClient
>
>  SpringBoot从order包开始扫描而导入依赖的路径从itcast就不一样了，有一个解决办法就是将SpringBoot扫描的范围扩大，但是！！！这是不合理的。不能这样

###### 5.6.3.1.1、如下两种解决方案：:tanabata_tree: 

当定义的FeignClient不在SpringBootApplication的扫描包范围时，这些FeignClient无法使用。有两种方式解决：

方式一：指定FeignClient所在包 (全拿来)

```java
@EnableFeignClients(basePackages = "cn.itcast.feign.clients")
```

方式二：指定FeignClient字节码 (精准打击，推荐使用：用哪个就指定那个)

```java
@EnableFeignClients(clients = {UserClient.class})
```

使用第二种方式解决代码如下：

```java
import cn.itcast.feign.clients.UserClient;
import cn.itcast.feign.config.FeignClientConfiguration;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@EnableFeignClients(clients = UserClient.class, defaultConfiguration = FeignClientConfiguration.class)
@MapperScan("cn.itcast.order.mapper")
@SpringBootApplication
public class OrderApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderApplication.class, args);
    }
}
```

再次启动项目

![image-20231007112615337](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071126898.png)

>  **总结**：
>
>  不同包的FeignClient的导入有两种方式：
>
>  1.  在@EnableFeignClients注解中添加basePackages，指定FeignClient所在的包
>  2.  在@EnableFeignClients注解中添加clients，指定具体FeignClient的字节码
