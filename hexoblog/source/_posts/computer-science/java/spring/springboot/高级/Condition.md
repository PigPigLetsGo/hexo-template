---
title: Condition
categories:
   - [计算机学科,java,spring,springboot,高级]
tags:
   - 计算机学科
   - springboot高级
---

### SpringBoot 自动配置

**Condition** 

>  Condition是在Spring4.0增加的条件判断功能，通过这个功能可以实现选择性的创建Bean操作。

**思考问题：** 

SpringBoot是如何知道要创建哪个Bean的? 比如SpringBoot是如何知道要创建RedisTemplate的？我们导入了redis-starter起步依赖，然后SpringBoot就帮我创建了RedisTemplate了，那么如果我没有导入redis-starter起步依赖那SpringBoot会不会帮我创建RedisTemplate呢，SpringBoot怎么知道我到底导没导这个起步依赖呢？这些问题要解决就是==Condition==这么一个功能

```java
@SpringBootApplication
public class ConditionApplication {

    public static void main(String[] args) {
        //启动SpringBoot的应用，返回Spring的IOC容器
        ConfigurableApplicationContext context = SpringApplication.run(ConditionApplication.class, args);
        //1.获取Bean,redisTemplate
        Object redisTemplate = context.getBean("redisTemplate");
        System.out.println(redisTemplate);
    }
}
```

运行程序后报错了

![20ecbdbd016090f41f7f8cfd104c45b](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309092041404.png)

原因：没有导入redis-starter而SpringBoot找不到这个依赖就报错了

在pom.xml文件中导入redis-starter依赖

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>	
```

导入依赖后再次运行SpringBoot启动类

![1f55e7bc5ccf6a787a7b3236c02c53b](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309092041180.png)

结果：

```
org.springframework.data.redis.core.RedisTemplate@228cea97
```

**需求：** 

在SpringBoot的IOC容器中有一个User的Bean，现要求：

1. 导入Jedis坐标后，加载该Bean，没导入，则不加载。

**代码演示：** 

创建两个类

config类

```java
@Configuration
public class UserConfig {
    @Bean
    public User user() {
        return new User();
    }
}
```

User类

```java
public class User {

}
```

运行启动类获取User的Bean


```java
@SpringBootApplication
public class ConditionApplication {

    public static void main(String[] args) {
        //启动SpringBoot的应用，返回Spring的IOC容器
        ConfigurableApplicationContext context = SpringApplication.run(ConditionApplication.class, args);
        //1.获取Bean,redisTemplate
        Object user = context.getBean("user");
        System.out.println(user);
    }
}
```

运行结果

![aa1053976e4de85c34338a1d5c2d69d](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309092041025.png)

```
com.condition.conditiondemo.domain.User@3964d79
```

创建一个ClassCondition类实现Condition接口并重写方法

```java
import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.type.AnnotatedTypeMetadata;

public class ClassCondition implements Condition {

    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        return false;
    }
}
```

在UserConfig类中使用@Conditional(ClassCondition.class)来指定配置类后犹豫重写方法返回的是false表示SpringBoot找不到则运行报错。

```java
import com.condition.conditiondemo.condition.ClassCondition;
import com.condition.conditiondemo.domain.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Conditional;
import org.springframework.context.annotation.Configuration;

@Configuration
public class UserConfig {
    @Bean
    @Conditional(ClassCondition.class)
    public User user() {
        return new User();
    }
}
```

运行启动类获取User的Bean


```java
@SpringBootApplication
public class ConditionApplication {

    public static void main(String[] args) {
        //启动SpringBoot的应用，返回Spring的IOC容器
        ConfigurableApplicationContext context = SpringApplication.run(ConditionApplication.class, args);
        //1.获取Bean,redisTemplate
        Object user = context.getBean("user");
        System.out.println(user);
    }
}
```

运行结果

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309092042635.png)

如果将Condition的重写方法中改为返回true则运行SpringBoot启动类可以找到User的Bean对象不报错

### 需求1

**需求：导入jedis坐标后创建Bean** 

导入jedis坐标到pom.xml文件中

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.7.0</version>
</dependency>
```

ClassCondition类

```java
public class ClassCondition implements Condition {

    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        //1.导入jedis坐标后创建Bean
        // 思路：判断redis.clients.jedis.Jedis.class文件是否存在
        // 定义一个开关返回最终结果
        boolean flag = true;
        try{
            Class<?> cls = Class.forName("redis.clients.jedis.Jedis");
        }catch (ClassNotFoundException e) {
            flag = false;
            e.printStackTrace();
        }
        return flag;
    }
}
```

启动类

```java
@SpringBootApplication
public class ConditionApplication {

    public static void main(String[] args) {
        //启动SpringBoot的应用，返回Spring的IOC容器
        ConfigurableApplicationContext context = SpringApplication.run(ConditionApplication.class, args);
        //1.获取Bean,redisTemplate
        Object user = context.getBean("user");
        System.out.println(user);
    }
}
```

运行结果

![907e20a99ec4a72541e03490ce9449b](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309092042227.png)

### 需求2

