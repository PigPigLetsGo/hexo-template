---
title: 六、统一网关Gateway
categories:
    - [计算机学科,java,spring,springcloud]
tags:
    - 计算机学科
    - springcloud
---

## 六、统一网关Gateway:christmas_tree: 

-  为什么需要网关
-  gateway快速入门
-  断言工厂
-  过滤器工厂
-  全局过滤器
-  跨域问题

### 6.1、为什么需要网关:deciduous_tree: 

先看下现在的微服务结构：

我们有很多个不同的服务，每个服务都需要去访问数据库完成自己的业务功能，并且微服务都可以在nacos里面完成服务注册，配置管理。当微服务内部又相互调用关系时，我们就可以利用feign组件去做了。而外部用户需要访问时则让它直接发请求到微服务就行了。

![image-20231007114556885](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071145537.png)

但是！这里其实存在一个问题。

我们微服务直接摆在那里允许任何人发请求来访问是不是不太安全呢？你要知道不是所有业务都对外公开的

所以我们需要对用户的身份进行验证，如果说你是符号条件的人员才能让你进去看相关的内容和操作否则拦住不让进。

那这件事儿就是由网关来做的。

一切请求一定先到网关再到微服务

![image-20231007114955240](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071149540.png)

这就是网关功能之一：身份认证和权限校验

但是还有一个问题。

比如说某用户要做一个用户查询，那么网关能处理用户查询的业务吗？不能！肯定要把请求转发到对应的处理用户查询的服务。比如说userService。因此网关必须做一件事儿了，它需要根据用户请求判断是找userService还是orderService如果弄错了就要出问题了。

而这个动作我们称之为：服务路由，负载均衡

### 6.2、网关功能：:deciduous_tree: 

-  身份认证和权限校验
-  服务路由，负载均衡
-  请求限流

### 6.3、网关的技术实现:deciduous_tree: 

在SpringCloud中网关的实现包括两种：

-  gateway
-  zuul

>  zuul与gateway对比：
>
>  Zuul是基于Servlet的实现，属于阻塞式编程。而SpringCloudGateway则是基于Spring5中提供的WebFlux，属于响应式编程的实现，具备更好的性能。

>  **总结**：
>
>  网关的作用：
>
>  -  对用户请求做身份认证，权限校验
>  -  将用户请求路由到微服务，并实现负载均衡
>  -  对用户请求做限流

### 6.4、搭建网关服务:deciduous_tree: 

搭建网关服务的步骤：

1、创建新的module，引入SpringCloudGateway的依赖和nacos的服务发现依赖：

```xml
<!--网关依赖-->
<dependency>
   <groupId>org.springframework.cloud</groupId>
   <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<!--nacos 服务发现依赖-->
<dependency>
   <groupId>com.alibaba.cloud</groupId>
   <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

2、编写路由配置及nacos地址

```yml
#===========这些配置让网关能够联系上nacos实现服务注册和发现====
server:                                              #||
  port: 10010 # 网关端口                              #||
spring:                                              #||
  application:                                       #||
    name: gateway # 服务名称                           #||
  cloud:                                              #||
    nacos:                                            #||
      server-addr: localhost:8848 # nacos地址          #||
