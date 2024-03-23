---
title: 八、RabbitMQ服务异步通讯
categories:
    - [计算机学科,java,spring,springcloud,RabbitMQ]
tags:
    - 计算机学科
    - springcloud
    - RabbitMQ
---

## 八、服务异步通讯:christmas_tree: 

实用篇-RabbitMQ

**学习内容**：

-  初始MQ
-  RabbitMQ快速入门
-  SpringAMQP

### 8.1、初始MQ:deciduous_tree: 

-  同步通讯
-  异步通讯
-  MQ常见框架

#### 8.1.1、同步调用的问题:evergreen_tree: 

微服务间基于Feign的调用就属于同步方式，存在一些问题。

>  我们做一个购买商品的业务，用户支付调用支付服务。支付成功后需要调用订单业务，修改订单状态。然后还要去调用仓储服务，因为要给用户发货。
>
>  支付服务调用订单服务也好，调用仓储服务也好它都需要等待对方的响应。所以这种调用是事实的调用也就是同步调用

![image-20231010151319336](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310101513894.png)

它的问题：

1、耦合问题

如果需要增加业务而需要改动支付服务的代码

![image-20231010151616770](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310101516352.png)

2、性能下降

用户来调用支付业务，假如说支付业务耗时50ms，紧接着支付服务调用其它服务，其它服务都耗时150ms。支付服务调用再调用其它服务是同步调用所以它必须等待其它服务的执行结果才能执行其它的工作。而这一套流程下来就达到了耗时：500ms。这样的业务也太烂了

![image-20231010151851047](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310101518339.png)

3、资源浪费

支付服务在不能执行下一步操作时一直都是在占用CPU，占用内存的

4、级联失败问题

假如说中间某个服务报错了就会产生阻塞，等待时长较长结果且是失败的。用户体验极差

综上所述：

同步调用尽管时效性还不错但是却存在其它问题

![image-20231010152713676](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310101527400.png)

>  **总结**：
>
>  同步调用的优点：
>
>  -  时效性较强，可以立即得到结果
>
>  同步调用的问题：
>
>  -  耦合度高
>  -  性能和吞吐能力下降
>  -  有额外的资源消耗
>  -  有级联失败问题

#### 8.1.2、异步调用方案:evergreen_tree: 

异步调用常见实现就是事件驱动模式

引入了一个新的东西叫：Broker (事件代理者)

支付服务通知Broker，Broker通过其它服务

在整个过程当中，支付服务完成事件发布后就立即结束了自己的业务，可以去返回给用户了，而不需要去等其它服务完成结果

这种方式就是异步方式

![image-20231010153325787](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310101533471.png)

优势：

1、服务解耦

有新的业务只需要订阅Broker就行了跟支付服务没有任何关系

2、性能提升，吞吐量提高

3、没有强依赖关系，不担心级联失败问题

4、流量削峰

解释 “流量削峰” ：假如说一瞬间来了三个请求，订单服务，仓储服务，短信服务每一时刻只能处理一个，这时Broker可以起到缓冲的 作用。然后其它服务就一个一个处理，处理完了就到Broker去取，这样这些服务就能一直按照自己平时的处理速度来工作。一切压力由Broker扛着，这样一个高度的并发就可以变成低度的并发，这！就是 流量削峰

![image-20231010154228957](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310101542090.png)

>  **总结**:
>
>  异步通信的优点：
>
>  -  耦合度低
>  -  吞吐量提升
>  -  故障隔离
>  -  流量削峰
>
>  异步通信的缺点：
>
>  -  依赖于Broker的可靠性，安全性，吞吐能力
>  -  架构复杂了，业务没有明显的流程线，不好追踪管理

#### 8.1.3、什么是MQ:evergreen_tree: 

MQ (MessageQueue) ，中文是消息队列，字面来看就是存放消息的队列。也就是事件驱动架构中的Broker

|            | RabbitMQ          | ActiveMQ                          | RocketMQ   | KafKa      |
| ---------- | ----------------- | --------------------------------- | ---------- | ---------- |
| 公司/社区  | Rabbit            | Apache                            | 阿里       | Apache     |
| 开发语言   | Erlang            | Java                              | Java       | Scala&Java |
| 协议支持   | AMQP，XMPP，STOMP | OpenWire，STOMP，REST，XMPP，AMQP | 自定义协议 | 自定义协议 |
| 可用性     | 高                | 一般                              | 高         | 高         |
| 单机吞吐量 | 一般              | 差                                | 高         | 非常高     |
| 消息延迟   | 微秒级            | 毫秒级                            | 毫秒级     | 毫秒以内   |
| 消息可靠性 | 高                | 一般                              | 高         | 一般       |

#### 8.1.4、RabbitMQ快速入门:evergreen_tree: 

-  RabbitMQ概述和安装
-  常见消息模型
-  快速入门

##### 8.1.4.1、RabbitMQ概述:palm_tree: 

