---
title: SpringMVC
categories:
    - [计算机学科,java,spring,springmvc]
tags:
    - 计算机学科
    - springmvc
---

# SpringMVC

**1. 什么是MVC** 

MVC是一种软件架构的思想,将软件按照模型,视图,控制器来划分

M: Model,模型层,指工程中的javaBean,作用是处理数据

JavaBean分为两类:

- 一类称为实体类Bean: 专门存储业务数据的,如Student,User等

- 一类称为业务处理Bean: 指Service或Dao对象,专门用于处理业务逻辑和数据访问

V: View,视图层,指工程中的html或jsp等页面,作用是与用户进行交互,展示数据

C: Controller,控制层,指工程中的Servlet,作用是接收请求和响应浏览器

MVC的工作流程:

用户通过视图层发送请求到服务器,在服务器中请求被Controller接收,Controller调用相应的Model层处理请求,处理完毕将结果返回到Controller,Controller再根据请求处理的结果找到相应的View视图,渲染数据后最终响应给浏览器

## Spring概述

- SpringMVC技术与Servlet技术功能等同,均属于web层开发技术

当前web程序的工作流程

web 通过浏览器访问页面,前端页面使用异步提交的方式发送到后端服务器,后端服务器采用,表现层,业务层,数据层的三层架构的形式进行开发,页面发送的请求由表现层接收,获取用户的请求参数后,将参数传递到业务层再由业务层访问数据层得到用户需要访问的数据后将数据返回给表现层

