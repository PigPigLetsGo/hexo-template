---
title: Import注解
categories:
   - [计算机学科,java,spring,springboot,高级]
tags:
   - 计算机学科
   - springboot高级
---

@EnableXXX注解的底层其实就是使用了@Import注解的功能

## @Import注解

@Enable*底层依赖于@Import注解导入一些类，使用@Import导入的类会被Spring加载到IOC容器中。而@Import提供4种用法：

1.  导入Bean
2.  导入配置类
3.  导入ImportSelector实现类。一般用于加载配置文件中的类
4.  导入ImportBeanDefinitionRegistrar实现类。

### 导入Bean

项目结构：

![image-20230913093726083](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309130937266.png)

![image-20230913093743925](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309130937861.png)

![image-20230913093804763](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309130938320.png)

User类与Role类

```java
public class User { }

public class Role { }
```

启动类

```java
// @ComponentScan 扫描范围：当前引导类所在包及其子包
// com/dkx1
// com/dkx
// 两个路径同级路径项目可以被扫描到
@Import({User.class, Role.class})
@SpringBootApplication
public class SpringBootEnableApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringBootEnableApplication.class, args);
        // Import导入了User但是名称不确定所以使用类的类型来获取Bean
        User user = run.getBean(User.class);
        System.out.println(user);
        System.out.println("-------------------------");
        Role role = run.getBean(Role.class);
        System.out.println(role);

        System.out.println("-----------获取Bean的名字-----------");
        Map<String, User> users = run.getBeansOfType(User.class);
        System.out.println(users); // {com.dkx.domain.User=com.dkx.domain.User@3241713e}
        System.out.println("-------------------------");

        System.out.println("-----------通过获取到Bean的名称来获取Bean-----------");
        Object bean = run.getBean("com.dkx.domain.User");
        System.out.println(bean);

    }
}
```

运行结果：

![image-20230913093839900](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309130938112.png)

### 导入配置类

User类和Role类

```java
public class Role { }

public class User { }
```

UserConfig类

```java
public class UserConfig {
    @Bean
    public User user() {
        return new User();
    }

    @Bean
    public Role role() {
        return new Role();
    }
}
```

启动类

```java
// @ComponentScan 扫描范围：当前引导类所在包及其子包
// com/dkx1
// com/dkx
// 两个路径同级路径项目可以被扫描到
@Import(UserConfig.class)
@SpringBootApplication
public class SpringBootEnableApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringBootEnableApplication.class, args);
        System.out.println("==========UserBean==========");
        Object user = run.getBean("user");
        System.out.println(user);
        System.out.println("==========RoleBean==========");
        Object role = run.getBean("role");
        System.out.println(role);
    }
}
```

运行结果：

![image-20230913094442665](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309130944948.png)

### 导入ImportSelector实现类

创建一个MyImportSelector类

![image-20230913102744218](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131027362.png)

编写MyImportSelector类

```java
public class MyImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        // 配置的Bean的路径是一个字符串意味着可以不用写死了,可以动态的加载类
        return new String[]{"com.dkx.domain.Role", "com.dkx.domain.User"};
    }
}
```

启动类

```java
// @ComponentScan 扫描范围：当前引导类所在包及其子包
// com/dkx1
// com/dkx
// 两个路径同级路径项目可以被扫描到
@Import(MyImportSelector.class)
@SpringBootApplication
public class SpringBootEnableApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringBootEnableApplication.class, args);
        System.out.println("==========UserBean==========");
        User user = run.getBean(User.class);
        System.out.println(user);
        System.out.println("==========RoleBean==========");
        Role role = run.getBean(Role.class);
        System.out.println(role);
    }
}
```

运行结果：

![image-20230913103028478](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131030912.png)

### 导入ImportBeanDefinitionRegistrar实现类

创建一个ImportBeanDefinitionRegistrar实现类

![image-20230913104515744](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131045445.png)

编写ImportBeanDefinitionRegistrar实现类代码注册Bean

```java
public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {

    /**
     * @param importingClassMetadata annotation metadata of the importing class
     * @param registry current bean definition registry 向IOC中注入某一些对象
     */
    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        // 构建Bean
        AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.rootBeanDefinition(User.class).getBeanDefinition();
        // 注册Bean
        // 参数1：Bean的名称，参数2：Bean类型
        registry.registerBeanDefinition("user", beanDefinition);
    }

}
```

启动类

```java
// @ComponentScan 扫描范围：当前引导类所在包及其子包
// com/dkx1
// com/dkx
// 两个路径同级路径项目可以被扫描到
@Import(MyImportBeanDefinitionRegistrar.class)
@SpringBootApplication
public class SpringBootEnableApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(SpringBootEnableApplication.class, args);
        System.out.println("==========UserBean==========");
        User user = run.getBean(User.class);
        System.out.println(user);
        System.out.println("==========使用注册时的名称来获取Bean==========");
        // 在MyImportBeanDefinitionRegistrar中注册Bean的时候定义了Bean的名称为user
        Object user1 = run.getBean("user");
        System.out.println(user1);
    }
}
```

运行结果：

![image-20230913104646971](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309131048269.png)