RabbitMQ是基于Erlang语言开发的开源消息通信中间件，官网地址：https://www.rabbitmq.com/

安装RabbitMQ可以点击查看详细文章：[RabbitMQ安装部署指南](./RabbitMQ/RabbitMQ部署指南.md).

>  RabbitMQ的结构和概念
>
>  Publisher是消息发送者，consumer就是消息消费者，发送者会把消息发送到exchange也就是交换机。交换机负责路由把消息投递到queue队列，队列负责暂存消息，然后消费者再去从队列中获取消息然后处理消息
>
>  VirtulHost就是虚拟主机，将来有多个用户它会有自己的一个虚拟主机，各个虚拟主机之间互相隔离避免干扰
>
>  这就是整体MQ的架构了

![image-20231010204307956](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310102044747.png)

>  **总结**：
>
>  RabbitMQ中的几个概念：
>
>  -  channel：操作MQ的工具
>  -  exchange：路由消息到队列中
>  -  queue：缓存消息
>  -  virtual host：虚拟主机，是对queue，exchange等资源的逻辑分组

##### 8.1.4.2、常见消息模型:palm_tree: 

MQ的官方文档中给出了5个MQ的demo示例，对应了几种不同的用法：

-  基本消息队列 (BasicQueue)
-  工作消息队列 (WorkQueue)

![image-20231010205233688](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310102052128.png)

-  发布订阅 (Publish ，Subscribe)，又根据交换机类型不同分为三种：

   -  Fanout Exchange：广播
   -  Direct Exchange：路由
   -  Topic Exchange：主题

   图中，紫色的就是交换机

![image-20231010205422743](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310102054075.png)

##### 8.1.4.3、HelloWorld案例:palm_tree: 

官方的HelloWorld是基于最基础的消息队列模型来实现的，只包括三个角色：

-  publisher：消息发布者，将消息发送到队列 queue
-  queue：消息队列，负责接受并缓存消息
-  consumer：订阅队列，处理队列中的消息

![image-20231010205637991](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310102056496.png)

发布者代码：

debug运行代码

```java
import com.rabbitmq.client.Channel;
import com.rabbitmq.client.Connection;
import com.rabbitmq.client.ConnectionFactory;
import org.junit.Test;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

public class PublisherTest {
    @Test
    public void testSendMessage() throws IOException, TimeoutException {
        // 1.建立连接
*        ConnectionFactory factory = new ConnectionFactory();
        // 1.1.设置连接参数，分别是：主机名、端口号、vhost、用户名、密码
        factory.setHost("192.168.249.128");
        factory.setPort(5672);
        factory.setVirtualHost("/");
        factory.setUsername("itcast");
        factory.setPassword("123321");
        // 1.2.建立连接
        Connection connection = factory.newConnection();

        // 2.创建通道Channel
        Channel channel = connection.createChannel();

        // 3.创建队列
        String queueName = "simple.queue";
        channel.queueDeclare(queueName, false, false, false, null);

        // 4.发送消息
        String message = "hello, rabbitmq!";
        channel.basicPublish("", queueName, null, message.getBytes());
        System.out.println("发送消息成功：【" + message + "】");

        // 5.关闭通道和连接
        channel.close();
        connection.close();

    }
}
```

代码走到建立连接没有报错的话，查看下RabbitMQ的ui管理平台中的Connections

里面就会出现一个主机的连接信息

![image-20231011074738545](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110748590.png)

再往下创建通道

![image-20231011074927559](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110749797.png)

再往下创建队列

![image-20231011075033895](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110750475.png)

再往下准备发送消息往队列中发送queName，将消息转为字节发送 getBytes()

下面就可以看到队列中有一条消息：Ready = 1

![image-20231011075619843](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110756263.png)

我们可以点击Name，点击进去然后点击Get Message 里面可以看到发送的消息

![image-20231011075826602](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110758298.png)

消费者代码

debug运行

```java
import com.rabbitmq.client.*;

import java.io.IOException;
import java.util.concurrent.TimeoutException;

public class ConsumerTest {

    public static void main(String[] args) throws IOException, TimeoutException {
        // 1.建立连接
        ConnectionFactory factory = new ConnectionFactory();
        // 1.1.设置连接参数，分别是：主机名、端口号、vhost、用户名、密码
        factory.setHost("192.168.249.128");
        factory.setPort(5672);
        factory.setVirtualHost("/");
        factory.setUsername("itcast");
        factory.setPassword("123321");
        // 1.2.建立连接
        Connection connection = factory.newConnection();

        // 2.创建通道Channel
        Channel channel = connection.createChannel();

        // 3.创建队列
        String queueName = "simple.queue";
        channel.queueDeclare(queueName, false, false, false, null);

        // 4.订阅消息
        channel.basicConsume(queueName, true, new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope,
                                       AMQP.BasicProperties properties, byte[] body) throws IOException {
                // 5.处理消息
                String message = new String(body);
                System.out.println("接收到消息：【" + message + "】");
            }
        });
        System.out.println("等待接收消息。。。。");
    }
}
```

