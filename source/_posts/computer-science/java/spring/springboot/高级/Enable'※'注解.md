---
title: Eable* 注解
categories:
   - [计算机学科,java,spring,springboot,高级]
tags:
   - 计算机学科
   - springboot高级
---

## @Eable*注解

SpringBoot中提供了很多Enable开头的注解，这些注解都是用于动态启用某些功能的。而其底层原理是使用@Import注解导入一些配置类，实现Bean 的动态加载。

>  <span alt='solid'>思考</span>：
>
>  SpringBoot工程是否可以直接获取jar包中定义的Bean?
>
>  答案：不可以
>
>  如果不可以这个地方问题就非常严重了。比如说redisTemplate获取，是不是人家jar包里面定义的Bean啊。那为什么我的工程引入了redis的起步依赖我就直接可以获取到了呢 ？

**首先演示不能获取第三方jar包里面定义的Bean的特点**：

创建一个父模块，两个同级子模块

![image-20230911215125989](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309112151227.png)

将enable-other模块导入到enable模块中使用

```xml
<dependency>
   <groupId>com.dkx</groupId>
   <artifactId>SpringBoot-enable-other</artifactId>
   <version>0.0.1-SNAPSHOT</version>
</dependency>
```

install,enable-other模块否则报错找不到XXX类

![image-20230912074640515](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309120746815.png)

编写enable模块

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

// @ComponentScan 扫描范围：当前引导类所在包及其子包
// com/dkx1
// com/dkx 
// 1.使用@ComponentScan扫描，com.dkx.包
// 2.可以使用@Impoert注解，加载类
@SpringBootApplication
public class SpringBootEnableApplication {

	public static void main(String[] args) {
		ConfigurableApplicationContext run = SpringApplication.run(SpringBootEnableApplication.class, args);
		Object user = run.getBean("user");
		System.out.println(user);
	}
}
```

项目结构：

![image-20230911215325372](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309112153017.png)

编写enable-other模块

config/UserConfig

```java
@Configuration
public class UserConfig {
    @Bean
    public User user() {
        return new User();
    }
}
```

domain / User

```java
public class User { }
```

项目结构：

![image-20230911215345356](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309112153209.png)

运行结果：

![image-20230911215412559](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309112154774.png)

#### 第一种方式

报错找不到user这个Bean，我们可以通过下面的方式来解决。

首先要知道这个问题其实很简单，规则是子模块中路面相同的话就不会报错了，可以理解为两个模块可以看做一个模块的目录来构建就不会报错了。

```java
import com.dkx.config.UserConfig;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Import;

// @ComponentScan 扫描范围：当前引导类所在包及其子包
// com/dkx1
// com/dkx
// 1.使用@ComponentScan扫描，com.dkx.包
// 2.可以使用@Impoert注解，加载类
@SpringBootApplication
@ComponentScan("com.dkx.config")
//@Import(UserConfig.class)
public class SpringBootEnableApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringBootEnableApplication.class, args);
        Object user = run.getBean("user");
        System.out.println(user);
    }
}
```

运行结果：

![image-20230911220053181](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309112200456.png)

#### 第二种方式

使用自定义注解的方式在自定义注解类使用Import导入UserConfig类然后在启动类中添加该自定义注解 简化获取User对象的操作

User

```java
public class User { }
```

UserConfig

```java
@Configuration
public class UserConfig {
    @Bean
    public User user() {
        return new User();
    }
}
```

EnableUser

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(UserConfig.class)
public @interface EnableUser {
}
```

启动类

```java
// @ComponentScan 扫描范围：当前引导类所在包及其子包
// com/dkx1
// com/dkx
// 两个路径同级路径项目可以被扫描到
@SpringBootApplication
// 简化操作
@EnableUser
public class SpringBootEnableApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringBootEnableApplication.class, args);
        Object user = run.getBean("user");
        System.out.println(user);
    }
}
```

运行结果：

![image-20230913085728841](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309130857302.png)

#### 第三种方式

还可以更下目录来解决问题

目录结构：

![image-20230912073845767](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309120739006.png)

启动类代码：

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.ComponentScan;

// @ComponentScan 扫描范围：当前引导类所在包及其子包
// com/dkx
// com/dkx
// 两个路径同级路径项目可以被扫描到
@SpringBootApplication
public class SpringBootEnableApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringBootEnableApplication.class, args);
        Object user = run.getBean("user");
        System.out.println(user);
    }
}
```

运行结果：

![image-20230912073932170](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309120739177.png)