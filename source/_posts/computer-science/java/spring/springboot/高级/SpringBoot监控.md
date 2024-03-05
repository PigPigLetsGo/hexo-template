---
title: SpringBoot监控
categories:
   - [计算机学科,java,spring,springboot,高级]
tags:
   - 计算机学科
   - springboot高级
---

## SpringBoot监控

>  <span alt='solid'>概述</span>：
>
>  SpringBoot自带监控功能Actuator，可以帮助实现对程序内部运行情况监控，比如监控状况，Bean加载情况，配置属性，日志信息等。

使用步骤：

1.  导入依赖

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
    ```

    或者在创建SpringBoot的时候进行勾选该依赖

    ![image-20230914090657267](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309140906622.png)

2.  访问：http://localhost:8080/acruator

访问路径后返回了一些json字符串，我们去找一个网站解析一下

![image-20230914091151109](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309140911642.png)

json字符串解析后的格式

![image-20230914091254472](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309140912118.png)

访问路径 http://localhost:8080/actuator/health

表示当前应用正常DOWN就是失败了有问题了，默认没有开启完整信息的，因为不是很安全

![image-20230914091616106](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309140916704.png)

在application.yml配置文件中开启完整信息

```yml
# 开启健康检查的完整信息
management:
   endpoint:
     health:
       show-details: always
```

访问结果：

![image-20230914092454794](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309140924716.png)

导入redis的坐标

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

然后redis本机没有启动，它默认会链接本机的redis但是没有启动就会报错。我们查看SpringBoot监控的情况中redis是处于什么状态

![image-20230914094309384](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309140943062.png)

其它的配置

```yml
# 将所有的监控endpoint暴漏出来 可以理解为：一个endpoints就是一个url
management:
  endpoints:
    web:
      exposure:
        include: "*" #这里需要注意 * 号需要加上双引号包裹起来, 否则启动报错
```

访问结果：

![image-20230914100445727](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141004121.png)

## SpringBootAdmin

SpringBootAdmin是一个开源社区项目，用于管理和监控SpringBoot应用程序。

SpringBootAdmin有两个角色，客户端(Client)和服务端(Server)。

应用程序作为SpringBootAdminClient向为SpringBootAdminServer注册

SpringBootAdminServer的UI界面将SpringBootAdminClient的ActuatorEndpoint上的一些监控信息

这个东西的使用有两端，所以我们要创建两端

![image-20230914101003770](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141010968.png)

创建admin-server模块

![image-20230914101337604](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141013377.png)

server依赖包含了actuator所以不用选择了

因为本机的端口号默认是8080，需要将两端的端口号分别开来否则就会出问题：

server的yml配置文件

```yml
server:
  port: 8081

```

client的yml配置文件

```yml
# 执行admin-server地址
spring:
  boot:
    admin:
      client:
        url: "http://localhost:8081"
management:
  endpoint:
    health:
      show-details: always # 健康检查的完整信息
  endpoints:
    web:
      exposure:
        include: "*" # 开启所有配置

```

启动server的启动类

然后再启动client的启动类

![image-20230914104325433](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141043830.png)

启动之后我们直接访问server就可以看到client中的所有信息了。

![image-20230914104507076](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141045216.png)

点击应用墙就可以看到启动的时间

![image-20230914104622026](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141046919.png)

点击后就可以看到里面的详细信息

![image-20230914104700110](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141047560.png)

里面有健康信息和其它的一些信息

![image-20230914104727301](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309141047714.png)

在client模块中添加一个UserController控制类

```java
@RestController
public class UserController {
    @GetMapping("/str")
    public String getStr() {
        return "Hello,World";
    }
}
```

重启代码

