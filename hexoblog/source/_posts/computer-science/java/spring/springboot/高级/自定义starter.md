---
title: 自定义starter起步依赖
categories:
   - [计算机学科,java,spring,springboot,高级]
tags:
   - 计算机学科
   - springboot高级
---

## 自定义starter

>  <span alt='solid'>需求</span>：
>
>  自定义redis-starter。要求当导入redis坐标时，SpringBoot自定创建Jedis的Bean。

我们知道SpringBoot提供了很多很多的starter起步依赖，但是有些起步依赖并没有提供。而是由某个技术自己写的它希望和SpringBoot整合它自己写的starter起步依赖。

比如说Mybatis就是这样做的，自己写的起步依赖让SpringBoot整合一下，接下来可以参考Mybatis的做法找到对应的思路。

<span alt='solid'>在pom.xml中引入Mybatis的起步依赖</span>：

```xml
<dependency>
   <groupId>org.mybatis.spring.boot</groupId>
   <artifactId>mybatis-spring-boot-starter</artifactId>
   <version>1.3.2</version>
</dependency>
```

一般SpringBoot官方提供的起步依赖功能名写在最后面比如test，而一般第三方提供的起步依赖功能名写在最前面比如mybatis

![image-20230913150659200](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131514336.png)

mybatis起步依赖里面包含的坐标点击进去查看

比如其中的坐标如下：

```xml
<dependency>
   <groupId>org.mybatis.spring.boot</groupId>
   <artifactId>mybatis-spring-boot-autoconfigure</artifactId>
</dependency>
```

从其名得其意 ， 这个坐标哦就是mybatis自动配置的一个坐标。当我们引入到mybatis的starter起步依赖的坐标后，自动配置和其它的依赖就能加载得到了

在mybatis的起步依赖中其实有什么功能代码也没有，只是将起步依赖中的所有坐标进行了整合

![image-20230913151926826](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131519634.png)

我们关键看mybatis-spring-boot-autoconfigure依赖，其中的代码就比较多了。

![image-20230913152049774](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131520757.png)

比如说我们可以看下MybatisAutoConfiguration这个类，这个类就是Mybatis的自动配置类

![image-20230913152330440](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131523827.png)

那么这个配置类要能够被SpringBoot所识别从而加载这个 配置类 里面定义的Bean的话，那么它的做法就是在META-INF中定义了一个spring.factories的文件

![image-20230913152640934](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131526086.png)

这样就能够在启动SpringBoot的时候读取到这个配置文件，从而加载Mybatis相关的Bean到IOC容器中进行使用。	

## 实现步骤

1.  创建redis-spring-boot-autoconfigure模块
2.  创建redis-spring-boot-starter模块，依赖redis-spring-boot-autoconfigure的模块
3.  在redis-spring-boot-autoconfigure模块中初始花Jedis的Bean，并定义META-INF/spring.factories文件
4.  创建redis-spring-enable模块是redis-spring-boot-autoconfigure的子模块

模块结构：

![image-20230913170937301](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131709586.png)

子模块的目录结构：

![image-20230913171044858](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131710300.png)

<span alt='solid'>RedisProperties类</span>.

通过注解ConfigurationProperties读取以redis前缀命名的配置文件的内容映射到字段上，并且如果没有配置信息则会有默认值就是本机

```java
import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "redis")
public class RedisProperties {
    // 主机名
    private String host = "localhost";
    // 端口号
    private int port = 6379;

    public String getHost() {
        return host;
    }

    public void setHost(String host) {
        this.host = host;
    }

    public int getPort() {
        return port;
    }

    public void setPort(int port) {
        this.port = port;
    }
}
```

<span alt='solid'>RedisAutoConfiguration类</span>.

使用注解：EnableConfigurationProperties将指定的RedisProperties读取配置文件类注册到IOC容器当中，ConditionalOnClass判断是否有Jedis的依赖再加载类中定义的Bean，注解：ConditionalOnMissingBean(name = "jedis")判断用户是否定义了名叫jedis的Bean如果没有则使用我们定义的，如果有则使用用户定义的。

```java
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import redis.clients.jedis.Jedis;

@Configuration
@EnableConfigurationProperties(RedisProperties.class)
// 如果有Jedis的再加载Bean
@ConditionalOnClass(Jedis.class)
public class RedisAutoConfiguration {
    /**
     * 提供jedis的Bean
     */
    // 如果用户自己定义了jedis的Bean则使用用户定义的
    // ConditionalOnMissingBean如果没有名称叫jedis的Bean的时候再提供我们的jedis的Bean
    @Bean
    @ConditionalOnMissingBean(name = "jedis")
    public Jedis jedis (RedisProperties redisProperties) {
        System.out.println("RedisAutoConfiguration ... ");
        return new Jedis(redisProperties.getHost(), redisProperties.getPort());
    }
}
```

找到SpringBoot启动时会自动读取配置文件来加载Bean的位置@EnableAutoConfiguration

![image-20230913171908801](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131719444.png)

进入到AutoConfigurationImportSelector中

![image-20230913171837565](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131718374.png)

复制这段

![image-20230913171947375](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131719997.png)

在 redis-spring-boot-starter 模块的 resources 下创建 META-INF /spring.factories 配置文件，这个配置文件就是SpringBoot启动时就会读取的然后加载里面配置的依赖项

spring.factories 内容：

其中的 \ 表示，这段代码太长了 换下行而已

```yml
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  com.dkx.config.RedisAutoConfiguration
```

SpringBoot启动类

位置：redis-spring-enable \ src \ main \ java \ com \ dkx \  RedisSpringEnableApplication 

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.Bean;
import redis.clients.jedis.Jedis;

@SpringBootApplication
public class RedisSpringEnableApplication {
	public static void main(String[] args) {
		ConfigurableApplicationContext run = SpringApplication.run(RedisSpringEnableApplication.class, args);
		Jedis jedis = run.getBean(Jedis.class);
		jedis.set("username", "liusang");
		String value = jedis.get("username");
		System.out.println(value);
	}
}
```

运行结果：

![image-20230913172626902](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131726470.png)

如果用户自己定义了jedis这个Bean的话

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.context.annotation.Bean;
import redis.clients.jedis.Jedis;

@SpringBootApplication
public class RedisSpringEnableApplication {
	public static void main(String[] args) {
		ConfigurableApplicationContext run = SpringApplication.run(RedisSpringEnableApplication.class, args);
		Jedis jedis = run.getBean(Jedis.class);
		jedis.set("username", "liusang");
		String value = jedis.get("username");
		System.out.println(value);
	}

	// 重写jedis的Bean如果我们自定义的没有打印内容说明注解：ConditionalOnMissingBean 生效
	@Bean
	public Jedis jedis() {
		return new Jedis("localhost", 6379);
	}
}
```

运行结果：

![image-20230913172724154](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131727375.png)

而且我们也可以自己来改yml中的配置比如改一下端口号，但是注意：将用户定义的jedis注释掉因为会使用它的就不会使用我们自己定义的了所以配置文件就等于白配置

```yml
redis:
  port: 666
```

运行结果：

![image-20230913173001307](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131730408.png)

报错信息是端口号不对