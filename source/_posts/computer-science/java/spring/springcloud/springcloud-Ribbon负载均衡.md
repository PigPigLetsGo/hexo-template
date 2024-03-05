---
title: 三、Ribbon负载均衡
categories:
    - [计算机学科,java,spring,springcloud]
tags:
    - 计算机学科
    - springcloud
---

# 三、Ribbon负载均衡:christmas_tree: 

-  负载均衡原理
-  负载均衡策略
-  懒加载

### 3.1、负载均衡流程:deciduous_tree: 

直接通过http:userservice/user/1无法访问。并<font color='red'>不是</font>一个   <font color='red'>真实可用的地址</font>. 

当order-service发起请求时，是无法到达user-service服务的，因此一定会有人将请求拦截下来做一下处理找到真是ip和端口才行

![image-20231005085431639](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310050854814.png)

order-service发送请求被Ribbon拦截它得到请求指定的是哪个服务(userservice)，然后就去找eureka拉取userservice的服务列表。

拿到服务列表后再进行负载均衡挑选一个进行请求

![image-20231004194527737](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041945961.png)

具体Ribbon什么时候拦截处理这些请求的具体看源码：

在RestTemplate的Bean中添加的@LoadBalanced注解就代表将来RestTemplate发起的请求要被Ribbon进行拦截和处理了。

```java
@Bean
@LoadBalanced
public RestTemplate restTemplate()
{
   return new RestTemplate();
}
```

那么拦截的动作是谁来完成的呢？

是LoadBalancerInterceptor来完成的。它实现了一个接口ClientHttpRequestInterceptor这个接口是干什么呢的？

```java
public class LoadBalancerInterceptor implements ClientHttpRequestInterceptor {
```

ClientHttpRequestInterceptor它会去拦截由客户端发起的http请求，而RestTemplate不正是一个发http请求的客户端吗，所以就会被这个拦截器所拦截

ClientHttpRequestInterceptor这个接口中定义的一个方法为：intercept

```java
ClientHttpResponse intercept(HttpRequest request, byte[] body, ClientHttpRequestExecution execution) throws IOException;
```

那么我们回到它的实现类LoadBalancerInterceptor这个拦截器既然实现了ClientHttpRequestInterceptor接口就一定会实现intercept方法

我们在重写方法处打上断点 * 表示断点

```java
public ClientHttpResponse intercept(final HttpRequest request, final byte[] body, final ClientHttpRequestExecution execution) throws IOException {
*   URI originalUri = request.getURI();
   String serviceName = originalUri.getHost();
   Assert.state(serviceName != null, "Request URI does not contain a valid hostname: " + originalUri);
   return (ClientHttpResponse)this.loadBalancer.execute(serviceName, this.requestFactory.createRequest(request, body, execution));
}
```

我们可以认为由RestTemplate发起的请求就一定会被intercept拦截

开启debug访问url：http://127.0.0.1:8080/order/101

就会执行到断点处

![image-20231005093021344](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310050930000.png)

证明这个请求确实被拦截了，获取请求地址拿到的是http://userservice/user/1这不就是哪个无法直接访问的地址吗

![image-20231005093231384](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310050932340.png)

```java
public ClientHttpResponse intercept(final HttpRequest request, final byte[] body, final ClientHttpRequestExecution execution) throws IOException {
   // 获取请求地址
   URI originalUri = request.getURI(); // http://userservice/user/1
   // 获取主机名(不是指本地主机名) url中的主机名也就是服务名称
   // 拿到服务名称后找eureka拉取服务列表信息
   String serviceName = originalUri.getHost(); // serviceName: userservice
   // 得到服务名称后怎么完成拉取的呢
   Assert.state(serviceName != null, "Request URI does not contain a valid hostname: " + originalUri);
   // Ribbon的负载均衡客户端去执行
   return this.loadBalancer.execute(serviceName, this.requestFactory.createRequest(request, body, execution));
}
```

进入execute方法，执行getLoadBalancer得到一个对象，这个对象名为：DynamicServerListLoadBalancer就是动态服务列表负载均衡器

![image-20231005094542640](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310050945434.png)

在服务列表中可以看到8081，8082成功的被拉取到了已经。

![image-20231005094741094](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310050947059.png)

所以getLoadBalancer这个方法就是在根据服务名称找eureka拉取服务列表，下一步就是负载均衡了

进入getServer看看是怎么做的

![image-20231005094951601](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310050949067.png)

rule(规则) 在DynamicServerListLoadBalancer中选择一个那么选择时是有一种规则来选的那么这个rule是个什么类型对象呢

![image-20231005095131490](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310050953275.png)

叫做IRule(I就是接口的意思Rule就是规则的意思)那么就是一个规则的接口的意思，那它一定会有实现类通过ctrl+h查看实现类

![image-20231005095318356](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310050953846.png)

它的实现类都有以下几个

RandomRule：随机规则

RoundRobinRule：轮询调度

