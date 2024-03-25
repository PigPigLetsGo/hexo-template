---
title: 四、Nacos注册中心
categories:
    - [计算机学科,java,spring,springcloud,Nacos]
tags:
    - 计算机学科
    - springcloud
    - Nacos
---

## 四、Nacos注册中心:christmas_tree: 

-  认识和安装Nacos
-  Nacos快速入门
-  Nacos服务分级存储模型
-  Nacos环境隔离

### 4.1、认识Nacos:deciduous_tree: 

[Nacos](http://nacos.io/zh-cn/)是阿里巴巴的产品，现在是[SpringCloud](https://spring.io/projects/spring-cloud)中的一个组件。相比[Eureka](https://github.com/Netflix/eureka)功能更加丰富，在国内受欢迎程度较高。

![image-20231005115513961](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051155062.png)

**安装和启动教程详细查看**：[点击查看](Nacos/Nacos安装指南.md).

### 4.2、服务注册到Nacos:deciduous_tree: 

1、在cloud-demo父工程中添加spring-cloud-alibaba的管理依赖：

```xml
<!--nacos管理依赖-->
<dependency>
   <groupId>com.alibaba.cloud</groupId>
   <artifactId>spring-cloud-alibaba-dependencies</artifactId>
   <version>2.2.5.RELEASE</version>
   <type>pom</type>
   <scope>import</scope>
</dependency>
```

2、注释掉order-service和user-service中原有的eureka依赖。

3、添加nacos的客户端依赖：

```xml
<!-- nacos客户端依赖包 -->
<dependency>
   <groupId>com.alibaba.cloud</groupId>
   <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

4、修改user-service & order-service中的application.yml文件，注释eureka地址，添加nacos地址：

```yml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos服务地址
```

==PS==：在当前启动类中添加注解：@EnableDiscoveryClient (服务发现)

5、启动并测试

![image-20231005130842602](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051308798.png)

>  **总结**：
>
>  1.  Nacos服务搭建
>      1.  下载安装包
>      2.  解压
>      3.  在bin目录下运行指令：startup.cmd -m standalone
>  2.  Nacos服务注册或发现
>      1.  引入nacos.discovery依赖
>      2.  配置nacos地址spring.cloud.nacos.server-addr
>      3.  在当前启动类中添加@EnableDiscoveryClient注解，用于服务发现

### 4.3、Nacos服务分级存储模型:deciduous_tree: 

**服务**：提供用户查询的user service ，提供订单查询的order-service都叫==服务==.

user-service还部署了多个实例：8081，8082，8083

所以我们之前是分为两层概念的：

一层是服务，而第二层就是实例，一个服务可以包含多个实例

![image-20231005142022420](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051420619.png)

不过随着业务规模越来越扩大，那么我们就要考虑更多的问题了。

>  比如说我们把所有的实例都部署到一个机房，这就像把所有的鸡蛋都放到一个篮子里一样，不小心将篮子打翻了那么里面所有的蛋也就全部都打了，如果是机房哪天天灾人祸出了事儿，那这整个服务不就完了。所以为了解决这个问题我们会将一个服务的多个实例部署到多个机房。

特别像阿里巴巴，京东这样的大公司，给全国各地都整一个。这就像我们把鸡蛋分散开了，一个打了我还有好几篮子呢。

这样就能做到一种叫 **容灾**.

![image-20231005142511407](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051425151.png)

而Nacos服务分级存储模型就是引入了这样一个机房的概念或者叫地域的概念。它把同在一个机房的多个实例称为一个集群

比如说：杭州两个机房的userService实例就称之为杭州的userService集群

![image-20231005143046790](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051430517.png)

所以在Nacos服务分级存储模型中一级是服务往下是集群再往下就是实例

![image-20231005143242380](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051432046.png)

**为什么Nacos要引入这样的一个服务分级的模型呢？我原来直接用服务找实例不好吗？为什么要多加一个地域划分集群的一个概念？**

#### 4.3.1、服务跨集群调用问题:evergreen_tree: 

我们设想这样一种情况：

比方说 我有一个杭州的机房里面有order-service集群还有user-service的集群。还有还有一个上海的机房里面也是同样的配置

现在，order-service需要访问user-service那么有两种选择：一种是在自己本地局域网内访问，另一种是去外面机房访问

局域网内访问速度比较快，延迟比较低

>  服务调用尽可能选择本地集群的服务，跨集群调用延迟较高
>
>  本地集群不可访问时，再去访问其它集群

![image-20231005144140180](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051442382.png)

但是现在我们还没有配置集群属性，看下nacos控制台

![image-20231005150616402](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051506788.png)

可以看到集群名称为：Default默认，没有集群

![image-20231005150745644](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051507220.png)

### 4.4、服务集群属性:deciduous_tree: 

1、修改application.yml，添加如下内容：

```yml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos服务端地址
      discovery:
        cluster-name: HZ # 配置集群名称，也就是机房位置，这里HZ代指杭州
```

2、我们创建两个服务来模拟演示集群三个分别代表不同的位置

![image-20231005151045217](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051510774.png)

具体的操作步骤看动图：

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051518637.gif)

3、查看nacos的情况

可以看到成功启动了三个集群

![image-20231005151355371](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051513016.png)

查看详细，别分是不同的位置的集群

![image-20231005151517180](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051515250.png)

>  **总结**：
>
>  1.  Nacos服务分级存储模型
>      1.  一级是服务，例如userservice
>      2.  二级是集群，例如杭州或上海
>      3.  三级是实例，例如杭州机房的某台部署了userservice的服务器
>  2.  如何设置实例的集群属性
>      1.  修改application.yml文件，添加spring.cloud.nacos.discovery.cluster-name属性即可

我们将user-service的8081服务和8082服务还有8083服务分别设置到了不同位置的集群但是我们最终想要实现的是order-service远程调用user-service时，优先选择本地集群。那因此呢我们还需要给order-service也配置一个集群属性

#### 4.4.1、根据集群负载均衡:evergreen_tree: 

1、在order-service服务中的application.yml中进行配置

```yml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos服务端地址
      discovery:
        cluster-name: HZ # 配置集群名称，也就是机房位置，这里HZ代指杭州
```

配置完成后启动order-service服务

![image-20231005153152984](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051531329.png)

查看nacos信息order-service是否与user-service处于HZ集群

![image-20231005153349541](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051533345.png)

![image-20231005153426289](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051534369.png)

2、然后在order-service中设置负载均衡的IRule为NacosRule，这个规则优先会寻找与自己同集群的服务：

```yml
# 给指定的微服务进行集群负载均衡的配置
userservice:
  ribbon:
    NFLoadBalancerRuleClassName: com.alibaba.cloud.nacos.ribbon.NacosRule # 负载均衡规则
```

3、访问order-service服务3次查看轮询结果

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051630022.gif)

访问第一次：

```sh
http://127.0.0.1:8080/order/101
```

访问第二次：

```sh
http://127.0.0.1:8080/order/102
```

访问第三次：

```sh
http://127.0.0.1:8080/order/103
```

8081

```
Creating a new SqlSession
SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@20d32e8c] was not registered for synchronization because synchronization is not active
JDBC Connection [HikariProxyConnection@773117158 wrapping com.mysql.cj.jdbc.ConnectionImpl@1a9c99e4] will not be managed by Spring
==>  Preparing: select * from tb_user where id = ? 
==> Parameters: 1(Long)
<==    Columns: id, username, address
<==        Row: 1, 柳岩, 湖南省衡阳市
<==      Total: 1
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@20d32e8c]
Creating a new SqlSession
SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@4c31c2cf] was not registered for synchronization because synchronization is not active
JDBC Connection [HikariProxyConnection@1123427641 wrapping com.mysql.cj.jdbc.ConnectionImpl@1a9c99e4] will not be managed by Spring
==>  Preparing: select * from tb_user where id = ? 
==> Parameters: 2(Long)
<==    Columns: id, username, address
<==        Row: 2, 文二狗, 陕西省西安市
<==      Total: 1
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@4c31c2cf]
Creating a new SqlSession
SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@16810291] was not registered for synchronization because synchronization is not active
JDBC Connection [HikariProxyConnection@407305777 wrapping com.mysql.cj.jdbc.ConnectionImpl@1a9c99e4] will not be managed by Spring
==>  Preparing: select * from tb_user where id = ? 
==> Parameters: 3(Long)
<==    Columns: id, username, address
<==        Row: 3, 华沉鱼, 湖北省十堰市
<==      Total: 1
Closing non transactional SqlSession [org.apache.ibatis.session.defaults.DefaultSqlSession@16810291]
```

8082

```
```

8083

```
```

可以看到优先选择的是本地集群的服务，但是也会有可能选择远程的集群。

它并不是轮询调用本机集群或者远程集群而是一种没有规律的随机的调用方式

如果说user-service:8081这个服务挂掉了呢？ 将8081关闭我们再次访问

![image-20231005163505936](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051635074.png)

此时8083就被请求了打印出了消息

![image-20231005163557083](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051635696.png)

但是在order-service中控制台中就会打印一个警告

警告的意思就是：发生了一次跨集群的访问order-service访问user-service 位置为HZ的集群但是却访问了 SH

![image-20231005163756874](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051637118.png)

通过这次测试我们得出NacosRule它其实优先访问的是本地，本地没有它才会进行跨集群访问，而跨集群就会发出一次警告

3、注意将user-service的权重都设置为1

![image-20231005164127314](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051641100.png)

>  **总结**：
>
>  1.  NacosRule负载均衡策略
>      1.  优先选择同集群服务实例列表
>      2.  本地集群找不到提供者，才去其它集群寻找，并且会报警告
>      3.  确定了可用实例列表后，再采用随机负载均衡挑选实例

#### 4.4.2、根据权重负载均衡:evergreen_tree: 

>  上集，我们学习了Nacos的负载均衡的一种规则NacosRule，它可以做到==集群优先的负载均衡==。不过我们部署的时候可能出现这样的情况，由于企业设备会更新迭代有些机器性能比较好，还有一些属于是祖传设备了性能非常差，可以说老弱病残。这时候我们肯定希望这些性能好的机器它承担更多的用户请求，而哪些性能差一点的自然承担少一点的用户请求。但是我们目前看来NacosRule做到的是集群优先而后做随机。当用户请求完了以后它可不会管你机器性能好还是差的拉过来就是一顿叫这时性能差的机器肯定就出问题了。
>
>  那么我们要怎么去控制不同服务它请求的量呢？
>
>  Nacos给我们提供了一种权重的配置

**实际部署中会出现这样的场景**：

-  服务器设备性能有差异，部分实例所在机器性能较好，另一些较差，我们希望性能好的机器承担更多的用户请求

Nacos提供了权重配置来控制访问频率，权重越大则访问频率越高

#### 4.4.3、权重配置操作：:evergreen_tree: 

1、在Nacos控制台可以设置实例的权重值，首先选中实例后面的编辑按钮

![image-20231005165528544](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051655722.png)

2、将权重设置为0.1，测试可以发现8081被访问到的频率大大降低

权重值配置范围：0 ~ 1

![image-20231005165622001](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051656163.png)

查看下目前的集群情况

order-service

![image-20231005165920414](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051659631.png)

user-service

![image-20231005170021931](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051700588.png)

在没有配置权重值都是相同的情况下访问3次order-service打印的信息如下

user-service:8081 打印了全部的信息，8082,8083都没有

我们将8081这个集群的权重值设置为0在进行测试

![image-20231005194035918](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310051940128.png)

访问order-service刷新页面10次打印的信息如下

8081没有信息，8082有信息，8083有信息

>  **总结**：
>
>  1.  实例的权重控制
>      1.  Nacos控制台可以设置实例的权重值，0 ~ 1之间
>      2.  同集群内的多个实例，权重越高被访问的频率越高
>      3.  权重设置为0则完全不会被访问

### 4.5、环境隔离-namespace:deciduous_tree: 

Nacos中服务存储和数据存储的最外层都是一个名为namespace的东西，用来做最外层隔离

>  将来可以有多个namespace可以理解为多个蛋，相互之间是隔离开的，在namespace内部有一个group属性。也就是说同一个命名空间的多个东西我们将来还可以分组，组内就是具体的东西了。比如说服务，服务再往下就是集群，再往下就是实例。

这里我们的环境隔离其实就是在对服务做隔离

![image-20231005201313765](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052013655.png)

为什么需要隔离呢？

服务划分，实例划分这是基于业务去做的划分或者是地域但事实上我们还会有开发环境，生产环境，测试环境的变化所以我们会基于这种环境变化去做隔离。

我们可以吧业务相关度比较高的放在一组中

1、在Nacos控制台可以创建namespace，用来隔离不同环境

![image-20231005202456034](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052024412.png)

2、然后填写一个新的命名空间信息

![image-20231005202536537](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052025744.png)

3、保存后会在控制台看到这个命名空间的id

![image-20231005202632739](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052026832.png)

4、修改order-service的application.yml，添加namespace

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/cloud_user?useSSL=false&characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: dkx.
    driver-class-name: com.mysql.cj.jdbc.Driver
  application:
    name: userservice
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos服务端地址
      discovery:
        cluster-name: TJ # 集群名称，这里HZ代指杭州
        namespace: ed32fa6f-e9e3-4483-b860-7bcd9731d0d9 # 命名空间，填 ID
```

