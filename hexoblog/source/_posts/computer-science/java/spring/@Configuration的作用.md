---
title: Configuration的作用
categories:
   - [计算机学科,java,spring]
tags:
   - 计算机学科
   - spring
   - 基础
---

# 一,问题描述

在项目中，通常都会配置一个或者多个加了@[Configuration](https://so.csdn.net/so/search?q=Configuration&spm=1001.2101.3001.7020)注解的配置类，那么@Configuration这个注解到底有神马作用勒？看下面案例

Config类

```java
@ComponentScan("com")
public class SpringMVCConfig {
   //该方法返回是一个bean所以需要加上@Bean注解
    @Bean
    public BookDao getBookDao(){
        return new BookDao();
    }
}
```

Dao类

```java
public class BookDao {
    public BookDao(){
        System.out.println("bookDao constructor runing...");
    }
}
```

test测试代码

```java
public class SpringTest {
    @Test
    public void test(){
       //获取IoC容器
        AnnotationConfigApplicationContext a = new AnnotationConfigApplicationContext(SpringMVCConfig.class);
    }
}
//--------------------------Result--------------------------
bookDao constructor runing...
```

执行上面的代码，我们会发现当我们不加@Configuration这个注解的时候我们的BookDao 这个类还是还是会被实例化，也会打印bookDao constructor runing...。我们的spring环境也可以正常运行。

再看下面案例:

config类

```java
public class SpringMVCConfig {
    @Bean
    public BookDao getBookDao(){
        return new BookDao();
    }
    @Bean
    public BookDao1 getBookDao1(){
        getBookDao();
        return new BookDao1();
    }
}
```

Dao和Dao1类

```java
//BookDao
public class BookDao {
    public BookDao(){
        System.out.println("bookDao constructor runing...");
    }
}
//BookDao1
public class BookDao1 {
    public BookDao1(){
        System.out.println("BookDao1 constructor runing...");
    }
}
```

test测试代码

```java
public class SpringTest {
    @Test
    public void test(){
        AnnotationConfigApplicationContext a = new AnnotationConfigApplicationContext(SpringMVCConfig.class);
    }
}
//-------------------------Result-------------------------
bookDao constructor runing...
bookDao constructor runing...
BookDao1 constructor runing...
```

不加@Configuration打印的结果bookDao打印了两次，那我们加上@Configuration看情况

```java
@Configuration
public class SpringMVCConfig {
    @Bean
    public BookDao getBookDao(){
        return new BookDao();
    }
    @Bean
    public BookDao1 getBookDao1(){
        getBookDao();
        return new BookDao1();
    }
}
//----------------------------Result----------------------------
bookDao constructor runing...
BookDao1 constructor runing...
```

加上@Configuration后bookDao只打印了一次

# 分析

从表面来看，当我们不加@Configuration注解的时候，我们的BookDao会被实例化两次，这违背了我们spring默认单例的设计原则，当加上我们的@Configuration注解的时候，BookDao只被实例化了一次。那么其底层到底做了什么，让我们来深追一下spring源码吧。
当我们解析beanAppcofig的时候，会给它的一个属性标识为Full，表明它是一个全注解类。
![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/20190904003822882.png)

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/20190904003841858_20230417121926.png)

然后在我们调用ConfigurationClassPostProcessor.postProcessBeanFactory()方法的时候会去判断我们的bean工厂当中是否有bean需要进行cglib代理。

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681704933572-5_20230417122031.png)

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681704941383-8_20230417122129.png)

然后遍历configBeanDefs这个map

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681704956508-11_20230417122144.png)

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681704968471-14_20230417122200.png)

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681704975527-17_20230417122217.png)

cglib代理主要是对我们的方法进行拦截增强；当我们执行AppConfig中的方法的时候会去执行cglib代理类中的代理方法，主要就是callBacks中的方法。

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681704988545-20_20230417122231.png)

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681704993782-23_20230417122248.png)

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681704999909-26_20230417122303.png)

**isCurrentlyInvokedFactoryMethod(beanMethod)) 会判断我们的执行方法和我们的调用方法是否是同一个；如果是同一个就调用父类的方法进行new；如果不是就调用$$beanFactory.getBean()获取。** 

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681705012799-29_20230417122323.png)

# 总结

加上@Configuration注解主要是给我们的类加上了cglib代理。在执行我们的配置类的方法时，会执行cglib代理类中的方法，其中有一个非常重要的判断，当我们的执行方法和我们的调用方法是同一个方法时，会执行父类的方法new（cglib代理基于继承）；当执行方法和调用方法不是同一个方法时会调用beanFactory.getBean获取。
![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681705034246-32_20230417122340.png)

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpdXoxMDI0%2Csize_16%2Ccolor_FFFFFF%2Ct_70-1681705039670-35_20230417122357.png)