在消费者代码中又进行了创建队列，发布者已经产生时创建了一个队列了，但是消费者为什么又要创建队列呢？

这是因为，发布者和消费者它们启动的顺序不确定。万一消费者先启动的呢，那找队列找不到怎么办。为了避免这种问题发生它们各自都去声明队列，虽然代码是重复的但是我们可以看下RabbitMQ-ui控制台的Queue管理页面

它并没有去创建这个队列，所以发布者与消费者都写只是一个保险措施防止不存在队列

![image-20231011084433956](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110844291.png)

第四步消费消息

![image-20231011084522086](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110845508.png)

消费者最终在idea控制台的打印信息

```
等待接收消息。。。。
接收到消息：【hello, rabbitmq!】
```

先打印了main函数的 “等待接收消息。。。。”这段话，而后打印了 消费的消息。

因为回调机制消费者处理函数handleDelivery与队列进行了绑定queueName。但是消息还并没有过来然后再继续往下执行打印了 “等待接收消息。。。。” ，等rabbitMQ把消息投递回来了再打印 “接收到消息：【hello, rabbitmq!】”，这就说明RabbitMQ处理消息是异步的

>  **总结**：
>
>  基本消息队列的消息发送流程：
>
>  1.  建立connection
>  2.  创建channel
>  3.  利用channel声明队列
>  4.  利用channel向队列发送消息
>
>  基本消息队列的消息接收流程：
>
>  1.  建立connection
>  2.  创建channel
>  3.  利用channel声明队列
>  4.  定义consumer的消费行为handleDelivery()
>  5.  利用channel将消费者与队列绑定

#### 8.1.5、SpringAMQP:evergreen_tree: 

-  Basic Queue 简单队列模型
-  Work Queue 工作队列模型
-  发布，订阅模型-Fanout
-  发布，订阅模型-Direct
-  发布，订阅模型-Topic
-  消息转换器

##### 8.1.5.1、什么是SpringAMQP:palm_tree: 

SpringAMQP官方地址：https://spring.io/projects/spring-amqp

![image-20231011090014596](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110903769.png)

##### 8.1.5.2、案例，利用SpringAMQP实现HelloWorld中的基础消息队列功能:palm_tree: 

**流程如下**：

1、在父工程中引入spring-amqp的依赖

因为publisher和consumer服务都需要amqp依赖，因此这里把依赖直接放到父工程mq-demo中：

```xml
<!--AMQP依赖，包含RabbitMQ-->
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```

2、在publisher服务中利用RabbitTemplate发送消息到simple.queue到这个队列

在publisher中编写测试方法，向simple.queue发送消息

2.1、在publisher服务中编写application.yml，添加mq连接信息：

```yml
spring:
  rabbitmq:
    host: 192.168.249.128 # RabbitMQ的ip地址
    port: 5672 # RabbitMQ的端口号
    username: itcast # 用户名
    password: 123321 # 密码
    virtual-host: / # 虚拟主机
```

2.2、在publisher服务中新建一个测试类，编写测试方法：

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/119:18
 * @function
 * @comment
 */
@SpringBootTest
@RunWith(SpringRunner.class)
public class SpringAmqpTest {
    @Autowired
    private RabbitTemplate rabbitTemplate;
    @Test
    public void test()
    {
        String queueName = "simple.queue";
        String message = "hello,Spring Amqp";
        rabbitTemplate.convertAndSend(queueName, message);
    }
}
```

查看rabbitmq的ui管理平台的队列信息

![image-20231011092250032](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110922013.png)

![image-20231011092326888](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110923115.png)

>  **总结**：
>
>  什么是AMQP
>
>  -  应用间消息通信的一种协议，与语言和平台无关
>
>  SpringAMQP如何发送消息？
>
>  -  引入amqp的starter依赖
>  -  配置RabbitMQ地址
>  -  利用RabbitTemplate的convertAndSend方法发送消息到指定队列中

3、在consumer服务中编写消费逻辑，绑定simple.queue这个队列

在consumer中编写消息逻辑，监听simple.queue

3.1、在consumer服务中编写application.yml，添加mq连接信息：

```yml
spring:
  rabbitmq:
    host: 192.168.249.128 # RabbitMQ的ip地址
    port: 5672 # RabbitMQ的端口号
    username: itcast # 用户名
    password: 123321 # 密码
    virtual-host: / # 虚拟主机
```

3.2、在consumer服务中新建一个类，编写消费逻辑：

不能写在test中进行测试是因为它需要被Spring所管理注册为Bean

```java
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/119:34
 * @function
 * @comment
 */
@Component
public class SpringRabbitListener {

    @RabbitListener(queues = "simple.queue")
    public void listenerSimpleQueue(String msg)
    {
        System.out.println("消费者接收到simple.queue的消息为：" + msg);
    }
}
//----------------------打印结果----------------------
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.3.9.RELEASE)