5、刷新nacos页面

order-service就不在public中了

![image-20231005203254198](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052032144.png)

而是到了dev中了，它们两个已经不是同一个位置的服务了

![image-20231005203322946](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052033504.png)

我们访问的话就会报错：

![image-20231005203400951](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052034062.png)

**idea控制台报错提示为**：<font color='red'>找不到可用的实例</font>.

![image-20231005203453936](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052034175.png)

所以要想让服务可以访问必须是同一个环境下

>  **总结**：
>
>  1.  Nacos环境隔离
>      1.  namespace用来做环境隔离
>      2.  每个namespace都有唯一id
>      3.  不同namespace下的服务不可见
>          -  如果想要两个服务可以访问必须把它们放到同一个namespace下

### 4.6、nacos注册中心细节分析:deciduous_tree: 

服务提供者在启动时，都会把自己的信息提交给注册中心。而注册中心就会把这些信息保留下来

![image-20231005203942965](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052039311.png)

当服务消费者要消费时它就可以去找注册中心拉取服务信息了。不过这个拉取的动作不是每一次都要做的它会将拉取到的服务信息缓存到一个列表中

当然如果缓存一直不更新也是不行的万一服务提供者信息变化了呢，所以这个列表会每隔30秒重新拉取一次，进行一个更新