**将类的判断定义为动态的，判断哪个字节码文件存在可以动态指定**。

#### 第一步自定义注解：

目录结构

![image-20230910195826453](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309101958066.png)

Condition类

```java
import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.type.AnnotatedTypeMetadata;

public class ClassCondition implements Condition {
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        boolean flag = true;
        try{
            Class<?> cls = Class.forName("redis.clients.jedis.Jedis");
        } catch(ClassNotFoundException e) {
            e.printStackTrace();
            flag = false;
        }
        return flag;
    }
}
```

ConditionOnClass注解类

```java
import org.springframework.context.annotation.Conditional;

import java.lang.annotation.*;

@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional(ClassCondition.class)
public @interface ConditionOnClass {
    String[] value();
}
```

UserConfig类

```java
import com.dkx.condition.ConditionOnClass;
import com.dkx.domain.User;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class UserConfig {
    @Bean
    // @Conditional(ClassCondition.class)
    @ConditionOnClass("redis.clients.jedis.Jedis")
    public User user() {
        return new User();
    }
}
```

User类

```java
public class User { }
```

SpringBoot启动类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication
public class SpringBootSeniorApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringBootSeniorApplication.class, args);
        Object user = run.getBean("user");
        System.out.println(user);
    }
}
```

运行结果：

![image-20230910200016563](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309102000918.png)

>  <span alt='solid'>总结</span>：
>
>  这样我们就可以通过注解的方式来获取自己定义的字符串或者类是否存在了而不用再Condition中写死了，那么怎获取重写方法matches中 的值 到 反射中 以 动态指定字节码文件呢？

动态指定字节码文件代码演示：

修改ClassCondition代码

```java
import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.type.AnnotatedTypeMetadata;

import java.util.Map;

public class ClassCondition implements Condition {
    /**
     * @param context 上下文对象，用于获取环境，IOC容器，ClassLoader对象
     * @param metadata 注解元对象，可以用于获取注解定义的属性值
     * @return
     */
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        // 导入通过注解属性值value指定坐标后创建Bean
        // 获取注解属性 value
        Map<String, Object> map = metadata.getAnnotationAttributes(ConditionOnClass.class.getName());
        //System.out.println(map); // map 集合 输出的结果 里面是指定的路径 {value=[redis.clients.jedis.Jedis]}
        String[] value = (String[]) map.get("value");
        boolean flag = true;
        try{
            for(String item : value) {
                Class<?> cls = Class.forName(item);
            }
        } catch(ClassNotFoundException e) {
            e.printStackTrace();
            flag = false;
        }
        return flag;
    }
}
```

运行结果：

![image-20230910204712670](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309102047406.png)

这样就完成了动态的指定路径获取字节码文件了

那么这个功能有什么用呢？

我们在回过头来看 如下问题：

>  SpringBoot是如何知道要创建哪个Bean的？比如SpringBoot是如何知道要创建RedisTemplate的？

其实就是通过Condition来判断有没有导入Redis的Starter如果导入了则帮我们创建，SpringBoot中给我们提供了非常多的ConditionalOnXXX的接口

在spring-boot-autoconfigure中的condition目录中可以看到很多的ConditionalOnXXX的接口可以使用

![image-20230910205831361](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309102101741.png)

随便演示一个ConditionalOnXXX接口比如演示 ConditionalOnProperty这个接口

<span alt='solid'>功能</span>：当配置文件中的键和值与注解指定的键和值存在且对应则加载Bean

```java
import com.dkx.domain.User;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class UserConfig {
    @Bean
    @ConditionalOnProperty(name = "dkx", havingValue = "666")
    public User user() {
        return new User();
    }
}
```

配置文件

![image-20230910210519974](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309102105295.png)

运行启动类

```java
@SpringBootApplication
public class SpringBootSeniorApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringBootSeniorApplication.class, args);
        Object user = run.getBean("user");
        System.out.println(user);
    }
}
```

运行结果：

![image-20230910210458883](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309102105642.png)

将配置文件中添加注解对应的键和值

![image-20230910210601049](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309102106608.png)

运行结果

![image-20230910210731051](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309102107000.png)

>  Condition-小结：
>
>  1.  自定义条件：
>      1.  定义条件类：自定义类实现Condition接口，重写matches方法，在metches方法中进行逻辑判断，返回boolean值，matches方法两个参数：
>          1.  context：上下文对象，可以获取属性值，获取类加载器，获取BeanFactory等。
>          2.  metadata：元数据对象，用于获取注解属性
>      2.  判断条件：在初始化Bean时，使用<span alt='solid'>@Conditional (条件类.class) </span>注解
>  2.  SpringBoot提供的常用条件注解：
>      1.  <span alt='solid'>ConditionalOnProperty</span>：判断配置文件中是否有对应属性和值才初始化Bean
>      2.  <span alt='solid'>ConditionalOnClass</span>：判断环境中是否有对应字节码文件才初始化Bean
>      3.  <span alt='solid'>ConditionalOnMissingBean</span>：判断环境中没有对应Bean才初始化Bean