#========================================================
#=========================网关路由配置=====================
    gateway:                                           #||
      routes: # 网关路由配置                              ||
        - id: user-service # 路由id，自定义，只要唯一即可    ||
        # uri 有两种方式的写法                             ||
        # uri: http://127.0.0.1:8081 # 路由的目标地址 http 就是固定地址
          uri: lb://userservice # 路由的目标地址 lb(LoadBalance) 就是负载均衡，后面跟服务名称
          predicates: # 路由断言，也就是判断请求是否符合路由规则的条件
            - Path=/user/** # 这个是按照路径匹配，只要以 /user/ 开头就符合要求
        - id: order-service                            #||
          uri: lb://orderservice                       #||
          predicates:                                  #||
            - Path=/order/**                           #||
#=========================================================
```

访问gateway网关uri：http://localhost:10010/user/1

![image-20231007160105779](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071601031.png)

503的各种原因：

1、注意yml配置文件中的uri配置lb//服务名，是否与nacos中的自定义服务名相同。

-  解释：gateway的路由规则是基于nacos配置中心来定位项目的

2、如果配置文件没问题，Spring Cloud 2020版本报503错误，需要手动添加loadbalancer依赖

-  在当前项目的pom.xml文件中导入如下依赖即可

   ```xml
   <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-loadbalancer</artifactId>
   </dependency>
   ```

再次访问gateway网关uri：http://localhost:10010/user/1

![image-20231007160021450](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071600956.png)

<center>路由过程流程图</center>

![image-20231007161853807](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071618234.png)

微服务都可以注册到nacos注册中心，这时用户发送了请求，请求进入网关但是网关无法处理这个业务。所以它只能是基于路由规则去做判断，而上面我们定义了两个规则一个是把user请求开头的路径代理到userservice，另一个是把order请求开头的路径代理到orderservice。网关拿着userserivce去注册中心拉取服务列表然后做负载均衡，至此流程完毕！

>  **总结**：
>
>  网关搭建步骤：
>
>  1.  创建项目，引入nacos服务发现和gateway依赖
>  2.  配置application.yml，包括服务基本信息，nacos地址，路由
>
>  路由配置包括：
>
>  1.  路由id：路由的唯一标示
>  2.  路由目标(uri)：路由的目标地址，http代表固定地址，lb代表根据服务名负载均衡
>  3.  路由断言(predicates)：判断路由的规则
>  4.  过滤器(filters)：对请求或响应做处理

### 6.5、路由断言工厂Route Predicate Factory:deciduous_tree: 

网关路由可以配置的内容包括：

-  路由id：路由唯一标示
-  uri：路由目的地，支持lb和http两种
-  <font color='red'>predicates：路由断言，判断请求是否符合要求，符合则转发到路由目的地</font>.
-  filters：路由过滤器，处理请求或响应

断言我们知道就是一种判断的规则，那断言工厂又是什么呢？

#### 6.5.1、路由断言工厂Route Predicate Factory:evergreen_tree: 

-  我们在配置文件中写的断言规则只是字符串，这些字符串会被 (断言工厂) Prediacte Factory读取并处理，转变为路由判断的条件

-  例如Path=/user/*** 是按照路径匹配，这个规则是由

   org.springframework.cloud.gateway.handler.predicate.PathRoutePredicateFactory类来处理的

-  像这样的断言工厂在SpringCloudGateway还有十几个，每个都有自己的判断规则和条件

**Spring提供了11种基本的Predicate工厂**：

| 名称       | 说明                          | 示例                                                         |
| ---------- | ----------------------------- | ------------------------------------------------------------ |
| After      | 是某个时间点后的请求          | - After=2037-01-20T17:42:47.789-07:00[America/Denver]        |
| Before     | 是某个时间点之前的请求        | - Before=2031-04-13T15:14:47.433+08:00[Asia/Shanghai]        |
| Between    | 是某两个时间点之前的请求      | - Between=2037-01-20T17:42:47.789-07:00[America/Denver]<br />,2037-01-21T17:42:47.789-07:00[America/Denver] |
| Cookie     | 请求必须包含某些Cookie        | - Cookie=chocolate，ch.p                                     |
| Header     | 请求必须包含某些header        | - Header=X-Request-Id，\d+                                   |
| Host       | 请求必须是访问某个host (域名) | `- Host**.somehost.org，**.anotherhost.org`                  |
| Method     | 请求方式必须是指定方式        | - Method=GET，POST                                           |
| Path       | 请求路径必须符合指定规则      | - Path=/red/{segment}，/blue/**                              |
| Query      | 请求参数必须包含指定参数      | - Query=name，jack或者-Query=name                            |
| RemoteAddr | 请求者的ip必须是指定范围      | - RemoteAddr=192.168.1.1/24                                  |
| Weight     | 权重处理                      |                                                              |

功能演示：

```yml
#===========这些配置让网关能够联系上nacos实现服务注册和发现====
server:                                              #||
  port: 10010 # 网关端口                              #||
spring:                                              #||
  application:                                       #||
    name: gateway # 服务名称                           #||
  cloud:                                              #||
    nacos:                                            #||
      server-addr: localhost:8848 # nacos地址          #||
#========================================================
#=========================网关路由配置=====================
    gateway:                                           #||
      routes: # 网关路由配置                              ||
        - id: user_service # 路由id，自定义，只要唯一即可    ||
        # uri 有两种方式的写法                             ||
        # uri: http://127.0.0.1:8081 # 路由的目标地址 http 就是固定地址
          uri: lb://userservice # 路由的目标地址 lb(LoadBalance) 就是负载均衡，后面跟服务名称
          predicates: # 路由断言，也就是判断请求是否符合路由规则的条件
            - Path=/user/** # 这个是按照路径匹配，只要以 /user/ 开头就符合要求
        - id: order_service                            #||
          uri: lb://orderservice                       #||
          predicates:                                  #||
            - Path=/order/**                           #||
            # 配置断言规则是在2037年之后才能访问
            - After=2037-01-20T17:42:47.789-07:00[America/Denver]
#========================================================
```

==使用Query的方式如下：==

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: test_route
          uri: https://www.baidu.com
          predicates: # 断言规则当我们访问某个url比如：http://localhost:8088/?url=qq 那么此时就会跳转到qq的网址 访问url=baidu就到百度了
            - Query=url,baidu
        - id: qq_route
          uri: https://www.qq.com
          predicates:
            - Query=url,qq
```

访问uri：http://127.0.0.1:10010/order/101

![image-20231007170619279](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071706489.png)

```yml
#===========这些配置让网关能够联系上nacos实现服务注册和发现====
server:                                              #||
  port: 10010 # 网关端口                              #||
spring:                                              #||
  application:                                       #||
    name: gateway # 服务名称                           #||
  cloud:                                              #||
    nacos:                                            #||
      server-addr: localhost:8848 # nacos地址          #||
#========================================================
#=========================网关路由配置=====================
    gateway:                                           #||
      routes: # 网关路由配置                              ||
        - id: user_service # 路由id，自定义，只要唯一即可    ||
        # uri 有两种方式的写法                             ||
        # uri: http://127.0.0.1:8081 # 路由的目标地址 http 就是固定地址
          uri: lb://userservice # 路由的目标地址 lb(LoadBalance) 就是负载均衡，后面跟服务名称
          predicates: # 路由断言，也就是判断请求是否符合路由规则的条件
            - Path=/user/** # 这个是按照路径匹配，只要以 /user/ 开头就符合要求
        - id: order_service                            #||
          uri: lb://orderservice                       #||
          predicates:                                  #||
            - Path=/order/**                           #||
            # 配置断言规则是在2037年之后才能访问
            - Before=2037-01-20T17:42:47.789-07:00[America/Denver]
#========================================================
```

访问uri：http://127.0.0.1:10010/order/101

![image-20231007170751853](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071707263.png)

>  **总结**：
>
>  -  PredicateFactory的作用是什么？
>
>     读取用户定义的断言条件，对请求做判断
>
>  -  Path=/user/** 是什么含义？
>
>     路径是以 /user 开头的就认为是符合的

### 6.6、路由过滤GatewayFilter:deciduous_tree: 

GatewayFilter是网关中提供的一种过滤器，可以对进入网关的==请求==和微服务返回的==响应==做==处理==。

请求：

>  当用户向网关发起请求时，需要先做路由里面会有一个断言工厂它可以基于我们配置的规则完成请求路由去判断到底应该到哪个微服务。但是路由之后也并不能直接向微服务发起请求。因为我们还可以给路由配置各种各样的过滤器

![image-20231007174642784](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071746074.png)

>这些过滤器会形成一个过滤器链。那么用户的请求就一定要经过这些过滤器链，然后才能到达微服务。
>
>那么在这个过程当中过滤器就可以对进入网关的请求做各种处理。比如说对请求头或者请求参数做什么处理都是没问题的。

响应：

>  当用户请求给微服务后，微服务处理完了要返回一个结果。而这个结果肯定也是先到达了网关，网关同样会经过过滤器链来逐层处理这个响应结果，最终才会返回给用户

![image-20231007175153476](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071754515.png)

>  那么在这个过程当中过滤器可以对响应做些什么事儿呢？比如说把响应头检查一下加一些头信息之类的

对请求或响应做出各种各样的处理，具体能做出什么样的处理那就要看Spring里面提供的过滤工厂了

#### 6.6.1、过滤器工厂GatewayFilterFactory:evergreen_tree: 

Spring提供了31种不同的路由过滤器工厂。例如：

| 名称                 | 说明                         |
| -------------------- | ---------------------------- |
| AddRequestHeader     | 给当前请求添加一个请求头     |
| RemoveRequestHeader  | 移除请求中的一个请求头       |
| AddResponseHeader    | 给响应结果中添加一个响应头   |
| RemoveResponseHeader | 从响应结果中移除有一个响应头 |
| RequestRateLimiter   | 限制请求的流量               |
| …                    |                              |

##### 6.6.1.1、代码案例:palm_tree: 

**给所有进入userservice的请求添加一个请求头**.

给所有进入userservice的请求添加一个请求头：Truth=itcast is freaking awesome!

实现方式：

在gateway中修改application.yml文件，给userservice的路由添加过滤器：

```yml
spring:
  cloud:
    gateway:                                          
      routes: # 网关路由配置                              
        - id: user_service # 路由id，自定义，只要唯一即可   
        # uri 有两种方式的写法                             
        # uri: http://127.0.0.1:8081 # 路由的目标地址 http 就是固定地址
          uri: lb://userservice # 路由的目标地址 lb(LoadBalance) 就是负载均衡，后面跟服务名称
          predicates: # 路由断言，也就是判断请求是否符合路由规则的条件
            - Path=/user/** # 这个是按照路径匹配，只要以 /user/ 开头就符合要求
          filters: # 过滤器
            - AddRequestHeader=Truth,Itcast is freaking awesome! # 添加请求头
```

在UserController类中编写queryById使用注解@RequestHeader获取请求头的信息，加上require=false表示没获取到也没有关系，不加会报错。输出请求头的信息

```java
@Slf4j
@RestController
@RequestMapping("/user")
public class UserController {

    @Autowired
    private UserService userService;
@GetMapping("/{id}")
    public User queryById(@PathVariable("id") Long id, @RequestHeader(value = "Truth", required = false) String truth)
    {
        System.out.println("truth: " + truth);
        return userService.queryById(id);
    }
}
```

访问uri：http://localhost:10010/user/1

![image-20231007192552616](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071926883.png)

可以正常访问我们查看idea控制台中user-service的打印有没有请求体信息输出的那句话

![image-20231007192850157](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071928357.png)

可以看到请求头信息被打印出来了。

但是有个问题，我如果想要给全部的微服务都加请求头信息呢，这时可以使用默认过滤器来配置

##### 6.6.1.2、默认过滤器:palm_tree: 

如果要对所有的路由都生效，则可以将过滤器工厂写到default下。格式如下：

```yml
spring:
  cloud:
    gateway:                                          
      routes: # 网关路由配置                              
        - id: user_service # 路由id，自定义，只要唯一即可   
        # uri 有两种方式的写法                             
        # uri: http://127.0.0.1:8081 # 路由的目标地址 http 就是固定地址
          uri: lb://userservice # 路由的目标地址 lb(LoadBalance) 就是负载均衡，后面跟服务名称
          predicates: # 路由断言，也就是判断请求是否符合路由规则的条件
            - Path=/user/** # 这个是按照路径匹配，只要以 /user/ 开头就符合要求
      default-filters: # 默认过滤器，会对所有的路由请求都生效
        - AddRequestHeader=Truth,Itcast is freaking awesome! # 添加请求头
```

多访问几次uri：http://localhost:10010/user/1

查看每个服务的打印信息是否有请求头信息

8081

![image-20231007193808029](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071938856.png)

8082

![image-20231007193834963](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071938158.png)

8083

![image-20231007193903983](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310071939348.png)

>  **总结**：
>
>  -  过滤器的作用是什么？
>     1.  对路由的请求或响应做加工处理，比如添加请求头
>     2.  配置在路由下的过滤器只对当前路由的请求生效
>  -  defaultFilters的作用是什么？
>     1.  对所有路由都生效的过滤器

#### 6.6.2、全局过滤器GlobalFilter:evergreen_tree: 

全局过滤器的作用也是处理一切进入网关的请求和微服务响应，与GatewayFilter的作用一样。

区别在于GatewayFilter通过配置定义，处理逻辑是固定的。而GlobalFilter的逻辑需要自己写代码实现可操作性更高。

```java
public interface ClobalFilter
{
   /**
   * 处理当前请求，有必要的话通过{@link GatewayFilterChain} 将请求交给下一个过滤器处理
   *
   * @param exchange 请求上下文，里面可以获取Request，Response等信息
   * @param chain 用来把请求委托给下一个过滤器
   * @return {@code Mono<Void>} 返回标示当前过滤器业务结束
   */
   Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain);
}
```

##### 6.6.2.1、代码案例:palm_tree: 

**定义全局过滤器，拦截并判断用户身份**.

需求：定义全局过滤器，拦截请求，判断请求的参数是否满足下面条件：

-  参数中是否有authorization
-  authorization参数值是否为admin

如果同时满足则放行，否则拦截

在gateway服务中创建一个AuthorizeFilter类实现GlobalFilter接口并重写过滤器方法来达到自己的目的

目的：请求头有authorization=admin的信息则可以访问网关后的服务，否则拦截

@Order：配置多个过滤器的执行优先级21亿到负21亿  数值越小优先级越高

也可以实现Ordered并重写getOrder函数来实现多过滤器执行优先级

```java
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.cloud.gateway.filter.GlobalFilter;
import org.springframework.core.annotation.Order;
import org.springframework.http.HttpStatus;
import org.springframework.http.server.reactive.ServerHttpRequest;
import org.springframework.stereotype.Component;
import org.springframework.util.MultiValueMap;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/719:53
 * @function
 * @comment
 */