10-11 09:37:32:584  INFO 1164 --- [           main] cn.itcast.mq.ConsumerApplication         : Starting ConsumerApplication on Jfier with PID 1164 (E:\微服务\实用篇\资料\day04-MQ\资料\mq-demo\consumer\target\classes started by Administrator in E:\微服务\实用篇\资料\day04-MQ\资料\mq-demo)
10-11 09:37:32:594  INFO 1164 --- [           main] cn.itcast.mq.ConsumerApplication         : No active profile set, falling back to default profiles: default
10-11 09:37:34:476  INFO 1164 --- [           main] o.s.a.r.c.CachingConnectionFactory       : Attempting to connect to: [192.168.249.128:5672]
10-11 09:37:34:531  INFO 1164 --- [           main] o.s.a.r.c.CachingConnectionFactory       : Created new connection: rabbitConnectionFactory#640dc4c6:0/SimpleConnection@7a0f244f [delegate=amqp://itcast@192.168.249.128:5672/, localPort= 3380]
10-11 09:37:34:610  INFO 1164 --- [           main] cn.itcast.mq.ConsumerApplication         : Started ConsumerApplication in 2.755 seconds (JVM running for 4.791)
消费者接收到simple.queue的消息为：hello,Spring Amqp
```

>  **总结**：
>
>  SPringAMQP如何接收消息？
>
>  -  引入amqp的starter依赖
>  -  配置RabbitMQ地址
>  -  定义类，添加@Component注解
>  -  类中声明方法，添加@RabbitListener注解，方法参数就是消息

#### 8.1.6、Work Queue工作队列:evergreen_tree: 

它也具备消息发布和消费，但是不同的是它后面挂了两个消费者。两个消费者共同处理发布者的消息，

比如发布者发送50消息，consumer1消费25，consumer2消费25。被消费过的消息就会被删除不可能会再被其它消费者消费已经消费过的消息。

![image-20231011094302230](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310110943473.png)

##### 8.1.6.1、模拟WorkQueue，实现一个队列绑定多个消费者:palm_tree: 

**基本思路如下**：

1、在publisher服务中定义测试方法，每秒产生50条消息，发送到simple.queue

```java
@Test
public void workQueueTest()
{
   String queueName = "simple.queue";
   String message = "hello,Spring Amqp __ ";
   int i = 0;
   for(;i < 50;i++)
   {
      try {
         Thread.sleep(10);
      } catch (InterruptedException e) {
         throw new RuntimeException(e);
      }
      rabbitTemplate.convertAndSend(queueName, message + i);
   }
}
```

2、在consumer服务中定义两个消息监听者，都监听simple.queue队列

2.1、消费者1每秒处理50条消息，消费者2每秒处理10条消息

```java
@RabbitListener(queues = "simple.queue")
public void listenerSimpleQueue1(String msg)
{
   System.out.println("消费者1接收到simple.queue的消息为：" + msg);
   try {
      Thread.sleep(20);
   } catch (InterruptedException e) {
      throw new RuntimeException(e);
   }
}

@RabbitListener(queues = "simple.queue")
public void listenerSimpleQueue2(String msg)
{
   System.err.println("消费者2......接收到simple.queue的消息为：" + msg);
   try {
      Thread.sleep(200);
   } catch (InterruptedException e) {
      throw new RuntimeException(e);
   }
}
//--------------------打印结果--------------------
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.3.9.RELEASE)