![image-20231005204255542](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052042295.png)

消费者拿到服务列表以后再去负载均衡挑选一个发起远程调用就可以了

![image-20231005204549665](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052045232.png)

但是nacos与eureka还是有一些差别，差别就在于服务提供者的健康检测

nacos会把==服务提供者划分成==**临时实例**和**非临时实例**。

在nacos页面中挑选一个服务查看 详情就可以看到 临时实例还是非临时实例了

如下图，nacos中默认就是临时实例，而临时实例与非临时实例在nacos中做健康监测是不一样的

![image-20231005204834080](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052048533.png)

#### 4.6.1、临时实例:evergreen_tree: 

临时实例，听其名认为它是将来可能人为的把服务停掉的，所以它在nacos中做健康监测时采用的是心跳检测

如果服务提供者心跳不跳了超过时间就会被nacos从服务列表中剔除

![image-20231005205144644](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052051280.png)

#### 4.6.2、非临时实例:evergreen_tree: 

>  非临时实例中nacos不会去做心跳，那么这时健康监测是怎么监测的呢？
>
>  是由nacos主动发请求去询问服务提供者：“诶！你还活着吗？”，如果它没回应nacos并不会将它从服务列表中剔除而是标记为不健康了，它们等着这个服务恢复健康

![image-20231005205645566](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052056840.png)

