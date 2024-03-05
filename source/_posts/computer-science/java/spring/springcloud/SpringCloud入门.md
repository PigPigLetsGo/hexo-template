---
title: 零、SpringCloud入门|介绍
categories:
    - [计算机学科,java,spring,springcloud]
tags:
    - 计算机学科
    - springcloud
    - 介绍
---

## 零、SpringCloud:christmas_tree: 

### 0.1、什么是微服务?:deciduous_tree: 

![image-20231004090313859](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040903119.png)

微服务做的第一件事就是==拆分==。

<font color='red'>因为传统的单体架构，所有的业务功能全部都写在一起随着业务越来越复杂代码也越来越耦合将来想要升级维护就会很困难</font>.

![image-20231004090509652](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040905977.png)

### 0.2、拆分:deciduous_tree: 

所以需要进行==拆分==.

微服务在做拆分的时候会根据==业务功能模块把一个单体的项目拆分成许多个独立的项目==，==每个项目完成一部分业务功能==.

![image-20231004090908822](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040909340.png)

### 0.3、服务集群:deciduous_tree: 

将来==独立开发==和==部署==，我们把一个==独立的项目==称为一个==服务==.

一个大型的互联网项目会包含数百甚至上千个服务最终形成一个==服务集群==.

![image-20231004091001079](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040910853.png)

### 0.4、调用关系:deciduous_tree: 

而**一个业务往往就需要有多个服务共同来完成**。

<font color='red'>比如说一个请求来了可能会去调用服务A，然后A调用了服务B，B调用了C。当业务越来越多越来越复杂的时候这些服务中间的关系就会越来越复杂</font>.

![image-20231004091151122](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040911835.png)

这么复杂的调用关系让人去记录和维护这是不可能的，那怎么办呢？

### 0.5、注册中心:deciduous_tree: 

在微服务中就会有一个组件叫做 “==注册中心==”

![image-20231004091258876](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040912887.png)

它可以去==记录==微服务中==每一个微服务==的**IP**，**端口**，以及**可以干什么事儿这些信息**.

==当有一个服务调用另外的服务时==它<font color='red'>不需要自己去记录对方的IP</font>了==直接找注册中心==，从==注册中心获取对应的 服务信息==.

随着服务越来越多，每个服务都有自己的配置文件，将来如果要更改配置难道需要我们逐一去修改吗，这样太麻烦了

### 0.6、配置中心:deciduous_tree: 

所以在微服务中还有一个 “==配置中心==”

![image-20231004091637687](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040916028.png)

它可以去==管理整个服务群里成千上百的配置==。**如果有配置需要变更直接找**==配置中心==**就可以了**，它会通知相关的微服务实现==配置的热更新==。当我们微服务运行起来以后用户就可以访问我们了。

### 0.7、网关组件:deciduous_tree: 

但是！这时还需要一个 ==网关组件==.

网关就像小区的看门大爷，不是谁都随便进出的

==网关一方面就是对用户身份做校验==.

![image-20231004092022457](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040920302.png)

==另一方面可以将用户的请求路由到我们具体的服务，在路由过程中可以做**负载均衡**==.

![image-20231004092134757](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040921256.png)

这时服务接收到用户的请求去处理业务该访问数据库时就访问数据库，以后把查询到的数据返回给用户

![image-20231004092258211](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040922644.png)

数据库也可能是**集群**但是没有用户多，可能会扛不住因此还加入了 ==缓存==.

### 0.8、缓存:deciduous_tree: 

缓存是什么

==缓存就是把数据库查询出常用的数据存放到内存当中==，内存查询效率比数据库快很多，而<font color='red'>缓存还不能是单体缓存为了做高并发还是分布式缓存</font>。

![image-20231004092528106](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040925055.png)

用户请求先到==缓存==，==缓存未命中了再去查数据库==.

### 0.9、分布式搜索功能:deciduous_tree: 

一些==海量==的==复杂==的==搜索统计和分析缓存做不了==。这个时候我们需要用到==分布式搜索功能==.

