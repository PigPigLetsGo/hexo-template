---
title: 二、服务拆分及远程调用
categories:
    - [计算机学科,java,spring,springcloud]
tags:
    - 计算机学科
    - springcloud
---

## 二、服务拆分及远程调用:christmas_tree: 

-  服务拆分
-  服务间调用

### 2.1、服务拆分注意事项:deciduous_tree: 

1.  不同微服务，不要重复开发相同业务
2.  微服务数据独立，不要访问其它微服务的数据库
3.  微服务可以将自己的业务暴漏为接口，供其它微服务调用

![image-20231004140816014](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041408195.png)

前往查看代码：

https://gitee.com/doukaixin/typora/tree/cloud-demo%E4%BB%A3%E7%A0%81%E6%BC%94%E7%A4%BA/

查询的结果：

user

![image-20231004160050762](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041615225.png)

order

![image-20231004160120577](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041615730.png)

>  **总结**：
>
>  1.  微服务需要根据业务模块拆分，做到单一职责，不要重复开发相同业务
>  1.  微服务可以将业务暴漏为接口，供其它微服务使用
>  1.  不同微服务都应该有自己独立的数据库

### 2.2、微服务远程调用:deciduous_tree: 

案例：根据订单id查询订单功能

需求：根据订单id查询订单的同时，把订单所属的用户信息一起返回

![image-20231004154028321](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041615033.png)

#### 2.2.1、远程调用方式分析:evergreen_tree: 

现在user服务对外暴漏了一个RestFull的接口，只要我们在url中输入对应的地址就一定能拿到用户信息

![image-20231004154213334](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041615089.png)

![image-20231004154408769](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041615140.png)

如何在java中发送http请求呢？

#### 2.2.2、操作步骤演示：:evergreen_tree: 

##### 一、注册RestTemplate:palm_tree: 

在order-service的OrderApplication ，SpringBoot启动类中注册RestTemplate

```java
/**
* 创建RestTemplate并注入Spring容器
*/
@Bean
public RestTemplate restTemplate()
{
   return new RestTemplate();
}
```

##### 二、修改方法:palm_tree: 

修改order-service中的OrderService的queryOrderById方法

```java
@Service
public class OrderService {
    @Autowired
    private RestTemplate restTemplate;
   
    public Order queryOrderById(Long orderId) {
        // 1.查询订单
        Order order = orderMapper.findById(orderId);
        // 2.利用RestTemplate发起http请求，查询用户
        // 2.1 url路径
        String url = "http://localhost:8081/user/" + order.getUserId();
        // 2.2 发送请求，实现远程调用
        // 默认返回json数据类型，我们可以指定返回的类型为user对象类型
        User user = restTemplate.getForObject(url, User.class);
        // 3.封装User对象到Order
        order.setUser(user);
        // 4.返回
        return order;
    }
}
```

>  **总结**：
>
>  1.  微服务调用方式
>      -  基于RestTemplate发起的http请求实现远程调用
>      -  http请求做远程调用是与语言无关的调用，只要知道对方的ip，端口，接口路径，请求参数即可

#### 2.2.3、提供者与消费者:deciduous_tree: 

-  服务提供者：一次业务中，被其它微服务调用的服务。(提供接口给其它微服务)
-  服务消费者：一次业务中，调用其它微服务的服务。(调用其它微服务提供的接口)

![image-20231004161322330](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041615963.png)

>  **思考问题**：
>
>  服务A调用服务B，服务B调用服务C，那么服务B是什么角色？
>
>  **答**：
>
>  服务B是什么角色要看相对于谁而言，如果相对于A调B那它就是提供者，相对B调C它又变成了消费者
>
>  因此我们可以认为一个服务它既可以是提供者也可以是消费者

>  **总结**：
>
>  1.  服务调用关系
>      -  服务提供者：暴漏接口给其它微服务调用
>      -  服务消费者：调用其它微服务提供的接口
>      -  提供者与消费者 角色其实是==相对==的
>      -  一个服务可以同时服务提供者和服务消费者