这是一个nacos与eureka之间的差别，但是还有一个差别在于消费者

eureka采用的是定时拉取每隔30秒拉取一次，如果有服务提供者在30秒内挂掉了那消费者不知道吧，它不知道提供者挂了还直接去消费就会出问题了。

所以eureka做服务定时拉取它服务列表更新的这个效率比较差(更新不够及时)

而nacos它做了一个叫推送消息。也就是eureka采用的是pull，而nacos采用的是pull+push两者结合

假设nacos发现服务提供者挂了，它就会立即发送一条消息推送给我们的服务消费者，然后服务消费者就赶紧更新

![image-20231005210145076](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052101513.png)

这就是eureka与nacos之间的一些差别了！

#### 4.6.3、临时实例和非临时实例:evergreen_tree: 

服务注册到nacos时，可以选择注册为临时实例或非临时实例，通过下面的配置来设置：

```yml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        ephemeral: false # 设置是否为临时实例 设置为非临时实例
```

临时实例宕机时，会从nacos的服务列表中剔除，而非临时实例则不会

​	在order-service中进行配置为非临时实例，再去nacos中查看orderservice的详情

可以看到临时实例为false，表示为非临时实例了

![image-20231005213059792](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052131922.png)

这时我们将order-service服务停掉

由于是非临时实例，就算服务提供者挂掉了 注册中心 也不会将其从服务列表中剔除掉，标红表示这个服务挂掉了