那==数据库主要的职责就是写数据操作==还有==事务安全类型==要求较高的一些数据存储

![image-20231004092815751](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040928230.png)

### 0.10、异步消息队列:deciduous_tree: 

最后在微服务中还需要一种 ==异步通信的消息队列== 组件。为什么呢？

对于==分布式的服务或者在微服务里面它的业务往往会跨越多个服务==：<font color='red'>一个请求来了先调用请求A，A再调B，B再调C整个业务的链路就很长，那调用时长就会等于每个服务执行时长之和性能会有一定的下降</font>.

**异步请求意思就是，请求来了调用服务A，服务A不是去调用B和C而是通知你们，通知B和C你们去干活儿去，而服务A直接就结束了。这样业务链路就变短了响应时间也就缩短了那么吞吐能力也就变强了。所以异步通信可以大大提高我们服务的并发**.

![image-20231004093342606](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040933174.png)

### 0.11、异常排除:deciduous_tree:  

如果，这样如此<font color='red'>庞大</font>和<font color='red'>复杂</font>的一个服务在运行的过程当中如果出现了什么问题好排查吗？<font color='red'>不好排查！</font>.

所以在微服务当中我们还会引入两个新的组件来解决这种服务的 **异常定位**.

1 **分布式日志服务**。

它可以去==统计整个集群当中成千上百的服务它们的运行日志==，==统一做一个存储统计分析==。将来==出现问题就比较好定位了==.

2 **系统监控和链路追踪**。

可以实时==监控整个集群中每一个服务节点的运行状态==，==CPU负载==，==内存的占用==等等的情况，==一旦出现任何问题直接可以定位到具有的某一个方法展现信息==，那么就==可以很快定位到异常所在位置了==.

![image-20231004093935355](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040939839.png)

### 0.12、自动化部署:deciduous_tree: 

Jenkns ：对项目进行自动化编译

Docker ：打包，形成镜像

kubernetes 和 RANCHER 实现自动化部署

![image-20231004094218305](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040942711.png)

**这一套就可以称之为**：==持续集成==。<font color='red'>结合微服务这些技术，再加上持续集成那么这才是完整的微服务技术栈</font>.

## :zap:如何学习？:star2:  

1、**微服务治理**:deciduous_tree: 

2、**缓存技术**:deciduous_tree: 

3、**异步通信MQ相关技术**:deciduous_tree: 

4、**分布式搜索技术**:deciduous_tree: 

5、**持续集成DevOps相关技术**:deciduous_tree: 

![image-20231004095140699](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040951444.png)

## 一、认识微服务:christmas_tree: 

-  服务架构演变
-  SpringCloud

### 1.1、服务架构演变:deciduous_tree: 

#### 1.1.1、单体架构:evergreen_tree: 

>  将业务的所有功能集中在一个项目中开发，打成一个包部署。

##### 优点：

-  架构简单

-  部署成本低

   ![image-20231004104442050](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041045582.png)

##### 缺点：

-  耦合度高
-  不适合做大型项目开发

比如说有一个商城项目里面的功能肯定很多有很多模块，开发这种项目时。因为是单体架构所以不用去搞非常复杂的架构设计，只需要创建一个项目，然后有功能就往里面加代码。再有功能再加代码，不断的堆积代码就OK了

![image-20231004104206028](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041042597.png)

#### 1.1.2、分布式架构:evergreen_tree: 

>  根据业务功能对系统进行拆分，每个业务模块作为独立项目开发，称为一个服务。

##### 优点：

-  降低服务耦合
-  有利于服务升级扩展

![image-20231004105033179](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041050054.png)

>  **问题**：
>
>  因为做了服务的==拆分==，==服务拆分的越多将来部署各方面也会越复杂==，而且在拆分的过程中也会有一些问题，拆分好的机器为了保证高可用还要做集群，一旦拆分就会产生一个新的问题原来是单体项目的时候都是在一起用户下单的时候需要商品信息怎么办在商品功能里直接调service 就完事儿了，因为是在==一个项目里大家可以互相调用。但是现在做了拆分那就不行了==，==两服都是独立的机器不能互相调用了==，**除非从一个服务向另一个服务发送请求直接来调用服务，这种调用就叫** “==远程调用==”