// @Order：设置多个过滤器执行的优先级顺序，可以通过注解也可以通过方法
@Order(-1)
@Component
public class AuthorizeFilter implements GlobalFilter/*, Ordered*/ {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        // 1.获取请求参数
        ServerHttpRequest request = exchange.getRequest();
        MultiValueMap<String, String> params = request.getQueryParams();
        // 2.获取参数中的 authorization参数
        String auth = params.getFirst("authorization");
        // 3.判断参数值是否等于 admin
        if("admin".equals(auth))
        {
            // 4.是，放行
            return chain.filter(exchange);
        }
        // 5.否拦截
        // 5.1设置状态码返回给用户
        exchange.getResponse().setStatusCode(HttpStatus.UNAUTHORIZED);
        // 5.2拦截请求
        return exchange.getResponse().setComplete();
    }

/*
    重写执行过滤器优先级的方法
    @Override
    public int getOrder() {
        return -1;
    }
*/
}
```

访问uri：http://127.0.0.1:10010/user/1

次uri没有写在请求头信息authorization=admin

结果：

![image-20231007201452411](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310072014686.png)

由于访问被拦截所以服务在idea中控制台没有打印任何请求的响应数据

这次携带上请求头信息authorization=admin再进行访问uri：http://127.0.0.1:10010/user/1?authorization=admin

![image-20231007201545206](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310072015402.png)

访问成功！

>  **总结**：
>
>  -  全局过滤器的作用是什么？
>
>     对所有路由都生效的过滤器，并且可以自定义处理逻辑
>
>  -  实现全局过滤器的步骤？
>
>     1.  实现GlobalFilter接口
>     2.  添加@Order注解或实现Ordered接口
>     3.  编写处理逻辑

#### 6.6.3、过滤器执行顺序:evergreen_tree: 

请求进入网关会碰到三类过滤器：当前路由的过滤器，DefaultFilter，GlobalFilter

<font color='red'>请求路由后</font>，会将当前路由过滤器和DefaultFilter，GlobalFilter，合并到一个过滤器链(集合)中，排序后依次执行每个过滤器

![image-20231007202537098](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310072025396.png)

网关中所有过滤器最终都是GatewayFilter类型，既然是同一种类型那就可以存到一个集合中去做排序了

![image-20231007202943934](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310072029369.png)

新的问题是，怎么排序呢？

-  每一个过滤器都必须指定一个int类型的order值，==order值越小，优先级越高，执行顺序越靠前==。
-  GlobalFilter通过实现Ordered接口，或者添加@Order注解来指定order值，由我们自己指定
-  路由过滤器和defaultFilter的order由Spring指定，默认是按照声明顺序从1递增。
-  <font color='red'>当过滤器的order值一样时</font>，会按照defaultFilter > 路由过滤器 > GlobalFilter的顺序执行。

默认按照声明顺序从1递增的解释：

>  每个过滤器声明时有一个order值标记每加一个就会递增order值，而且路由过滤器和defaultFilter两个order是分开的，还有自定义的GlobalFilter我们自己设置order值也可能会一样，值都冲突了该怎么排序呢？

```yml
spring:
  cloud:
    gateway:                                          
      routes: # 网关路由配置                              
        - id: user_service # 路由id，自定义，只要唯一即可   
        # uri 有两种方式的写法                             
        # uri: http://127.0.0.1:8081 # 路由的目标地址 http 就是固定地址
          uri: lb://userservice # 路由的目标地址 lb(LoadBalance) 就是负载均衡，后面跟服务名称
          predicates: # 路由断言，也就是判断请求是否符合路由规则的条件
            - Path=/user/** # 这个是按照路径匹配，只要以 /user/ 开头就符合要求
          filters:
            - AddRequestHeader=Truth,Itcast is freaking awesome! # order 1
            - AxxxHeader=Truth,Itcast is freaking awesome! # order 2
            - AxxxHeader=Truth,Itcast is freaking awesome! # order 3
      default-filters: # 默认过滤器，会对所有的路由请求都生效
        - AddRequestHeader=Truth,Itcast is freaking awesome! # order 1
        - AxxxHeader=Truth,Itcast is freaking awesome! # order 2
        - AxxxHeader=Truth,Itcast is freaking awesome! # order 3
```

可以参考下面几个类的源码来查看：

![image-20231007204141622](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310072041796.png)

>  **总结**：
>
>  -  路由过滤器，defaultFilter，全局过滤器的执行顺序？
>     1.  order值越小，优先级越高
>     2.  当order值一样时，顺序是defaultFilter最先，然后是局部的路由过滤器，最后是全局过滤器

### 6.7、跨域问题处理:deciduous_tree: 

>  在学习前后端分离项目时已经学习过了解决跨域问题，那为什么在微服务中还要学习跨域问题呢？
>
>  这是因为在微服务中所有的请求都要先经过网关再到微服务，也就是说跨域请求你不需要在每个微服务里都去处理。仅仅在网关处理就可以了，但是网关又跟我们以前实现不一样，网关是基于WebFlux实现的，没有Servlet相关的API因此我们以前所学的解决方案不一定能够适用。

跨域：域名不一致就是跨域，主要包括：

-  域名不同：www.taobao.com和www.taobao.org和www.jd.com和miaosha.js.com
-  域名相同，端口不同：localhost:8080和loclahost:8081
-  但是我们前面学习一直用的8080访问8081端口的数据就没有出现什么问题啊这是怎么回事呢？

**原因**：跨域问题：==浏览器禁止==请求的发起者与服务端发生跨域==ajax请求==，请求被浏览器拦截的问题

order-service和user-service和浏览器没有关系，也没有用到ajax所以不会出现跨域问题

==解决方案==：CORS

#### 6.7.1、演示案例:evergreen_tree: 

前端页面

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
</body>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
  axios.get("http://localhost:10010/user/1?authorization=admin")
  .then(resp => console.log(resp.data))
  .catch(err => console.log(err))
</script>
</html>
```

访问页面后查看浏览器控制台的打印信息

![image-20231007211114669](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310072111173.png)

这里有严重的问题因为gateway服务配置文件允许跨域的请求不可能是上图中uri的样子，我们需要使用一个服务器启动页面然后将uri允许跨域请求即可，比如我下面是使用tomcat启动的这个页面端口号为：8898

可以看到报错了说是跨域问题

**网关处理跨域采用的同样是CORS方案，并且只需要简单配置即可实现**：

```yml
#===========这些配置让网关能够联系上nacos实现服务注册和发现====
server:                                              #||
  port: 10010 # 网关端口                              #||
spring:                                              #||
  application:                                       #||
    name: gateway # 服务名称                           #||
  cloud:                                              #||
    nacos:                                            #||
      server-addr: localhost:8848 # nacos地址          #||
#========================================================
#=========================网关路由配置=====================
    gateway:                                           #||
      routes: # 网关路由配置                              ||
        - id: user_service # 路由id，自定义，只要唯一即可    ||
        # uri 有两种方式的写法                             ||
        # uri: http://127.0.0.1:8081 # 路由的目标地址 http 就是固定地址
          uri: lb://userservice # 路由的目标地址 lb(LoadBalance) 就是负载均衡，后面跟服务名称
          predicates: # 路由断言，也就是判断请求是否符合路由规则的条件
            - Path=/user/** # 这个是按照路径匹配，只要以 /user/ 开头就符合要求
#          filters: # 过滤器
#            - AddRequestHeader=Truth,Itcast is freaking awesome! # 添加请求头
        - id: order_service                            #||
          uri: lb://orderservice                       #||
          predicates:                                  #||
            - Path=/order/**                           #||
            # 配置断言规则是在2037年之后才能访问
            - Before=2037-01-20T17:42:47.789-07:00[America/Denver]
      default-filters:
        - AddRequestHeader=Truth,Itcast is freaking awesome! # 添加请求头
#===========================全局的跨域处理=============================
      globalcors: # 全局的跨域处理                                    #||
      # ajax采用的是cors方案，cors是浏览器去问服务器让不让这个请求跨域，     ||
      # 但是这个请求方式是  option，默认情况下这种请求会被网关拦截的，而加上这个配置就是不拦截           # option请求，这样cors请求就能正常发出了。                         ||
        add-to-simple-url-handler-mapping: true # 解决options请求被拦截问题
        corsConfigurations:                                        #||
          '[/**]':                                                 #||
            allowedOrigins: # 允许哪些网站的跨域请求                   #||
              # - "http://localhost:8090"                            #||
                - "http://localhost:8898"                    #||
            allowedMethods: # 允许的跨域ajax的请求方式                #||
              - "GET"                                              #||
              - "POST"                                             #||
              - "DELETE"                                           #||
              - "PUT"                                              #||
              - "OPTIONS"                                          #||
            allowedHeaders: "*" # 允许在请求中携带的头信息             #||
            allowCredentials: true # 是否允许携带cookie              #||
            maxAge: 360000 # 这次跨域检测的有效期                     #||
#====================================================================
```

重启Gateway服务然后再访问uri：http://localhost:8898/index.html

配置文件中本机地址是localhost就不要用127.0.0.1这样也会被跨域拦截

![image-20231007213052264](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310072130838.png)

>  **总结**：
>
>  CORS跨域要配置的参数包括哪几个？
>
>  -  允许哪些域名跨域？
>  -  允许哪些请求头？
>  -  允许哪些请求方式？
>  -  是否允许使用cookie？
>  -  有效期是多久？
