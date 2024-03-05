---
title: spring整合Junit
categories:
    - [计算机学科,java,spring,整合jar包]
tags:
    - 计算机学科
    - spring
    - 整合jar包
    - Junit
---

# Spring整合Junit

1.导入maven依赖

```xml
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-test</artifactId>
          <version>5.2.11.RELEASE</version>
      </dependency>
```

2.在测试类上加上注解,类运行器:@RunWith(SpringJUnit4ClassRunner.class)

```java
@SuppressWarnings("all")
@RunWith(SpringJUnit4ClassRunner.class)
public class SpringUserTest {
}
```

3.配置Spring环境上下文配置@ContextConfiguration(classes = SpringConfig.class)

```java
@SuppressWarnings("all")
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class SpringUserTest {
}
```

完整测试类代码

```java
@SuppressWarnings("all")
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class SpringUserTest {
    @Autowired
    private UserService userService;
    @Test
    public void test(){
        List<SpringMybatis> list = userService.getList();
        for(SpringMybatis i:list){
            System.out.println(i);
        }
    }
}
```

Run Result

![image_2023-02-26-14-11-04](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040858304.png)