![image-20231004105902381](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041059941.png)

##### 1.1.2.1、分布式 架构要考虑的问题：

-  服务拆分粒度如何？
   -  怎么拆，那几个业务单独作为独立模块，哪些业务在一起呢
-  服务集群地址如何维护？
   -  地址必须是方便调用的
-  服务之间如何实现远程调用？
   -  跨服务调用
-  服务健康状态如何感知？
   -  上百个服务调用如果某一个出现问题了就会发生阻塞 称之为 “级联失败”

人们在做分布式架构的时候一直在想办法解决这样的问题，因此也出现了各种各样的技术：WebService，ESB，Hession，Dubbo，SpringCloud等等。

但是近几年最火的莫过于 微服务方案了

#### 1.1.3、微服务:evergreen_tree: 

微服务是一种经过良好架构设计的==分布式==架构方案，微服务架构特征：

-  **单一职责**：微服务拆分粒度更小，每一个服务都对应唯一的业务能力，做到单一职责，避免重复业务开发

-  **面向服务**：微服务对外暴漏业务接口

-  **自治**：团队独立，技术独立，数据独立，部署独立

-  **隔离性强**：服务调用做好隔离，容错，降级，避免出现级联问题

   ![image-20231004112312974](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041123653.png)

##### 总结：

>  单体架构特点
>
>  -  简单方便，高度耦合，扩展性差，适合小型项目。例如：学生管理系统
>
>  分布式架构特点
>
>  -  松耦合，扩展性好，但架构复杂，难度大。适合大型互联网项目，例如：京东，淘宝
>
>  ==微服务==：一种良好的分布式架构方案
>
>  -  优点：拆分粒度更小，服务更独立，耦合度更低
>  -  缺点：架构非常复杂，运维，监控，部署难度提高

##### 1.1.3.1、微服务结构:evergreen_tree: 

微服务这种方案需要技术框架来落地，全球的互联网公司都在积极尝试自己的微服务落地技术。在国内最知名的就是SpringCloud和阿里巴巴的Dubbo

无论是SpringCloud还是Dubbo它们实现的功能基本是一致的

首先它们需要去做==微服务的拆分形成微服务集群==.

![image-20231004113225360](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041132761.png)

而集群中每个服务都要遵循单一职责的原则并且要==面向服务对外暴漏接口==，这样==服务之间就可以互相调用了==.

![image-20231004113330486](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041133120.png)

只不过不同技术去实现接口可能会有一些差异

##### 1.1.3.2、微服务技术对比:evergreen_tree: 

|                | Dubbo               | SpringCloud              | SpringCloudAlibaba       |
| -------------- | ------------------- | ------------------------ | ------------------------ |
| 注册中心       | zookeeper，Redis    | Eureka，Consul           | Nacos，Eureka            |
| 服务远程调用   | Dubbo协议           | Feign (http协议)         | Dubbo，Feign             |
| 配置中心       | 无                  | SpringCloudConfig        | SpringCloudConfig，Nacos |
| 服务网关       | 无                  | SpringCloudGateway，Zuul | SpringCloudGateway，Zuul |
| 服务监控和保护 | dubbo-admin，功能弱 | Hystrix                  | Sentinel                 |

##### 1.1.3.3、企业需求:evergreen_tree: 

![image-20231004114622603](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041146882.png)

### 1.2、SpringCloud:deciduous_tree: <img src="https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041159005.png" alt="image-20231004115500484" style="zoom: 50%;" />

>  SpringCloud是目前国内使用最广泛的微服务框架。官网地址：https://spring.io/projects/spring-cloud

-  SpringCloud集成了各种微服务功能组件，并基于SpringBoot实现了这些组件的自动装配，从而提供了良好的开箱即用体验

![image-20231004115847487](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041158804.png)

-  SpringCloud与SpringBoot的版本兼容关系如下：

![image-20231004120012308](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310041200407.png)