![image-20231005213247819](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052132484.png)

如果改为临时实例

```yml
spring:
  cloud:
    nacos:
      server-addr: localhost:8848
      discovery:
        ephemeral: true # 设置是否为临时实例 设置为非临时实例
```

![image-20231005213417852](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052134138.png)

我们再次将order-service服务停掉，然后再刷新页面或者返回到这个页面时发现关掉的服务被 注册中心 给剔除掉了不在服务列表中了。

![image-20231005213452133](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310052134361.png)

##### 4.6.3.1、CAP理论:palm_tree: 

C一致性，A高可用，P分区容错性

-  eureka只支持AP

-  nacos支持CP和AP两种

   nacos是根据配置识别CP或AP模式，如果注册Nacos的client节点注册时是ephemeral=true即为临时节点，那么Nacos集群对这个client节点效果就是AP，反之则是CP，既不是临时节点

>  **总结**：
>
>  1.  Nacos与eureka的共同点
>      1.  都支持服务注册和服务拉取
>      2.  都支持服务提供者心跳方式做健康检测
>  2.  Nacos与Eureka的区别
>      1.  Nacos支持服务端主动检测提供者状态：
>          1.  临时实例采用心跳模式
>          2.  非临时实例采用主动检测模式
>      2.  临时实例心跳不正常会被剔除，非临时实例则不会被剔除(除非手动删除)
>      3.  Nacos支持服务列表变更的消息推送模式，服务列表更新及时
>      4.  Nacos集群默认采用AP方式，当集群中存在非临时实例时，采用CP模式；Eureka采用AP方式

### 4.7、Nacos配置管理:deciduous_tree: 

-  统一配置管理
-  配置热更新
-  配置共享
-  搭建Naocs集群

#### 4.7.1、统一配置管理:evergreen_tree: 

上面学习我们搭建了两个微服务，每个服务里面的业务都需要完成数据库的查询，并且服务之间还会有一个相互调用。而完成相互调用我们的做法是让服务注册到注册中心，然后消费者就可以从注册中心完成服务的发现，实现服务获取和负载均衡。完成远程调用

![image-20231006084042876](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310060840065.png)

那么随着我们微服务越来越多，我们在生产环境中可能会达到数十上百甚至上千台服务器的情况，这时就有一个问题。

<font color='red'>**问题一**</font>：如果我们修改一个配置文件而这个配置文件可能跟我数十上百个微服务都有关系，那这个时候我是不是要逐个微服务去调整配置呢？

这样很麻烦。

<font color='red'>**问题二**</font>：修改配置后这些关联的服务是不是都需要重启啊

所以我们的需求就是可以将配置文件进行==统一的管理==.

**统一管理**：我不需要修改数十上百个微服务的配置文件而是只需要修改一个地方的配置文件就可以了。

-  **配置更改热更新**.

   希望配置更改后不需要重启，而可以立马生效这就是 ==配置更改热更新==.

而要实现 “配置更改热更新” 我们就需要一个 “配置管理服务”

>  **配置管理服务的作用**：
>
>  会去记录微服务中的一些核心的配置，那么微服务启动的时候就可以去读取 ==配置管理服务== 中的配置再和本地的配置结合作为完整配置去使用了。将来如果配置要发生修改我们不需要逐个服务修改，而是找到 ==配置管理服务== 在它上面把需要修改的配置修改一下，==配置管理服务== 发现配置修改了以后它会立即==通知微服务==，微服务再去读取 ==配置管理服务== 中修改后的配置文件并且不用重启进行热更新生效

![image-20231006090015625](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310060900914.png)

我们使用的是nacos的配置管理，而我们知道nacos已经有注册中心功能了。所以我们配置中心也好，注册中心也好，同时都是由nacos去实现的。

![image-20231006090222743](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310060902958.png)

1、在Nacos中添加配置信息：

![image-20231006091626481](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310060916710.png)