### 2.3、Eureka注册中心:deciduous_tree: 

-  远程调用的问题
-  eureka原理
-  搭建EurekaServer
-  服务注册
-  服务发现

**服务调用出现的问题**。

-  服务消费者该如何获取服务提供者的地址信息？
-  如果有多个服务提供者，消费者该如何选择？
-  消费者如何得知服务提供者的健康状态？

![image-20231004164239712](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041642943.png)

#### 2.3.1、Eureka的作用:evergreen_tree: 

在Eureka的结构当中，分成了两个(概念/角色)。==第一个====角色==就是==Server服务端==它的名字叫做 “==注册中心==” 其作用是：==记录==和==管理这些微服务==.

而另外==第二个角色==就是==user-service服务提供者==和==order-service服务消费者==，不管是==提供者==还是==消费者==都是微服务所以统称为：==Eureka的客户端==.

![image-20231004164706564](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041647816.png)

==user-service服务提供者启动时会将自己的信息注册给Eureka==(每一个服务启动时都会做这件事，只要是Eureka的客户端)

![image-20231004164916779](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041649276.png)

==order-service服务消费者找Eureka去查询一下有没有user-service==，然后==Eureka查询到的话就返回给order-service关于user-service的地址信息==.

![image-20231004165136110](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041651392.png)

现在我们拿到==列表服务的信息==了，这时就由==负载均衡从三个地址信息里面挑出一个然后进行请求==.

![image-20231004165416209](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041654642.png)

那么问题又来了，挑出来的这个会不会是<font color='red'>挂掉</font>的呢？

**答**：不会！==因为user-service服务每隔30秒都会想Eureka发送一次心跳，来确认一下自己的状态，如果它不跳了Eureka就会将user-service从列表中移除掉==.

![image-20231004165753458](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041657023.png)

>  **eureka的作用**.
>
>  -  消费者该如何获取服务提供者具体信息？
>     -  服务提供者启动时向eureka注册自己的信息
>     -  eureka保存这些信息
>     -  消费者根据服务名称向eureka拉取提供者信息
>  -  如果有多个服务提供者，消费者该如何选择？
>     -  服务消费者利用负载均衡算法，从服务列表中挑选一个
>  -  消费者如何感知服务提供者健康状态？
>     -  服务提供者会每隔30秒向EurekaServer发送心跳请求，报告健康状态
>     -  eureka会更新记录服务列表信息，心跳不正常会被剔除
>     -  消费者就可以拉取到最新的信息

>  **总结**：
>
>  在Eureka架构中，微服务角色有两类：
>
>  -  EurekaServer：服务端，注册中心
>     -  记录服务信息
>     -  心跳监控
>  -  EurekaClient：客户端
>     -  provider (提供者)：服务提供者，例如案例中的user-service
>        -  注册自己的信息到EurekaServer
>        -  每隔30秒向EurekaServer发送心跳
>     -  consumer (消费者)：服务消费者，例如案例中的order-service
>        -  根据服务名称从EurekaServer拉取服务列表
>        -  基于服务列表做负载均衡，选中一个微服务后发起远程调用

#### 2.3.2、动手实践:evergreen_tree: 

![image-20231004171046625](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041710804.png)

##### 2.3.2.1 、搭建EurekaServer:palm_tree: 

搭建EurekaServer服务步骤如下：

1、创建项目，引入spring-cloud-starter-netflix-eureka-server的依赖

这里的版本号由父模块进行了统一管理，这里出错了可能是版本与SpringBoot不对应的问题需要降低某一方或升级某一方的版本号

```xml
<dependencies>
   <!--eureka服务端-->
   <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
   </dependency>
</dependencies>
```

2、编写启动类，添加@EnableEurekaServer注解

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@EnableEurekaServer
@SpringBootApplication
public class EurekaServerApplication {
	public static void main(String[] args) {
		SpringApplication.run(EurekaServerApplication.class, args);
	}
}
```

3、添加appliation.yml文件，编写下面的配置：

```yml
server:
  port: 10086 #服务端口