消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 1---2023-10-11T10:16:47.564155
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 0---2023-10-11T10:16:47.564155
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 3---2023-10-11T10:16:47.602944400
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 5---2023-10-11T10:16:47.634311
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 7---2023-10-11T10:16:47.663563900
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 9---2023-10-11T10:16:47.693902600
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 11---2023-10-11T10:16:47.724277700
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 13---2023-10-11T10:16:47.754720
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 2---2023-10-11T10:16:47.784759800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 15---2023-10-11T10:16:47.784759800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 17---2023-10-11T10:16:47.814876500
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 19---2023-10-11T10:16:47.845253300
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 21---2023-10-11T10:16:47.876696200
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 23---2023-10-11T10:16:47.907389600
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 25---2023-10-11T10:16:47.938559800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 27---2023-10-11T10:16:47.968268500
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 29---2023-10-11T10:16:47.998623300
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 4---2023-10-11T10:16:47.998623300
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 31---2023-10-11T10:16:48.028620600
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 33---2023-10-11T10:16:48.058770700
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 35---2023-10-11T10:16:48.089572100
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 37---2023-10-11T10:16:48.119707200
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 39---2023-10-11T10:16:48.149984400
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 41---2023-10-11T10:16:48.179999600
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 6---2023-10-11T10:16:48.210307400
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 43---2023-10-11T10:16:48.210307400
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 45---2023-10-11T10:16:48.240964800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 47---2023-10-11T10:16:48.270986500
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 49---2023-10-11T10:16:48.301165400
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 8---2023-10-11T10:16:48.423433300
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 10---2023-10-11T10:16:48.636403400
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 12---2023-10-11T10:16:48.848982300
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 14---2023-10-11T10:16:49.060187200
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 16---2023-10-11T10:16:49.273197100
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 18---2023-10-11T10:16:49.484165800
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 20---2023-10-11T10:16:49.696880900
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 22---2023-10-11T10:16:49.908464300
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 24---2023-10-11T10:16:50.121477600
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 26---2023-10-11T10:16:50.333586300
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 28---2023-10-11T10:16:50.548236400
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 30---2023-10-11T10:16:50.759585700
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 32---2023-10-11T10:16:50.973508400
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 34---2023-10-11T10:16:51.185203800
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 36---2023-10-11T10:16:51.399855600
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 38---2023-10-11T10:16:51.610433700
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 40---2023-10-11T10:16:51.823352100
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 42---2023-10-11T10:16:52.035381900
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 44---2023-10-11T10:16:52.248226300
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 46---2023-10-11T10:16:52.462320400
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 48---2023-10-11T10:16:52.677121500
```

消费者1 很快就结束了，而消费者2却花了很长很长的时间。我们认为的快的多消费点，慢的少消费点而事实却是平均分配给了两个消费者，消费者1那拿的是所有的奇数，消费者2拿到的是所有的偶数。

这是因为RabbitMQ一个机制造成的，就是：消息预取机制

解释消息预取：

当有大量的消息到达队列时，队列中会把消息进行投递，consumer1和consumer2会提前把消息拿过来这就是消息预取，不管能不能处理先拿过来再说

于是两个人就平均分配所有的消息一人分了25条，但是呢consumer1处理的快很快就搞定了，consumer2处理的慢需要一段时间

![image-20231011102418214](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111024244.png)

###### 可以对消息预取进行配置限制:tanabata_tree: 

修改application.yml文件，设置preFetch这个值，可以控制预取消息的上限：

```yml
spring:
  rabbitmq:
    host: 192.168.249.128 # RabbitMQ的ip地址
    port: 5672 # RabbitMQ的端口号
    username: itcast # 用户名
    password: 123321 # 密码
    virtual-host: / # 虚拟主机
    listener:
      simple:
        prefetch: 1 # 每次只获取一条消息，处理完成才能获取下一个消息
```

设置完成后再进行测试：

```
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 0---2023-10-11T10:30:40.386440300
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 1---2023-10-11T10:30:40.398703100
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 2---2023-10-11T10:30:40.413527700
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 3---2023-10-11T10:30:40.444268700
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 4---2023-10-11T10:30:40.473866400
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 5---2023-10-11T10:30:40.504620300
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 6---2023-10-11T10:30:40.535400300
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 7---2023-10-11T10:30:40.566022200
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 8---2023-10-11T10:30:40.595970600
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 9---2023-10-11T10:30:40.611076800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 10---2023-10-11T10:30:40.626201300
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 11---2023-10-11T10:30:40.657505500
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 12---2023-10-11T10:30:40.688200
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 13---2023-10-11T10:30:40.717912700
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 14---2023-10-11T10:30:40.748533200
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 15---2023-10-11T10:30:40.778181500
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 16---2023-10-11T10:30:40.808905800
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 17---2023-10-11T10:30:40.823251200
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 18---2023-10-11T10:30:40.837450100
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 19---2023-10-11T10:30:40.867629400
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 20---2023-10-11T10:30:40.897964100
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 21---2023-10-11T10:30:40.927968900
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 22---2023-10-11T10:30:40.959483800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 23---2023-10-11T10:30:40.988862
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 24---2023-10-11T10:30:41.019388500
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 25---2023-10-11T10:30:41.034540
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 26---2023-10-11T10:30:41.049536900
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 27---2023-10-11T10:30:41.079546800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 28---2023-10-11T10:30:41.110340600
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 29---2023-10-11T10:30:41.141704800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 30---2023-10-11T10:30:41.171164300
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 31---2023-10-11T10:30:41.201884700
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 32---2023-10-11T10:30:41.231497200
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 33---2023-10-11T10:30:41.246620400
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 34---2023-10-11T10:30:41.262814300
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 35---2023-10-11T10:30:41.293344400
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 36---2023-10-11T10:30:41.322739
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 37---2023-10-11T10:30:41.352387700
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 38---2023-10-11T10:30:41.382386800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 39---2023-10-11T10:30:41.412727200
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 40---2023-10-11T10:30:41.443659
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 41---2023-10-11T10:30:41.458969
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 42---2023-10-11T10:30:41.475290100
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 43---2023-10-11T10:30:41.504816800
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 44---2023-10-11T10:30:41.535720400
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 45---2023-10-11T10:30:41.565967900
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 46---2023-10-11T10:30:41.596477300
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 47---2023-10-11T10:30:41.625940900
消费者1接收到simple.queue的消息为：hello,Spring Amqp __ 48---2023-10-11T10:30:41.658033
消费者2......接收到simple.queue的消息为：hello,Spring Amqp __ 49---2023-10-11T10:30:41.674047
```

这时消费者打印的消息特快就结束了，快的呢多消费，慢的呢就慢慢来消费。起到能者多劳的功能

>  **总结**：
>
>  -  多个消费者绑定到一个队列，同一条消息只会被一个消费者处理
>  -  通过设置prefetch来控制消费者预取的消息数量

#### 8.1.7、发布(publish)，订阅(subscribe):evergreen_tree: 

发布订阅模式与之前案例的区别就是允许将同一消息发送给多个消费者。实现方式是加入了exchange (交换机)

>  上面学习了RabbitMQ的两个案例一个是简单队列案例另一个是WorkQueue案例，这两个案例有一个共同的特点就是，所发出的消息只可能被一个消费者消费。因为一旦消费完就会从队列中删除而这一特点就无法满足一个需求比如以前说的 支付服务发布的消息要被，订单服务，仓储服务，短信服务。这三个服务各自去完成自己的业务，也就是说发布的这条用户支付成功的消息要被三个服务都接收到。
>
>  这时就需要用到这章的知识了

<center>模型结构</center>

![image-20231011104256966](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111042266.png)

**常见exchange类型包括**：

-  Fanout：广播
-  Direct：路由
-  Topic：话题

<font color='red'>注意</font>：exchange负责消息路由，而不是存储，路由失败则消息丢失

##### 8.1.7.1、发布订阅-Fanout Exchange:palm_tree: 

Fanout Exchange会将接收到的消息路由到每一个跟其绑定的queue

>  就像混社会的社会人一样，后面两个consumer跟着publisher混，只要跟着publisher混那什么钱啊什么的都不用发愁

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111051241.gif)

##### 8.1.7.2、利用SpringAMQP演示FanoutExchange的使用:palm_tree: 

实现思路如下：

1、在consumer服务中，利用代码声明队列，交换机，并将两者绑定

2、在consumer服务中，编写两个消费者方法，分别监听fanout.queue1和fanout.queue2

3、在publisher中编写测试方法，向itcast.fanout发送消息

![image-20231011111040177](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111110248.png)

**步骤**：

1、在consumer服务声明Exchange，Queue，Binding

SpringAMQP提供了声明交换机，队列，绑定关系的API，例如：

在consumer服务创建一个配置类，添加@Configuration注解，并声明FanoutExchange，Queue和绑定关系对象Binding，代码如下：

```java
import org.springframework.amqp.core.Binding;
import org.springframework.amqp.core.BindingBuilder;
import org.springframework.amqp.core.FanoutExchange;
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/1111:17
 * @function
 * @comment
 */