![image-20231005095544615](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310050955516.png)

而它默认的负载均衡规则：ZoneAvoidanceRule

那么看到这里我们就清楚了负载均衡的策略是由IRule接口来决定的

然后进行返回getServer拿到一个服务为8081，拿到服务信息了就可以用真是的ip，端口来替代原来的http://userservice/user/1服务名称。去发送真实的请求了。

![image-20231005095838892](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051001998.png)

**总结一下**：

当请求进入Ribbon后会怎么去处理呢，请求会被一个拦截器 拦截名为：LoadBalancerInterceptor拦截后会得到请求的服务名称userservice然后将服务名称交给RibbonLoadBalancerClient它再将服务交给DynamicServerListLoadBalancer它会去eureka中拉取服务列表信息，然后从服务列表中挑选一个而这个负载均衡是IRule来做的根据规则挑一个出来选中了8081服务，将值返回给RibbonLoadBalancerClient然后用这个ip，端口替换服务名称得到真实的请求地址最后就请求到了8081服务

这就是整个Ribbon工作的流程了 !

![image-20231004203247209](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310042032809.png)

#### 3.1.1、IRule接口实现:evergreen_tree: 

IRule接口决定了负载均衡的策略，下面详细学习IRule这个接口都有哪些实现以及自己如何修改它的每个实现

Ribbon的负载均衡规则是一个叫做IRule的接口来定义的，每一个子接口都是一种规则：

<center>IRule接口继承关系结构图</center>

![image-20231004203629391](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310042036620.png)

### 3.2、负载均衡策略:deciduous_tree: 

| 内置负载均衡规则类                         | 规则描述                                                     |
| ------------------------------------------ | ------------------------------------------------------------ |
| RoundRobinRule                             | 简单轮询服务列表来选择服务器。它是Ribbon默认的负载均衡规则   |
| AvailabilityFilteringRule                  | 对以下两种服务器进行忽略：<br />(1) 在默认情况下，这台服务器如果3次连接失败，这台服务器就会被设置为 “短路” 状态。短路状态将<br />持续30秒，如果再次连接失败，短路的持续就会几何级的增加。<br />(2) 并发数过高的服务器。如果一个服务器的并发连接数过高，配置了AvailabilityFilterngRule规则的客户端也会将其忽略。并发连接数的上限，可以由客户端的<clientName></clientConfigNameSpace>.ActiveConnectionsLimit属性进行配置。 |
| WeightedResponseTimeRule                   | 为每一个服务器赋予一个权重值。服务器响应时间越长，这个服务器的权重就越小。这个规则会随机选择服务器，这个权重值会影响服务器的选择。 |
| <font color='red'>ZoneAvoidanceRule</font> | 以区域可用的服务器为基础进行服务器的选择。使用Zone对服务器进行分类，这个Zone可以理解为一个机房，一个机架等。而后再对Zone内的多个服务做轮询。 |
| BestAvailableRule                          | 忽略哪些短路的服务器，并选择并发数较低的服务器。             |
| RandomRule                                 | 随机选择一个可用的服务器。                                   |
| RetryRule                                  | 重试机制的选择逻辑。                                         |

通过定义IRule实现可以修改负载均衡规则，有两种方式：

1、代码方式：在order-service中的OrderAppliction类中，定义一个新的IRule

这样就会让负载均衡规则从轮询变成随机规则

```java
@Bean
public IRule randomRule()
{
   return new RandomRule();
}
```

2、配置文件方式：在order-service的application.yml文件中，添加新的配置也可以修改规则：

先指定服务名称，再去执行负载均衡规则因此这种配置方案它是针对某个微服务而言的。

```yml
userservice: # 服务名称
  ribbon:
    NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule # 负载均衡规则
```

### 3.3、饥饿加载:deciduous_tree: 

==Ribbon默认是采用懒加载==，即第一次访问时才会去创建LoadBalanceClient，请求时间会很长。

而饥饿加载则会在==项目启动时创建==，降低第一次访问时的耗时，通过下面配置开启饥饿加载：

```yml
userservice: # 服务名
  ribbon:
    # NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule # 负载均衡规则
    eager-load:
      enabled: true # 开启饥饿加载
      clients: userservice # 指定对userservice这个服务饥饿加载
      # 如果多个服务需要饥饿加载则写如下格式
      # clients:
      #     - userservice
      #     - 服务名1
      #     - 服务名2
```

>  **总结**：
>
>  1.  Ribbon负载均衡规则
>      -  规则接口是IRule
>      -  默认实现是ZoneAvoidanceRule，根据zone选择服务列表，然后轮询
>  2.  负载均衡自定义方式
>      -  代码方式：配置灵活，但修改时需要重新打包发布
>      -  配置方式：直观，方便，无需重新打包发布，但是无法做全局配置
>  3.  饥饿加载
>      -  开启饥饿加载
>      -  指定饥饿加载的微服务名称