2、在弹出表单中填写配置信息：

<font color='red'>注意项</font>：

1.  DataId：需要具备唯一性
2.  配置内容：只需要配置有热更新需求的内容而不是将服务中配置文件的内容全拷贝进去

填写完毕后，点击发布

![image-20231006104947319](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061049488.png)

3、发布完成后返回查看

![image-20231006091517632](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310060915386.png)

先看如下图 没有Nacos时服务如何获取配置，配置获取的步骤如下：

![image-20231006092110607](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310060921217.png)

上面流程中读取的是本地的配置文件，但现在我们多了一个东西就是nacos中的配置文件。

将来我们的项目会把nacos的配置与本地配置做一个合并而后再去完成后续的容器创建bean的创建动作。

所以nacos读取要加入到上面流程当中那么就会变成如下的步骤：

项目启动先读取nacos配置文件再去读取本地配置文件两者合并最后创建bean

![image-20231006092504157](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310060925392.png)

<font color='red'>问题</font>：

-  **第一**：去哪读取
-  **第二**：读取谁

我们读取nacos配置文件时需要提前知道nacos的地址，那么需要一个启动优先级高于application.yml的东西来存储nacos地址。

此时，可以使用 “bootstrap.yml” 配置文件，这是SpringBoot中提供的。这个文件的优先级高于application.yml

当项目启动优先读取 “bootstrap.yml” 配置文件，我们只要把nacos地址，文件相关信息都配置进去那么它就可以完成nacos配置的读取了。而后再跟本地结合完成后续动作

![image-20231006093027358](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310060930844.png)

**那么就需要注意了**：

-  ==与nacos地址和配置文件有关的所有信息都应该放到 “bootstrap.yml” 当中==。

#### 4.7.2、操作步骤：:evergreen_tree: 

1、引入Nacos的配置管理客户端依赖：

```xml
<!--nacos 配置管理依赖-->
<dependency>
   <groupId>com.alibaba.cloud</groupId>
   <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

2、在userservice中的resource目录添加一个<font color='red'>bootstrap.yml</font>文件，这个文件是引导文件，优先级高于application.yml

```yml
spring:
  application:
    name: userservice # 服务名称
  profiles:
    active: dev # 环境
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos 地址
      config:
        file-extension: yaml # 配置文件后缀名
# 三要素：服务名称，环境，后缀名 而这三个决定了DateId
```

多环境配置和共享配置 不在本案例范围中，只是演示可以这样做

多环境配置：

```yml
spring:
  application:
    name: content-service
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos 地址
      config:
        namespace: dev # 命名空间
        group: xuecheng-plus-project # 组名称
        file-extension: yaml # 文件后缀
        refresh-enabled: true # 是否开启自动刷新值
```

共享配置/扩展配置

多个扩展配置的案例如下：

[案例文章](https://pigpigletsgo.github.io/computer-science/java/spring/springcloud/Nacos/%E6%A1%88%E4%BE%8B/%E6%8B%86%E5%88%86%E4%B8%8D%E5%90%8C%E9%85%8D%E7%BD%AE%E5%88%B0nacos%E5%B9%B6%E8%AF%BB%E5%8F%96%E4%B8%8D%E5%90%8C%E9%85%8D%E7%BD%AE%E5%88%B0%E9%A1%B9%E7%9B%AE/)

```yml
spring:
  application:
    name: content-api # 服务名
  cloud:
    nacos:
      server-addr: 192.168.249.128:8848
      discovery:
        namespace: dev
        group: xuecheng-plus-project
      config: # 配置文件相关信息
        namespace: dev
        group: xuecheng-plus-project
        file-extension: yaml
        refresh-enabled: true
        # 共享配置/扩展配置
        extension-configs:
          - data-id: content-service-${spring.profiles.active}.yaml
            group: xuecheng-plus-project
            refresh: true
  profiles:
    active: dev #环境名
```

3、我们在user-service中将pattern.dateformat这个属性注入到UserController中做测试：

```java
@Slf4j
@RestController
@RequestMapping("/user")
public class UserController {

    // 注入nacos中的配置属性
    @Value("${pattern.dateformat}")
    private String dateformat;