@Configuration
public class FanoutConfig {
    // itcast.fanout
    @Bean
    public FanoutExchange fanoutExchange()
    {
        return new FanoutExchange("itcast.fanout");
    }
    // fanout.queue1
    @Bean
    public Queue fanoutQueue1()
    {
        return new Queue("fanout.queue1");
    }
    // 绑定队列1到交换机
    @Bean
    public Binding fanoutBinding(Queue fanoutQueue1, FanoutExchange fanoutExchange)
    {
        return BindingBuilder.bind(fanoutQueue1).to(fanoutExchange);
    }
    // fanout.queue2
    @Bean
    public Queue fanoutQueue2()
    {
        return new Queue("fanout.queue2");
    }
    // 绑定队列2到交换机
    @Bean
    public Binding fanoutBinding1(Queue fanoutQueue2, FanoutExchange fanoutExchange)
    {
        return BindingBuilder.bind(fanoutQueue2).to(fanoutExchange);
    }
}
```

2、在consumer服务声明两个消费者

在consumer服务的SpringRabbitListener类中，添加两个方法，分别监听fanout.queue1和fanout.queue2

```java
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/119:34
 * @function
 * @comment
 */
@Component
public class SpringRabbitListener {
    @RabbitListener(queues = "fanout.queue1")
    public void listenerFanoutQueue1(String msg)
    {
        System.out.println("消费者接收到fanout.queue1的消息为：" + msg);
    }

