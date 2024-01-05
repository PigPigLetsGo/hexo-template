---
title: redis-发布订阅
categories:
    - [计算机学科,databse,redis]
tags:
    - 计算机学科
    - databse
    - redis
---

## Redis发布订阅

Redis发布订阅(pub/sub)是一种==消息通信模式==:发送者(pub)发送消息, 订阅者(sub)接受消息

Redis客户端可以订阅任意数量的频道

订阅/发布消息图:

第一个:消息发送者, 第二个:频道, 第三个:消息订阅者

![image-20240105142437181](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051424264.png)

下图展示了频道channel1, 以及订阅这个频道的三个客户端--client2, client5 和client1之间的关系:

![image_2023-01-03-20-27-49](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051424633.png)

当有新消息通过PUBLISH命令发送给频道channel1时, 这个消息就会被发送给订阅它的三个客户端:

![image_2023-01-03-20-47-11](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051424455.png)

> 命令

这些命令被广泛用于构建即时通信应用, 比如网络聊天室(chatroom)和实时广播, 实时提醒等

| 序号 | 命令及描述                                                                                   |
|------|----------------------------------------------------------------------------------------------|
| 1    | <font color='red'>psubscribe</font> pattern[pattern ...]<br>订阅一个或多个符合给定模式的频道 |
| 2    | <font color='red'>pubsub</font> subcommand[argument[argument...]]<br>查看订阅与发布系统状态  |
| 3    | <font color='red'>publish</font> channel message<br>将信息发送到指定的频道                   |
| 4    | <font color='red'>punsubscribe</font> [pattern[pattern ...]]<br>退订所有给定模式的频道       |
| 5    | <font color='red'>subscribe</font> channel[channel ...]<br>订阅给定的一个或多个频道的信息    |
| 6    | <font color='red'>unsubscribe</font> [channel[channel ...]]<br>指退订给定的频道              |

> 测试

<font style="color:red">**<u>注意事项: 要使用同一个redis 也就是同一个端口号来进行测试6379订阅不能接收到6380发布的消息的只能接收到6379发布的消息,至于订阅后被阻塞还怎么发布消息这就看你自己的智商了</u>**</font> 

订阅一个频道的信息

订阅端:

![image_2023-01-03-21-21-12](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051424362.png)

将信息发送到指定的频道

发送端:

![image_2023-01-03-21-48-46](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051424581.png)

接受到的信息

![image_2023-01-03-21-47-59](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051424495.png)

一次订阅多次发送

> 原理

Redis是使用C实现的, 通过分析Redis源码里的pubsub.c文件, 了解发布和订阅机制的底层实现, 籍此加深对Redis的理解

Redis通过publish, subscribe和psubscribe等命令实现发布和订阅功能

通过subscribe命令订阅某频道后, redis-server里维护了一个字典, 字典的键就是一个个channel, 而字典的值则是一个链表, 链表中保存了所有订阅这个channel的客户端, subscribe命令的关键, 就是将客户端添加到给定channel的订阅链表中

![image_2023-01-03-22-11-29](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051424967.png)

通过publish命令向订阅者发送消息, redis-server会使用给定的频道作为键, 在它所维护的channel字典中查找记录了订阅这个频道的所有客户端的链表, 遍历这个链表, 将消息发布给所有订阅者

![image_2023-01-03-22-32-20](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051424577.png)

pub/sub从字面上理解就是发布(publish)与订阅(subscribe), 在Redis中, 你可以设定对某一个key值进行消息发布及消息订阅, 当一个key值上进行了消息发布后, 所有订阅它的客户端都会收到响应的消息, 这一功能最明显的用法就是用作实时消息系统, 比如普通的即时聊天, 群聊等功能