    // 编写controller，通过日期格式化器来格式化现在时间并返回
    @GetMapping("now")
    public String now()
    {
        System.out.println("dateFormat = "+dateformat);
        return LocalDateTime.now().format(DateTimeFormatter.ofPattern(dateformat));
    }
}
```

4、请求接口查看结果

![image-20231006105444087](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061054265.png)

![image-20231006105458931](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061056279.png)

>  **总结**：
>
>  将配置交给nacos管理的步骤
>
>  1.  在nacos中添加配置文件
>  2.  在微服务中引入nacos的config依赖
>  3.  在微服务中添加bootstrap.yml，配置nacos地址，当前环境，服务名称，文件后缀名。这些决定了程序启动时去nacos读取哪个文件，写错一个就会报错！

#### 4.7.3、配置自动刷新:evergreen_tree: 

我们改一下nacos中配置文件的信息，点击编辑进行修改指定的nacos配置文件信息

![image-20231006105819948](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061058197.png)

原来的

![image-20231006105744125](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061057556.png)

修改后的

![image-20231006105913419](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061059763.png)

修改完点击发布，然后再去请求获取日期格式化的接口

可以看到没有变化

![image-20231006110004691](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061100940.png)

要想实现配置的自动更新我们需要如下操作：

Nacos中的配置文件变更后，微服务无需重启就可以感知。不过需要通过下面两种配置实现：

##### 4.7.4、@RefreshScope:palm_tree: 

-  方式一：在@Value注入的变量所在类上添加注解@RefreshScope

![image-20231006110317505](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061103130.png)

加上注解后重启服务，因为上次的更改没有生效这里又将nacos配置文件的信息改为原来的了。再次进行修改测试

修改前

![image-20231006110621029](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061106769.png)

修改后

![image-20231006110720366](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061107482.png)

在没有重启服务的情况下刷新两下从nacos配置文件中获取的格式化日期就发生了改变，说明热更新生效了。

##### 4.7.5、@ConfigurationProperties:palm_tree: 

-  方式二：使用@ConfigurationProperties注解

通过@ConfigurationProperties注解读取一个前缀为pattern的配置信息 (约定大于配置)

![image-20231006111308948](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061113170.png)

![image-20231006112043132](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061120271.png)

重启服务，测试结果

修改前

![image-20231006112226998](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061122192.png)

修改后

![image-20231006112303423](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061123665.png)

刷新页面

![image-20231006112317230](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061123281.png)

>  **总结**：
>
>  Nacos配置更改后，微服务可以实现热更新，方式：
>
>  1.  通过@Value注解注入，结合@RefreshScope来刷新
>  2.  通过@ConfigurationProperties注入，自动刷新 (建议使用，因为方便)
>
>  <font color='red'>**注意事项**</font>：
>
>  -  不是所有的配置都适合放到配置中心，维护起来比较麻烦
>  -  建议将一些关键参数，需要运行时调整的参数放到nacos配置中心，一般都是自定义配置

#### 4.7.4、多环境配置共享:evergreen_tree: 

>  场景：
>
>  有一个配置属性，它在开发，生产和测试等环境下的值都是一样的，那像这样的配置我在每个配置文件里都写一份。是不是有点浪费呀，而且将来如果要改动我还得在每个配置文件都去改这样很麻烦。我们想找一个地方我配置一次以后不管环境怎么变这个配置都能够被加载。那这就是多环境共享的需求了！

微服务启动时会从nacos读取多个配置文件：

第一个：它的构成有三部分

1.  服务名称
2.  环境
3.  后缀名

-  [spring.application.name]-[spring.profiles.active].yaml，例如：userservice-dev.yaml

第二个：它的构成有二部分，跟环境没有关系

1.  服务名称
2.  后缀名

-  [spring.application.name].yaml，例如：userservice.yaml

无论profile如何变化，[spring.application.name].yaml这个文件一定会加载，因此多环境共享配置可以写入这个文件

>  比如说如下图，有dev.yaml和userservice.yaml将来有测试环境在定义一个userservice-test.yaml。但是不管是dev还是userservice-test，userservice.yaml一定会被共享

![image-20231006114137708](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061141107.png)

编写获取envSharedValue的代码：

```java
@Data
@Component
@ConfigurationProperties(prefix = "pattern")
public class PatternProperties {
    private String dateformat;
    private String envSharedValue;
}
```

```java
@Slf4j
@RestController
@RequestMapping("/user")
public class UserController {
    @Autowired
    private PatternProperties patternProperties;
   