![![image_2023-02-28-09-55-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308231502785.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-02-28-09-55-34_20230228095922.png?v=1&type=image&token=V1:mM4SRRKC4ipI2jAbTv_XKB0Lvj3FXM3SLxSyTJlV5dI)

表现层拿到数据后将数据转换成json格式发送给前端页面,前端页面接收数据后解析数据并组织成用户浏览的最终页面信息交给浏览器

![![image_2023-02-28-10-04-48](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308231502121.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-02-28-10-04-48_20230228114047.png?v=1&type=image&token=V1:lPxGAY3PyF3SDnLjjy4wO3gbPVodnCklfxcALZQRTNg)

- SpringMVC是一种基于Java实现MVC模型的轻量级web框架

- 优点
    - 使用简单，开发便捷（相比于Servlet）

- 灵活性强

![![image_2023-02-28-10-09-22](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104087.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-02-28-10-09-22_20230228114059.png?v=1&type=image&token=V1:zipI1kWUI2CjRJWAdJ7YDmvma9-wlq57vyvpwhD5jdU)

- 小结
    - SpringMVC是一种表现层框架技术
    - SpringMVC用于进行表现层功能开发

## 操作步骤

1.使用SpringMVC技术需要先导入SpringMVC坐标与Servlet坐标

```xml
<dependency>
   <groupId>javax.servlet</groupId>
   <artifactId>javax.servlet-api</artifactId>
   <version>3.1.0</version>
   <scope>provided</scope>
</dependency>

<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>5.2.10.RELEASE</version>
</dependency>

<build>
   <plugins>
      <plugin>
         <groupId>org.apache.tomcat.maven</groupId>
         <artifactId>tomcat7-maven-plugin</artifactId>
         <version>2.2</version>
         <configuration>
            <port>80</port>
            <path>/</path>
         </configuration>
      </plugin>
   </plugins>
</build>
```

![![image_2023-02-28-11-01-23](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104909.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-02-28-11-01-23_20230228114117.png?v=1&type=image&token=V1:4HI6B2csKa50-InxxCciyzZXscbTRiNsq4IulZb8424)

2.创建SpringMVC控制器类(等同于Servlet功能)

```java
@SuppressWarnings("all")
//使用@Controller定义bean
@Controller
public class UserController {
//    设置当前路径的访问操作
    @RequestMapping("/save")
//    设置当前操作的返回值类型
    @ResponseBody
    public String save(){
        System.out.println("user save running...");
        return "{'module':'springMVC'}";
    }
}
```

3.初始化SpringMVC环境(同Spring环境),设定SpringMVC加载对应的bean

```java
@SuppressWarnings("all")
//创建SpringMvc的配置类，加载controller对应的bean
@Configuration
@ComponentScan("com.dkx.springmvc")
public class SpringMvcConfig {
}
```

4.初始化Servlet容器,加载SpringMVC环境,并设置SpringMVC技术处理的请求

```java
//定义一个Servlet容器启动的配置类,在里面加载Spring的配置
@SuppressWarnings("all")
public class ServletContainerInitializerConfig extends AbstractDispatcherServletInitializer {
//    加载SpringMvc容器配置
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext c = new AnnotationConfigWebApplicationContext();
        c.register(SpringMvcConfig.class);
        return c;
    }
//    设置哪些请求归属SpringMvc处理
    @Override
    protected String[] getServletMappings() {
//        设置所有请求归SpringMvc去处理
        return new String[]{"/"};
    }
//    加载SpringMvc容器的配置
    @Override
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }
}
```

## @Controller

- 类型:类注解

- 位置:SpringMvc控制器类定义上方

- 作用:设定SpringMvc的核心控制器bean

## @RequestMapping

- 类型 :方法注解

- 位置:SpringMvc控制器方法定义上方

- 作用:设置当前控制器方法请求访问路径

- 相关属性
    - value(默认):请求访问路径

- SpringMVC入门程序开发总结(1+N)
    - 一次性工作
        - 创建工程,设置服务器,加载工程
        - 导入坐标
        - 创建web容器启动类，加载SpringMVC配置，并设置SpringMVC请求拦截路径
        - SpringMVC核心配置类（设置配置类，扫描controller包，加载Controller控制器bean）
    - 多次工作
        -  定义处理请求的控制器类
        -  定义处理请求的控制器方法，并配置映射路径（@RequestMapping）与返回json数据（@ResponseBody）

- AbstractDispatcherServletInitializer类是SpringMvc提供的快速初始化web3.0容器的抽象类

- AbstractDispatcherServletInitializer提供三个接口方法供用户实现
    - createServletApplicationContext()方法,创建Servlet容器时,加载SpringMVC对应的bean并放入WebApplicationContext对象范围中,而WebApplicationContext的范围为ServletContext范围,即整个web容器范围

```java
//    加载SpringMvc容器配置
    @Override
    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext c = new AnnotationConfigWebApplicationContext();
        c.register(SpringMvcConfig.class);
        return c;
    }
```

- getServletMappings()方法,设定SpringMVC对应的请求映射路径,设置为/表示拦截所有请求,任意请求都将转入到SpringMVC进行处理

```java
//    设置哪些请求归属SpringMvc处理
    @Override
    protected String[] getServletMappings() {
//        设置所有请求归SpringMvc去处理
        return new String[]{"/"};
    }
```

- createRootApplicationContext()方法,如果创建Servlet容器时需要加载非SpringMVC对应的bean,使用当前方法进行,使用方式同createServletApplicationContext()

```java
//    加载SpringMvc容器的配置
    @Override
    protected WebApplicationContext createRootApplicationContext() {
        return null;
}
```

## 入门案例工作流程分析

- 启动服务器初始化过程

1. 服务器启动,执行ServletContainersInitConfig类,初始化web容器

2. 执行createServletApplicationContext方法,创建了WebApplicationContext对象

3. 加载SpringMvcConfig

4. 执行@ComponentScan加载对应的bean

5. 加载UserController,每个@RequestMapping的名称对应一个具体的方法

6. 执行getServletMapping方法,定义所有的请求都通过SpringMvc

- 单次请求过程

1. 发送请求localhost/save

2. web容器发现所有请求都经过SpringMVC,将请求交给SpringMVC处理

3. 解析请求路径/save

4. 由/save匹配执行对应的方法save()

5. 执行save()

6. 检测到有@ResponseBody直接将save()方法的返回值作为响应体返回给请求方

![![image_2023-02-28-14-48-08](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104777.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-02-28-14-48-08_20230228150010.png?v=1&type=image&token=V1:oD4mrSPwUaDZwAcbwGn2GXxkKorT-HhDI2vSyA0S5hs)

## Controller加载控制与业务bean加载控制

- SpringMVC相关bean(表现层bean)

- Spring控制的bean
    - 业务bean(Service)
    - 功能bean(DataSource等)

- SpringMVC相关bean加载控制
    - SpringMVC加载的bean对应的包均在com.dkx.spring.controller包内

- Spring相关bean加载控制
    - 方式一:Spring加载的bean设定扫描范围为com.dkx.spring,排除掉controller包内的bean
    - 方式二:Spring加载的bean设定扫描范围为精准范围,例如Service包,dao包等
    - 方式三:不区分Spring与SpringMVC的环境,加载到同一个环境中

![![image_2023-02-28-14-57-35](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104972.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-02-28-14-57-35_20230228150025.png?v=1&type=image&token=V1:HfBhhMHkzrQ-8QgMljigm3w6tD8kggRnAG3XqMB4To0)

## @ComponentScan

- 名称:@ComponentScan

- 类型:类注解

- 作用：**用于设定扫描Bean路径,多个数据使用数组格式** 

- 范例:

```java
@Configuration
@ComponentScan(value = "com.dkx.spring",
        excludeFilters = @ComponentScan.Filter(
                type = FilterType.ANNOTATION,
                classes = Controller.class
        )
)
public class SpringMvcConfig {
}
```

- 属性
    - excludeFilters:排除扫描路径加载的bean,需要指定类别(type)与具体项(classes)
    - includeFilters:加载指定的bean,需要指定类别(type)与具体项(classes)

## 简化初始化Servlet容器

```java
//定义简化Servlet容器启动的配置类，在里面加载Spring的配置
@SuppressWarnings("all")
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
//加载Spring容器配置
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }
//加载SpringMVC容器配置
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }
//设置哪些请求归属SpringMVC处理
    @Override
    protected String[] getServletMappings() {
       //设置所有请求归属SpringMVC去拦截处理
        return new String[]{"/"};
    }
}
```

## PostMan简介

- PostMan是一款功能强大的网页调试与发送网页HTTP请求的Chrome插件

- 作用:常用于进行接口测试

- 特征
    - 简单
    - 实用
    - 美观
    - 大方

### PostMan基本使用

- 注册登录

- 创建工作空间/进入工作空间

- 发起请求测试结果

## 请求与响应

### 请求的映射路径

**问题** 

- 如果两个Controller不同的类中有同样一个访问路径为 `/save` 那么,将会报错如下:

![![image-20230301103255294](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104266.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image-20230301103255294_20230301150535.png?v=1&type=image&token=V1:M6qs_9a4bb7PuqGlYabymKsv4IQO9Q2Q3d3iWhTUQRs)

导致错误的代码

- UserController

```java
@SuppressWarnings("all")
@Controller
public class UserController {
    @RequestMapping("/save")
    @ResponseBody
    public String save(){
        System.out.println("User save running...");
        return "{'module':'springMvc User Save'}";
    }
}
```

- BookController

```java
@SuppressWarnings("all")
@Controller
public class BookController {
    @RequestMapping("/save")
    @ResponseBody
    public String save(){
        System.out.println("Book save running...");
        return "{'module':'springMVC Book save'}";
    }
    @RequestMapping("/delete")
    @ResponseBody
    public String delete(){
        System.out.println("Book delete running...");
        return "{'module':'springMVC Book delete'}";
    }
}
```

1. 团队多人开发,每人设置不同的请求路径,冲突问题如何解决---<font style="color:red">设置模块名作为请求路径前缀</font> 

- 在UserController类中的save访问路径加上user的一级路径,BookController中也要这样加上统一规范

```java
@SuppressWarnings("all")
@Controller
public class UserController {
    @RequestMapping("/user/save")
    @ResponseBody
    public String save(){
        System.out.println("User save running...");
        return "{'module':'springMvc User Save'}";
    }
}
```

访问路径查看效果

![![image_2023-03-01-10-45-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104618.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-10-45-02_20230301104757.png?v=1&type=image&token=V1:uJXvcIF9AnGIXFW3S4geaxdy89MMGTSc8GflaJMsft0)

### 简化书写

- 类上定义整个模块的路径前缀

- 类中定义访问路径的具体

```java
@SuppressWarnings("all")
@Controller
@RequestMapping("/book")
public class BookController {
    @RequestMapping("/save")
    @ResponseBody
    public String save(){
        System.out.println("Book save running...");
        return "{'module':'springMVC Book save'}";
    }
    @RequestMapping("/delete")
    @ResponseBody
    public String delete(){
        System.out.println("Book delete running...");
        return "{'module':'springMVC Book delete'}";
    }
}
```

访问结果

![![image_2023-03-01-11-01-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104886.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-01-34_20230301150555.png?v=1&type=image&token=V1:jB0L_yef15YHTi5og4N4HpBXFyOb6wSR3OwRebOmpuU)

## 请求方式

### Get请求

- 处理中文乱码问题
    - 在pom.xml中配置Tomcat7的编码具体看如下:

```xml
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
        <port>80</port>
        <path>/</path>
        <uriEncoding>
            UTF-8
        </uriEncoding>
    </configuration>
</plugin>
```

```java
@SuppressWarnings("all")
@Controller
@RequestMapping("/book")
public class BookController {
    @RequestMapping("/save")
    @ResponseBody
    public String save(String name,int age){
        System.out.println("Book save running..."+name+","+age);
        return "{'module':'springMVC Book save'}";
    }
}
```

Send Request

![![image_2023-03-01-11-15-51](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104759.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-15-51_20230301150615.png?v=1&type=image&token=V1:F_VyXVqNDeJgU9E7axr187s12rN-X4K0NS6RrRcftj4)

show console result

![![image_2023-03-01-11-17-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104007.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-17-02_20230301150628.png?v=1&type=image&token=V1:DFtAW_rQFgFqa0IsqReHfQuo3nsiKdX2BZ500bUxq-4)

#### Post请求

- 普通参数：form表单Post请求传参，表单参数名与形参变量名相同，定义形参即可接收参数

Post请求不能在Url中写?来进行参数的赋值

![![image_2023-03-01-11-18-32](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104745.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-18-32_20230301150645.png?v=1&type=image&token=V1:wSNFsoibWZGB1HTtkzEcZjiqGzndvgIl_0fyokBgVRg)

我们需要在响应体中进行赋值

![![image_2023-03-01-11-22-51](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104217.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-22-51_20230301150656.png?v=1&type=image&token=V1:-F2apK9f0drjPUy_2QVN29uOciF-ofSIGq3YzvKu5UI)

![![image_2023-03-01-11-26-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104030.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-26-14_20230301150709.png?v=1&type=image&token=V1:WFlzGxbF9kovBCkYt3HB5I1oec2XGZbPTrFzXlzB6F0)

show console result

![![image_2023-03-01-11-26-51](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104083.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-26-51_20230301150723.png?v=1&type=image&token=V1:TKGTq0suvc8qDFKv7pBYBVi50TwopZPOHb36uXPPGuU)

## 处理请求参数中文乱码问题

- 在config目录下ServletConfig类中定义处理乱码的过滤方法

```java
@SuppressWarnings("all")
public class ServeltConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

//    处理乱码
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter character = new CharacterEncodingFilter();
        character.setEncoding("UTF-8");
//        多个character使用,(逗号)分隔
        return new Filter[]{character};
    }
}
```

![![image_2023-03-01-11-38-16](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104644.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-38-16_20230301150740.png?v=1&type=image&token=V1:TL148AyiBBTqA7icy316397q2umA9OTps_VPFKQDwpU)

send request

![![image_2023-03-01-11-44-17](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104148.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-44-17_20230301150750.png?v=1&type=image&token=V1:JxP3dqC6yZIvE5BaFZV76wo4I1tmt4jVEA7cnsqzj_w)

show console result

![![image_2023-03-01-11-44-39](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104019.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-44-39_20230301150805.png?v=1&type=image&token=V1:OrO83t7BqaxrMO8XxtMqyaK_IpNcxS9VEzuDADDZn-k)

- <font style="color:red">**注意**</font>:此处理乱码方式对POST有效,但对GET无效

### @RequestParam

- 问题:如果发送请求参数的key与我们接收参数的参数名不相同那么则接收不到参数

- 类型:形参注解

- 位置:SpringMvc控制器方法形参定义前面

- 作用:绑定请求参数与处理器方法形参间的关系

- 参数:
    - required:是否为必传参数
    - defaultValue:参数默认值

send request

![![image_2023-03-01-11-58-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104100.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-58-02_20230301150821.png?v=1&type=image&token=V1:H27NRXNfyD6vdxJel_CMq_Xcmjl_BSg5281qxzIITac)

show console result

![![image_2023-03-01-11-57-49](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104828.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-57-49_20230301150831.png?v=1&type=image&token=V1:QAiUWisL8vXBpDSD2FSWmtPUfyjsCikHHY5JsG9yyHY)

- 用于在访问路径的方法的参数列表中对请求参数进行形参之间绑定关系

```java
@SuppressWarnings("all")
@Controller
@RequestMapping("/book")
public class BookController {
    @RequestMapping("/save")
    @ResponseBody
//                    RequestParam绑定请求参数和形参之间的关系
    public String save(@RequestParam("n") String name, int age){
        System.out.println("Book save running..."+name+","+age);
        return "{'module':'springMVC Book save'}";
    }
}
```

send request

![![image_2023-03-01-11-59-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104817.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-59-05_20230301150845.png?v=1&type=image&token=V1:oVlgDoybS8okUommnvOQsbZcoQuXHUcaswo6kTPzqDE)

show console result

![![image_2023-03-01-11-59-33](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104905.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-11-59-33_20230301150858.png?v=1&type=image&token=V1:W_wzg-57RrwZCwaWil5BWiQxeao6aG1zuq3tDYJ7jcA)

## 五种数据类型的传参方式

### 普通参数:

-  url地址传参,地址参数名与形参变量名相同,定义形参即可接收参数

![![image_2023-03-01-14-50-09](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104842.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-14-50-09_20230301150944.png?v=1&type=image&token=V1:tUp4kE9X3Q390V2oQM6mfl3QXE7pfFm_ZJCkqnZnEWc)

- 请求参数名与形参变量名<font style="color:red">**不同**</font>,使用<font style="color:red">**@RequestParam**</font>绑定参数关系

```java
@RequestMapping("/save")
@ResponseBody
//                RequestParam绑定请求参数和形参之间的关系
public String save(@RequestParam("n") String name, int age){
   System.out.println("Book save running..."+name+","+age);
   return "{'module':'springMVC Book save'}";
}
```

### POJO参数/嵌套POJO参数:

前者:请求参数名与形参对象属性名相同,定义POJO类型形参即可接收参数

后者:POJO对象中包含POJO对象

<font style="color:red">**注意:** </font>**编写实体类代码的时候一定加上getter和setter还有toString否则值为null** 

![![image_2023-03-01-14-57-29](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104528.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-14-57-29_20230301151000.png?v=1&type=image&token=V1:XTzNFGj6VAvumdlB39Pl4riYLGUujwAJXmhB9OdgjVk)

```java
//    pojo参数，如果发送请求的参数名和pojo类中的属性名相同则可以接收到参数值
@RequestMapping("/save")
@ResponseBody
public String save(User u){
   System.out.println(u);
   return "{'module':'springMvc User Save'}";
}
```

### 数组参数:

-  请求参数名与形参对象属性名相同且请求参数为多个,定义数组类型形参即可接收参数

![![image_2023-03-01-15-01-24](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061104430.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-15-01-24_20230301151014.png?v=1&type=image&token=V1:XcAYDtTIOhiDO5t9lZkcQv8Y9rmOSZ41jDh87czsFV8)

```java
//    数组参数
@RequestMapping("/arrayParam")
@ResponseBody
public String arrayParam(String[] likes){
   System.out.println(Arrays.toString(likes));
   return "{'module':'springMvc User arrayParam running...'}";
}
```

### 集合保存普通参数:

-  请求参数名与形参集合对象名相同且请求参数为多个,@RequestParam绑定参数关系

![![image_2023-03-01-15-03-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061105625.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-15-03-14_20230301151028.png?v=1&type=image&token=V1:X2_xC1nK4n4iW6DK2y-sXI5DcZzKEpoqJviXMThD9Pg)

```java
//    集合参数
@RequestMapping("/listParam")
@ResponseBody           /*List是引用类型它会创建对象set属性,加上注解来让它直接往里面添加值*/
public String listParam(@RequestParam List<String> likes){
   System.out.println(likes);
   return "{'module':'springMvc User listParam running...'}";
}
```

### 传递json数据

1.  导入坐标

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.9</version>
</dependency>
```

2.  设置发送json数据（请求body中添加json数据）

    ![image-20230417173450225](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061105792.png)

3.  开启自动转换json数据的支持

![![image-20230301170021890](./images/image-20230301170021890.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image-20230301170021890_20230301214324.png?v=1&type=image&token=V1:6bhIWdxVMSWxSQbI-fN2xHbDAqoboL1GOote0oUILio)

<font style="color:red">**注意事项：**</font>**@EnableWebMvc**注解功能强大，该注解整合了多个功能，此处仅使用其中一部分功能，即json数据进行自动类型转换

4. 设置接收json数据

5. 设置接收json数据@RequestBody

```java
    @RequestMapping("/json")
    @ResponseBody
//    RequestBody:用于接收前端给后端传递的json数据
    public String listParamforJson(@RequestBody List<String> likes){
        System.out.println(likes);
        return "{'module':'springMvc User listParamforJson'}";
    }
//-------------------------------Run Result-------------------------------
[唱, 跳, Wrapr, 篮球]
```

#### json数组

- 在SpringMvcConfig配置类中 配置注解@EnableWebMvc

```java
@SuppressWarnings("all")
@Configuration
@ComponentScan("com.dkx.spring")
@EnableWebMvc
public class SpringMvcConfig {
}
```

- Controller类

```java
@SuppressWarnings("all")
@Controller
@RequestMapping("/user")
public class UserController {
    @RequestMapping("/json")
    @ResponseBody
//    RequestBody:用于接收前端给后端传递的json数据
    public String listParamforJson(@RequestBody List<String> likes){
        System.out.println(likes);
        return "{'module':'springMvc User listParamforJson'}";
    }
}
```

send request

![![image_2023-03-01-15-45-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061108054.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-15-45-34_20230301165118.png?v=1&type=image&token=V1:EJPbv1_S3rPUXUUsGYLCRhMZJIPY-oNY0aKvUXguYbE)

show console result

![![image_2023-03-01-16-09-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061108575.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-16-09-14_20230301165133.png?v=1&type=image&token=V1:jTNEFJBIf4haD9ulFJuhmqJo3Of4rVXOVZXMogd0svQ)

#### json对象(POJO)

- User

```java
@SuppressWarnings("all")
public class User {
    private String name;
    private String name1;
    private String name2;
    private String name3;
    private String name4;
    private Address address;

    public User() {
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", name1='" + name1 + '\'' +
                ", name2='" + name2 + '\'' +
                ", name3='" + name3 + '\'' +
                ", name4='" + name4 + '\'' +
                ", address=" + address +
                '}';
    }

    public Address getAddress() {
        return address;
    }

    public void setAddress(Address address) {
        this.address = address;
    }

    public User(String name, String name1, String name2, String name3, String name4, Address address) {
        this.name = name;
        this.name1 = name1;
        this.name2 = name2;
        this.name3 = name3;
        this.name4 = name4;
        this.address = address;
    }

    public User(String name, String name1, String name2, String name3, String name4) {
        this.name = name;
        this.name1 = name1;
        this.name2 = name2;
        this.name3 = name3;
        this.name4 = name4;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName1() {
        return name1;
    }

    public void setName1(String name1) {
        this.name1 = name1;
    }

    public String getName2() {
        return name2;
    }

    public void setName2(String name2) {
        this.name2 = name2;
    }

    public String getName3() {
        return name3;
    }

    public void setName3(String name3) {
        this.name3 = name3;
    }

    public String getName4() {
        return name4;
    }

    public void setName4(String name4) {
        this.name4 = name4;
    }

}
```

- Address

```java
@SuppressWarnings("all")
public class Address {
    private String provind;
    private String city;

    public Address() {
    }

    public Address(String provind, String city) {
        this.provind = provind;
        this.city = city;
    }

    public String getProvind() {
        return provind;
    }

    public void setProvind(String provind) {
        this.provind = provind;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    @Override
    public String toString() {
        return "Address{" +
                "provind='" + provind + '\'' +
                ", city='" + city + '\'' +
                '}';
    }
}
```

- UserController

```java
    @RequestMapping("/pojo")
    @ResponseBody
//    RequestBody:用于接收前端给后端传递的json数据
    public String pojoParam(@RequestBody User u){
        System.out.println(u);
        return "{'module':'springMvc User pojoParam'}";
    }
```

send request

![![image_2023-03-01-16-42-25](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061108671.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-16-42-25_20230301165202.png?v=1&type=image&token=V1:mKrSrm0-gLapYKgMEwSH646T1WJFZm2vsjQ3fHRlZPk)

#### json数组(POJO)

```java
    @RequestMapping("/listPOJO")
    @ResponseBody
//    @RequestBody:用于接收前端给后端传递的json数据
    public String listPOJOforParamJson(@RequestBody List<User> list){
        System.out.println(list);
        return "{'module':'springMvc User listPOJOforParamJson'}";
    }
```

send request

![![image_2023-03-01-16-50-01](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401061109370.png)](SpringMVC%E7%AE%80%E4%BB%8B_md_files/image_2023-03-01-16-50-01_20230301165216.png?v=1&type=image&token=V1:GODlLy0uFoSWsaNCDANiuu1TzfbXUFdIUdcyNcF7LqU)

### @EnableWebMvc

- 类型:配置类注解

- 位置:SpringMvc配置类定义上方

- 作用:**开启SpringMvc多项辅助功能** 


### @RequestBody

- 类型:形参注解

- 位置:SpringM vc控制器方法形参定义前面

- 作用:**将请求中请求体所包含的数据传递给请求参数,此注解一个处理器方法只能使用一次** 

## @RequestBody与RequestParam区别

- 区别
    - @RequestParam用于接收url地址传参,表单传参(application/x-www-form-urlencoded)
    - @requestBody用于接收json数据(application/json)

- 应用
    - 后期开发中,发送json格式数据为主,@RequestBody应用较广
    - 如果发送非json格式数据,选用@RequestParam接收请求参数

## 日期类型参数传递

- 日期类型数据基于系统不同格式也不尽相同
    - 2088-08-18
    - 2088/08/18
    - 08/18/2088

### @DateTimeFormat

- 类型:形参注解

- 位置:SpringMvc控制器方法形参前面

- 作用:设定日期时间型数据格式

- 属性:pattern:日期时间格式字符串

```java
public class User {
    private String name;
   //设定日期时间类型格式，如果格式在json传递参数时不一致则会导致报错
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private Date date;

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public Date getDate() {
        return date;
    }
    public void setDate(Date date) {
        this.date = date;
    }
    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", date=" + date +
                '}';
    }
}

```

controller

```java
@Controller
public class UserController {
    @RequestMapping("/json")
    @ResponseBody
    public String save(@RequestBody User u){
        System.out.println(u);
        return "{'model':'springMVC'}";
    }
}
//--------------------------------Result--------------------------------
User{name='Dkx', date=Wed Dec 19 08:00:00 CST 2001}
```

PostMan,发送json的数据

```json
{
    "name":"Dkx",
    "date":"2001-12-19"
}
```

### 类型转换器

帮我们进行类型转换的接口,将Date转换为String类型但是从第二个Date参数开始不进行自动转换

- Converter接口

```java
@FunctionalInterface
public interface Converter<S, T> {
    @Nullable
    T convert(S var1);
}
```

- 请求参数年龄数据(String --> Integer)

- 日期格式转换(String --> Date)

- @EnablewebMvc功能之一:根据类型匹配对应的类型转换器

## 响应

- 响应页面

```java
//    响应页面/跳转页面
    @RequestMapping("/toJumpPage")
    public String toJumpPage(){
        System.out.println("页面跳转");
        return "/page.jsp";
    }
```

- 响应数据
    - 文本数据

```java
//    响应文本数据
    @RequestMapping("/toText")
//    [WARNING] No mapping for GET /response text:找一个文件为response找不到报错异常
//    加上ResponseBody用于接收json数据 （application/json）
    @ResponseBody
    public String toText(){
        System.out.println("返回纯文本数据");
        return "response text";
    }
```

 - json数据

```java
//    响应POJO对象
    @RequestMapping("/toJsonPOJO")
    @ResponseBody
    public User toJsonPOJO(){
        System.out.println("返回POJO对象");
        User user = new User();
        user.setName("小日子-刘桑");
        user.setAge(20);
        return user;
    }
```

- 响应json数据(对象集合转json数组)

```java
    @RequestMapping("/toJsonList")
    @ResponseBody
    public List<User> toJsonList(){
        System.out.println("返回Json集合数据");
        User user = new User();
        user.setName("刘桑");
        user.setAge(23);
        User user1 = new User();
        user1.setName("张三");
        user1.setAge(22);
        List<User> list = new ArrayList<>();
        list.add(user);
        list.add(user1);
        return list;
    }
```

### @ResponseBody

- 类型:方法注解

- 位置:SpringMvc控制器方法定义上方

- 作用:设**置当前控制器返回值作为响应体** 

帮我们转换的接口:HttpMessageConverter接口

