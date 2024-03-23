---
title: 注解@RequiredArgsConstructor详解
categories:
   - [计算机学科,java,案例Demo]
tags:
   - 计算机学科
   - java
   - 案例Demo
   - lombok
   - 项目
---

# 注解@RequiredArgsConstructor详解

**作用**：忽略某些字段生成构造器

Lombok提供的 @RequiredArgsConstructor 方式（会生成一个包含常量，和标识了NotNull的变量的构造方法）
在springboot项目中，controller或service层中需要注入多个mapper接口或者另外的service接口，这时候代码中就会有多个@AutoWired注解，使得代码看起来什么的混乱。
**lombok提供了一个注解**：
@RequiredArgsConstructor(onConstructor =@_(@Autowired))
写在类上面可以代替@AutoWired注解，==需要注意的是==：在注入的时候需要用final定义，或者使用@notnull注解

![image-20240316212600141](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240316212600141.png)

## 1.注解注入:

```java
Controller
public class FooController {
  @Autowired
  //@Inject
  private FooService fooService;
  //简单的使用例子，下同
  public List<Foo> listFoo() {
      return fooService.list();
  }
}
```

## 2.构造器注入:

```java
Controller

public class FooController {
  private final FooService fooService; 
  @Autowired
  public FooController(FooService fooService) {
      this.fooService = fooService;
  }
  //使用方式上同，略
}
```

## 3.setter注入:

```java
@Controller
public class FooController {
  private FooService fooService;
  //使用方式上同，略
  @Autowired
  public void setFooService(FooService fooService) {
      this.fooService = fooService;
  }
}
```

## 4.最后就是lombok中的@RequiredArgsConstructor

```java
@RequiredArgsConstructor
public class VerifyController {
    private final VerifyService verifyService;
    private final InvitationService invitationService;
    private final VerificationCodeService verificationCodeService;
}
```