    @GetMapping("prop")
    public PatternProperties patternProperties()
    {
        return patternProperties;
    }
}
```

启动user-service服务！

但是需要注意当前服务的环境：

当前处于dev开发环境下，所以既可以访问到userservice环境也可以访问到dev环境

```yml
spring:
  application:
    name: userservice # 服务名称
  profiles:
    active: dev # 环境
  cloud:
    nacos:
      server-addr: localhost:8848 # nacos 地址
      config:
        file-extension: yaml # 配置文件后缀名
# 三要素：服务名称，环境，后缀名 而这三个决定了DateId
```

然后再启动一个test环境的服务这次不修改配置文件中的代码，而是在idea启动参数中设置 环境

![image-20231006121029595](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061210818.png)

![image-20231006121106691](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061211188.png)

两个服务启动完成后，进行请求user-service服务：

访问：http://127.0.0.1:8081/user/prop 这是dev环境，可以看到显示了 格式化日期和envSharedValue信息

![image-20231006123404747](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061234508.png)

访问：http://127.0.0.1:8082/user/prop 这是test环境，可以看到依然可以获取到userservice环境的值，而dev环境的格式化日期的值获取不到了。

![image-20231006123455577](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061234043.png)

同上测试可以证明，8081：dev环境，8082：test环境。都均可以得到 userserivce环境的值

证明了userservice.yaml文件是被不同环境所共享的。

##### 4.7.4.1、测试配置文件优先级:palm_tree: 

假如说userservice与dev配置文件中有相同的属性那它会以谁的为准呢？

测试一、在本地和远端都配置相同的属性看看以谁的为准

在本地配置文件中配置一个值

```yml
pattern:
  name: 本地环境local
```

在PatternProperties类中也加上这个属性

```java
@Data
@Component
@ConfigurationProperties(prefix = "pattern")
public class PatternProperties {
    private String dateformat;
    private String envSharedValue;
    private String name;
}
```

再到nacos控制台中给userservice环境加上这个属性

![image-20231006124535158](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061245246.png)

![image-20231006124517976](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061245100.png)

重启user-service服务。请求user-service的url：http://127.0.0.1:8081/user/prop 

结果是userservice环境中的属性值。这就证明了 本地与远端，远端的优先级高！

![image-20231006124719747](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061247510.png)

我们再将nacos控制台中的dev环境也加上这个属性

![image-20231006124900533](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061249639.png)

![image-20231006124929760](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061249996.png)

再去请求user-service的url：http://127.0.0.1:8081/user/prop 

这次结果是自己的环境配置的属性值了

![image-20231006125141160](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061251197.png)

##### 4.7.4.2、多种配置的优先级：:palm_tree: 

-  服务名-profile.yaml > 服务名称.yaml > 本地配置

<center>优先级关系图</center>

![image-20231006125407644](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310061254076.png)

如果想让本地配置优先则配置如下内容：

```yml
# 配置本地优先
spring:
 cloud:
  config:
   override-none: true
```



>  **总结**：
>
>  微服务会从nacos读取的配置文件：
>
>  1.  [服务名]-[spring.profile.active].yaml，环境配置
>  2.  [服务名].yaml，默认配置，多环境共享
>
>  优先级：
>
>  -  [服务名]-[环境].yaml > [服务名].yaml > 本地配置

#### 4.7.5、Nacos集群搭建:evergreen_tree: 

Nacos生产环境下一定要部署为集群状态，部署方式参考课前资料中的文档：

>  在前面的学习中我们学习到了Nacos的基本用法，不过之前我们一直使用的都是单点的nacos。这种方式我们自己测试的时候玩玩可还行，如果是在企业生产环境下也这么搞。那可就要出大问题了。
>
>  因为在企业当中我们更强调的是高可用所以nacos一定要做成一个集群，下面我们学习如何搭建nacos集群

查看nacos配合nginx基本集群搭建：[nacos集群搭建文章](./Nacos/nacos集群搭建.md).

>  **总结**：
>
>  集群搭建步骤：
>
>  1.  搭建MySQL集群并初始化数据库表
>  2.  下载解压nacos
>  3.  修改集群配置(节点信息)，数据库配置
>  4.  分别启动多个nacos节点
>  5.  nginx反向代理