    @RabbitListener(queues = "fanout.queue2")
    public void listenerFanoutQueue2(String msg)
    {
        System.out.println("消费者接收到fanout.queue2的消息为：" + msg);
    }
}
```

3、在publisher服务发送消息到FanoutExchange

在publisher服务的SpringAmqpTest类中添加测试方法：

```java
@Test
public void testFanoutExchange()
{
   // 交换机名称
   String exchangeName = "itcast.fanout";
   // 消息
   String message = "hello , every one";
   // 发送消息，参数分别是：交互机名称，Routingkey (暂时为空)，消息
   rabbitTemplate.convertAndSend(exchangeName, "", message);
}
```

启动消费者的启动类，然后启动发布者的test函数进行测试

![image-20231011114245056](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111142286.png)

>  **总结**：
>
>  交换机的作用是什么？
>
>  -  接收publisher发送的消息
>  -  将消息按照路由规则路由到与之绑定的队列
>  -  不能缓存消息，路由失败，消息丢失
>  -  FanoutExchange的会将消息路由到每个绑定的队列
>
>  声明队列，交换机，绑定关系的Bean是什么？
>
>  -  Queue
>  -  FanoutExchange
>  -  Binding

#### 8.1.8、发布订阅-DirectExchange:evergreen_tree: 

DirectExchange会将接收的消息根据==规则路由到指定的Queue==，因此称为路由模式 (routers)。

**规则**：

-  每一个Queue都与Exchange设置一个BindingKey，将来利用暗号进行通信，可以有多个Key
-  发布者发送消息时，指定消息的RoutingKey
-  Exchange将消息路由到BindingKey与消息RoutingKey一致的队列

![image-20231011115208085](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111152624.png)

##### 8.1.8.1、利用SpringAMQP演示DirectExchange的使用:palm_tree: 

**实现思路如下**：

1、利用@RabbitListener声明Exchange，Queue，RoutingKey

2、在consumer服务中，编写两个消费者方法，分别监听direct.queue1和direct.queue2

3、在publisher中编写测试方法，向itcast.direct发送消息

![image-20231011115803693](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111158674.png)

步骤：

1、利用@RabbitListener声明Exchange，Queue，RoutingKey

2、在consumer服务中，编写两个消费者方法，分别监听direct.queue1和direct.queue2

```java
@RabbitListener(bindings = @QueueBinding(
   // 声明队列
   value = @Queue(name = "direct.queue1"),
   // 声明交换机
   exchange = @Exchange(name = "itcast.direct", type = ExchangeTypes.DIRECT),
   // 绑定key
   key = {"red", "blue"}
))
public void listenDirectQueue1(String msg)
{
   System.out.printf("消费者接收到direct.queue1的消息：[" + msg + "]");
}

@RabbitListener(bindings = @QueueBinding(
   // 声明队列
   value = @Queue(name = "direct.queue1"),
   // 声明交换机
   exchange = @Exchange(name = "itcast.direct", type = ExchangeTypes.DIRECT),
   // 绑定key
   key = {"red", "yellow"}
))
public void listenDirectQueue2(String msg)
{
   System.out.printf("消费者接收到direct.queue2的消息：[" + msg + "]");
}
```

启动服务看下rabbitmq-ui管理页面

![image-20231011134749247](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111347685.png)

3、在publisher中编写测试方法，向itcast.direct发送消息

```java
@Test
public void testDirectExchange()
{
   // 交换机名称
   String exchangeName = "itcast.direct";
   // 消息
   String message = "hello , blue";
   // 发送消息，将消息发送给key为blue的Queue
   rabbitTemplate.convertAndSend(exchangeName, "blue", message);
}
//------------------打印结果------------------
消费者接收到direct.queue1的消息：[hello , blue]
```

将绑定的key改为yellow呢，记得将发送消息改为 “hello yellow”

```java
// 发送消息，将消息发送给key为blue的Queue
rabbitTemplate.convertAndSend(exchangeName, "yellow", message);
//----------------打印结果----------------
消费者接收到direct.queue2的消息：[hello , yellow]
```

将绑定的key改为red呢，记得将发送消息改为 “hello red”

```java
// 发送消息，将消息发送给key为blue的Queue
rabbitTemplate.convertAndSend(exchangeName, "red", message);
消费者接收到direct.queue2的消息：[hello , red]消费者接收到direct.queue1的消息：[hello , red]
```

>  **总结**：
>
>  描述下Direct交换机与Fanout交换机的差异？
>
>  -  Fanout交换机将消息路由给每一个与之绑定的队列
>  -  Direct交换机根据RoutingKey判断路由给哪个队列
>  -  如果多个队列具有相同的RoutingKey，则与Fanout功能类似
>
>  基于@RabbitListener注解声明队列和交换机有哪些常见注解？
>
>  -  @Queue
>  -  Exchange

#### 8.1.9、发布订阅-TopicExchange:evergreen_tree: 

TopicExchange与DirectExchange类似，区别在于routingKey必须是多个单词的列表，并且以 。 分割。

Queue与Exchange指定BindingKey时可以使用通配符：

`#`：代指0个或多个单词

`*`：代指一个单词

![image-20231011144903570](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111449230.png)

##### 8.1.9.1、利用SpringAMQP演示TopicExchange的使用:palm_tree: 

实现思路如下：

1、利用@RabbitListener声明Exchange，Queue，RoutingKey

2、在consumer服务中，编写两个消费者方法，分别监听topic.queue1和topic.queue2

3、在publisher中编写测试方法，向itcast.topic发送消息

![image-20231011145232937](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111452582.png)

步骤：

1、利用@RabbitListener声明Exchange，Queue，RoutingKey

2、在consumer服务中，编写两个消费者方法，分别监听topic.queue1和topic.queue2

