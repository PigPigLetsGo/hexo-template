---
title: SpringBoot排除自动配置的4个方法
categories:
   - [计算机学科,java,spring,springboot]
tags:
   - 计算机学科
   - springboot
   - 基础
   - 知识点
---

# SpringBoot排除自动配置的4个方法

## 方法1

使用@SpringBootApplication注解，用exclude属性进行排除指定的类：

```java
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
public class Application {
    // ...
}
```

## 方法2

单独使用@EnableAuthConfiguration注解的时候：

```java
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class})
public class Application {
    // ...
}
```

## 方法3

使用@SpringCloudApplication注解的时候：

```java
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class})
@SpringCloudApplication
public class Application {
    // ...
}
```

## 方法4

中级方案，不管是SpringBoot还是SpringCloud都可以搞定，在配置文件中指定参数

spring.authconfigure.exclude进行排除：

```properties
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

或者还可以这样写：

```properties
spring.autoconfigure.exclude[0]=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

yml配置格式：

```yaml
spring:     
  autoconfigure:
    exclude:
      - org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