# 为了做服务注册才配的信息
spring:
  application:
    name: eurekaserver #服务名称
eureka:
  client:
    service-url: #eureka地址信息
      defaultZone: http://127.0.0.1:10086/eureka
# 自己就有eureka为什么还要配置地址信息呢？
# 原因：eurka自己也是一个微服务，所以eureka在启动的时候会将自己注册到eureka中
#      这是为了将来eureka集群之间通信去用的，比方说启动了三个eureka将来这三个
#      eureka之间会相互注册，这样它们就可以做数据交流了。所以defaultZone应该
#      配置的是eureka集群的地址如果有多个使用逗号隔开
```

启动进行访问测试是否正常

![image-20231004174930549](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041749630.png)

启动正常。这里面最重要的在于如下：

每个实例的状态

![image-20231004175000766](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041750880.png)

>  **总结**：
>
>  1.  搭建EurekaServer
>      -  引入eureka-server依赖
>      -  添加@EnableEurekaServer注解
>      -  在application.yml中配置eureka地址

##### 2.3.2.2、注册user-service:palm_tree: 

将user-service服务注册到EurekaServer步骤如下：

1、在user-service项目引入spring-cloud-starter-netflix-eureka-client的依赖

```xml
<!--eureka客户端依赖-->
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2、在appliation.yml文件，编写下面的配置：

```yml
server:
  port: 8081
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/cloud_user?useSSL=false&characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: dkx.
    driver-class-name: com.mysql.cj.jdbc.Driver
  application:
    name: userservice # user服务名称
mybatis:
  type-aliases-package: cn.itcast.user.pojo
  configuration:
    map-underscore-to-camel-case: true
logging:
  level:
    cn.itcast: debug
  pattern:
    dateformat: MM-dd HH:mm:ss:SSS
eureka:
  client:
    service-url: # eureka 地址信息
      defaultZone: http://127.0.0.1:10086/eureka
```

操作完成！

如果要将服务消费者注册到eureka中 将同样的方法配置到order-service中然后访问eureka地址

![image-20231004181537727](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041815937.png)

另外，我们可以将user-service多次启动，模拟多实例部署，但为了避免端口冲突，需要修改端口设置：-Dserver.port=8082

![image-20231004190504048](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041905212.png)

设置好后运行user的新服务再次访问就会出现两个实例

![image-20231004192115170](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041921428.png)

>  **总结**：
>
>  1.  服务注册
>      -  引入eureka-client依赖
>      -  在application.yml中配置eureka地址
>  2.  无论是消费者还是提供者，==引入eureka-client依赖==，知道==eureka地址后==，都可以完成==服务注册==.

##### 2.3.2.3、在order-service完成服务拉取:palm_tree: 

服务拉取是基于服务名称获取服务列表，然后在对服务列表负载均衡

1、修改OrderService的代码，修改访问的url路径，用==服务名==代替ip，端口：

```java
String url = "http://userservice/user/" + order.getUserId();
```

2、在order-service项目的启动类OrderApplication中的RestTemplate添加==负载均衡==注解：

```java
@Bean
@LoadBalanced
public RestTemplate restTemplate()
{
   return new RestTemplate();
}
```

访问order地址

![image-20231004193719047](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041937389.png)

在order与user的控制台中就会打印查询的信息

![image-20231004193801716](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041938409.png)

>  **总结**：
>
>  1.  搭建EurekaServer
>      -  ==引入eureka-server依赖==.
>      -  启动类中添加@EnableEurekaServer注解
>      -  ==在application.yml中配置eureka地址==.
>  2.  服务注册
>      -  ==引入eureka-client依赖==.
>      -  ==在application.yml中配置eureka地址==.
>  3.  服务发现/拉取
>      -  ==引入eureka-client依赖==.
>      -  ==在application.yml中配置eureka地址==.
>      -  启动类中的Bean。给RestTemplate添加@LoadBalanced注解
>      -  用服务提供者的服务名称远程调用