```java
import org.springframework.amqp.core.ExchangeTypes;
import org.springframework.amqp.rabbit.annotation.Exchange;
import org.springframework.amqp.rabbit.annotation.Queue;
import org.springframework.amqp.rabbit.annotation.QueueBinding;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/119:34
 * @function
 * @comment
 */
@Component
public class SpringRabbitListener {

    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "topic.queue1"),
            exchange = @Exchange(name = "itcast.topic", type = ExchangeTypes.TOPIC),
            key = "china.#"
    ))
    public void listenTopicQueue1(String msg)
    {
        System.out.println("消费者接收到topic.queue1的消息：[" + msg + "]");
    }

    @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "topic.queue2"),
            exchange = @Exchange(name = "itcast.topic", type = ExchangeTypes.TOPIC),
            key = "#.news"
    ))
    public void listenTopicQueue2(String msg)
    {
        System.out.println("消费者接收到topic.queue2的消息：[" + msg + "]");
    }
}
```

3、在publisher中编写测试方法，向itcast.topic发送消息

```java
@Test
public void testSendTopicExchange()
{
   String exchangeName = "itcast.topic";
   String message = "郭明然上市了不要888不要999只要9.9就能带回家";
   rabbitTemplate.convertAndSend(exchangeName, "china.news", message);
}
//--------------------------打印结果--------------------------
消费者接收到topic.queue2的消息：[郭明然上市了不要888不要999只要9.9就能带回家]
消费者接收到topic.queue1的消息：[郭明然上市了不要888不要999只要9.9就能带回家]
```

>  **总结**：
>
>  描述下Direct交换机与Topic交换机的差异?
>
>  -  Topic交换机支持通配符，不支持多个key
>  -  bindingKey：通配符，routingkey：多个单词
>  -  #：0个或多个
>  -  *：一个

#### 8.1.10、测试发送Object类型消息:evergreen_tree: 

**说明**：在SpringAMQP的发送方法中，接收消息的类型是Object，也就是说我们可以发送任意对象类型的消息，SpringAMQP会帮我们序列化为字节后发送

可以看到函数参数都是Object类型的，那么就表示可以传递任意类型的java对象进去了吗？

![image-20231011155439361](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111554481.png)

创建一个队列

我们在consumer中利用@Bean声明一个队列：

```java
import org.springframework.amqp.core.Queue;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/1111:17
 * @function
 * @comment
 */
@Configuration
public class FanoutConfig {
    @Bean
    public Queue objectQueue()
    {
        return new Queue("object.queue");
    }
}
```

查看rabbitmq的ui管理页面

![image-20231011160243062](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111602511.png)

在publisher中发送消息以测试：

```java
@Test
public void testSendObjectQueue()
{
   Map<String, Object> map = new HashMap<>();
   map.put("name", "刘桑");
   map.put("age", 18);
   rabbitTemplate.convertAndSend("object.queue", map);
}
```

查看消息发送的详细信息

![image-20231011161154919](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111611580.png)

##### 8.1.10.1、消息转换器:palm_tree: 

Spring在对消息对象的处理是由org.springframework.amqp.support.converter.MessageConverter来处理的。而默认实现是SimpleMessageConverter，基于JDK的ObjectOutputStream完成序列化。

如果要修改只需要定义一个MessageConverter类型的Bean即可。推荐用JSON方式序列化，步骤如下：

1、我们在publisher服务引入依赖

```xml
<dependency>
   <groupId>com.fasterxml.jackson.core</groupId>
   <artifactId>jackson-databind</artifactId>
</dependency>
```

2、我们在publisher服务启动类中声明MessageConverter

```java
import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.amqp.support.converter.MessageConverter;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class PublisherApplication {
    public static void main(String[] args) {
        SpringApplication.run(PublisherApplication.class);
    }
    @Bean
    public MessageConverter messageConverter()
    {
        return new Jackson2JsonMessageConverter();
    }
}
```

清空之前的队列中的消息

![image-20231011164323272](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111643872.png)

再次运行测试函数，然后查看RabbitMQ中的消息详细情况

![image-20231011164413896](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111644952.png)

消息发送的时候把对象序列化为JSON，接收时反过来反序列化为对象

![image-20231011164436624](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111644991.png)

3、消费者接受消息

我们需要再消费者里面也加上MessageConverter进行反序列化。否则取消息就会报错

```java
import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.amqp.support.converter.MessageConverter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @author Dkx
 * @version 1.0
 * @2023/10/1117:31
 * @function
 * @comment
 */
@Configuration
public class RabbitMQConfig {
    @Bean
    public MessageConverter messageConverter()
    {
        return new Jackson2JsonMessageConverter();
    }
}
```

启动消费者启动类，启动生产者的测试函数查看结果：

![image-20231011173617788](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310111736739.png)

>  **总结**:
>
>  SpringAMQP中消息的序列化和反序列化是怎么实现的？
>
>  -  利用MessageConverter实现的，默认是JDK的序列化
>  -  注意发送与接收方必须使用相同的MessageConverter
