---
title: Java监听机制
categories:
   - [计算机学科,java,spring,springboot,高级]
tags:
   - 计算机学科
   - springboot高级
---

## Java监听机制

SpringBoot的监听机制，其实是对Java提供的事件监听机制的封装。

<span alt='solid'>Java中的事件监听机制定义了以下几个角色</span>：

1.  **事件**：Event，继承java.util.EventObject类的对象
2.  **事件源**：Source，任意对象Object
3.  **监听器**：Listener，实现java.util.EventListener接口的对象

<span alt='solid'>**SpringBoot监听机制**</span>：

SpringBoot在项目启动时，会对几个监听器进行回调，我们可以实现这些监听器接口，在项目启动时完成一些操作

SpringBoot中监听器分为：ApplicationContextInitializer，SpringApplicationRunListener，CommandLineRunner，ApplicationRunner

项目结构：

![image-20230913201836361](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309132018398.png)

ApplicationContextInitializer

```java
import org.springframework.context.ApplicationContextInitializer;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.stereotype.Component;

@Component
public class MyApplicationContextInitializer implements ApplicationContextInitializer {
    @Override
    public void initialize(ConfigurableApplicationContext applicationContext) {
        System.out.println("ApplicationContextInitializer...initialize");
    }
}
```

SpringApplicationRunListener

```java
import org.springframework.boot.ConfigurableBootstrapContext;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.SpringApplicationRunListener;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.core.env.ConfigurableEnvironment;

import java.time.Duration;

public class MySpringApplicationRunListener implements SpringApplicationRunListener {

    // 需要重写构造函数否则报错初始化异常
    public MySpringApplicationRunListener(SpringApplication application, String[] args) {}

    @Override
    public void starting(ConfigurableBootstrapContext bootstrapContext) {
        System.out.println("starting...项目启动中");
    }

    @Override
    public void environmentPrepared(ConfigurableBootstrapContext bootstrapContext,
                                     ConfigurableEnvironment environment) {
        System.out.println("environmentPrepared...环境对象开始准备");
    }

    @Override
    public void contextPrepared(ConfigurableApplicationContext context) {
        System.out.println("contextPrepared...上下文对象开始准备");
    }

    @Override
    public void contextLoaded(ConfigurableApplicationContext context) {
        System.out.println("contextLoaded...上下文对象开始加载");
    }

    @Deprecated
    public void started(ConfigurableApplicationContext context) {
        System.out.println("started...上下文对象加载完成");
    }

    @Deprecated
    public void running(ConfigurableApplicationContext context) {
        System.out.println("running...项目启动完成, 开始运行");
    }

    public void failed(ConfigurableApplicationContext context, Throwable exception) {
        System.out.println("failed...项目启动失败");
    }

    @Override
    public void started(ConfigurableApplicationContext context, Duration timeTaken) {
        started(context);
        System.out.println("started...");
    }

    public void ready(ConfigurableApplicationContext context, Duration timeTaken) {
        running(context);
        System.out.println("ready...");
    }
}
```

CommandLineRunner

```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Component
public class MyCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("CommandLineRunner...run");
        System.out.println(Arrays.toString(args));
    }
}
```

ApplicationRunner

```java
import org.springframework.boot.ApplicationArguments;
import org.springframework.boot.ApplicationRunner;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Component
public class MyApplicationRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("ApplicationRunner ... run");
        System.out.println(Arrays.toString(args.getSourceArgs()));
    }
}
```

spring.factories

```yml
org.springframework.context.ApplicationContextInitializer=\
  com.dkx.listener.MyApplicationContextInitializer
org.springframework.boot.SpringApplicationRunListener=\
  com.dkx.listener.MySpringApplicationRunListener
```

启动类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringbootListenerApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootListenerApplication.class, args);
    }

}
```

运行结果：

![image-20230913202225065](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309132022394.png)