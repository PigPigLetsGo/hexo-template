---
title: Thymeleaf
categories:
   - [计算机学科,java,第三方库]
tags:
   - 计算机学科
   - java
   - 第三方库
   - Thymeleaf
---

### 什么是Thymeleaf

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjkzMTcx%2Csize_1%2Ccolor_FFFFFF%2Ct_70.png)

Thymeleaf官网解释：

>  Thymeleaf is a modern server-side Java template engine for both web and standalone environments.
>
>  翻译:
>
>  Thymeleaf是适用于web和独立环境的现代服务器端<font style="color:red">Java模板引擎</font> 

#### 模板引擎介绍:

模板引擎？ 你可能第一次听说模板引擎，估计你会禁不住想问：什么是模板引擎？

-  模板引擎（这里特指用于Web开发的模板引擎）为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的html文档，从字面上理解<font style="color:red">模板引擎</font>,最重要的就是==模板==二字,这个意思就是做好的一个模板后套入对应位置的数据，最终以html的格式展示出来，这就是模板引擎的作用
-  对于模板引擎的理解，可以这样形象的做一个类比：**开会**! 相信你在上学初高中时候每次开会都要提前布置场地，拿小板凳，收拾场地，而你上了大学之后每次开会再也不去大操场了，每次开会都去学校的会议室，桌子板凳音箱主席台 齐全，来个人即可，还可复用。。模板引擎的功能就类似我们的会议室开会一样开箱即用，将模板设计好之后直接填充数据即可而不需要重新设计整个页面，提高页面，代码的复用性

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjkzMTcx%2Csize_1%2Ccolor_FFFFFF%2Ct_70-1681457541034-3.png)

不仅如此，在Java中模板引擎还有很多，模板引擎是动态网页发展进步的产物，在最初并且流传度最广的jsp它就是一个模板引擎。jsp是官方标准的模板，但是由于jsp的缺点比较多也挺严重的，所以很多人弃用jsp选用第三方的模板引擎，市面上开源的第三方的模板引擎也比较多，有Thymeleaf、FreeMaker、Velocity等模板引擎受众较广。

听完了模板引擎的介绍，相信你也很容易明白了模板引擎在web领域的主要作用：让网站实现界面和数据分离，这样大大提高了开发效率，让代码重用更加容易。

### Thymeleaf介绍

上面知晓了模板引擎的概念和功能，你也知道Thymeleaf是众多模板引擎的一种，你一定会好奇想深入学习Thymeleaf的方方面面。从官方的介绍来看，Thymeleaf的目标很明确：

-  Thymeleaf的主要目标是为您的开发工作流程带来优雅自然的模板-HTML可以在浏览器中正确显示，也可以作为静态原型工作，从而可以在开发团队中加强协作。

-  Thymeleaf拥有适用于Spring Framework的模块，与您喜欢的工具的大量集成以及插入您自己的功能的能力，对于现代HTML5 JVM Web开发而言，Thymeleaf是理想的选择——尽管它还有很多工作要做。

并且随着市场使用的验证Thymeleaf也达到的它的目标和大家对他的期望，在实际开发有着广泛的应用。Thymeleaf作为被Springboot官方推荐的模板引擎，一定有很多过人和不寻同之处：

-  动静分离： Thymeleaf选用html作为模板页，这是任何一款其他模板引擎做不到的！Thymeleaf使用html通过一些特定标签语法代表其含义，但并未破坏html结构，即使无网络、不通过后端渲染也能在浏览器成功打开，大大方便界面的测试和修改。
-  开箱即用： Thymeleaf提供标准和Spring标准两种方言，可以直接套用模板实现JSTL、 OGNL表达式效果，避免每天套模板、改JSTL、改标签的困扰。同时开发人员也可以扩展和创建自定义的方言。

### 学习Thymeleaf必知的知识点

Thymeleaf模板的运行离不开web的环境，所以你需要对相关知识学习理解才能更好的有助于你对Thymeleaf的学习和认知。

### SpringBoot

相信你对Springboot都很熟悉，我们使用Thymeleaf大多情况都是基于Springboot平台的，并且Thymeleaf的发展推广也离不开Springboot官方得支持，且本文的实战部分也是基于Springboot平台。

而Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。通过这种方式，Spring Boot致力于在蓬勃发展的快速应用开发领域(rapid application development)成为领导者。

简而言之，Springboot是当前web开发主流，且其简化了Spring的配置让开发者能够更容易上手Web项目的开发。且Thymeleaf能够快速整合入Springboot，使用方便快捷。

### MVC介绍

我们使用的Thymeleaf模板引擎在整个web项目中起到的作用为视图展示(view)，谈到视图就不得不提起模型(model)以及控制器(view),其三者在web项目中分工和职责不同，但又相互有联系。三者组成当今web项目较为流行的MVC架构。

MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，其中：

-  Model（模型）表示应用程序核心（用来存储数据供视图层渲染）。
-  View（视图）显示数据，而本篇使用的就是Thymeleaf作为视图。
-  Controller（控制器）处理输入请求，将模型和视图分离。

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjkzMTcx%2Csize_1%2Ccolor_FFFFFF%2Ct_70-1681457820079-6.png)

使用MVC设计模式程序有很多优点，比如降低程序耦合、增加代码的复用性、降低开发程序和接口的成本，并且通过这样分层结构在部署维护能够提供更大的便捷性。

在Java web体系最流行的MVC框架无疑就是Springmvc框架了，在项目中经常配合模板引擎使用或者提供Restful接口。在下面案例Thymeleaf同样使用Springmvc作为MVC框架进行控制。

### 动静分离

你可能还是不明白什么才是真正的动静分离，其实这个主要是由于Thymeleaf模板基于html，后缀也是.html，所以这样就会产生一些有趣的灵魂。

对于传统jsp或者其他模板来说，没有一个模板引擎的后缀为.html，就拿jsp来说jsp的后缀为.jsp,它的本质就是将一个html文件修改后缀为.jsp，然后在这个文件中增加自己的语法、标签然后执行时候通过后台处理这个文件最终返回一个html页面。

浏览器无法直接识别.jsp文件，需要借助网络(服务端)才能进行访问；而Thymeleaf用html做模板可以直接在浏览器中打开。开发者充分考虑html页面特性，将Thymeleaf的语法通过html的标签属性来定义完成，这些标签属性不会影响html页面的完整性和显示。如果通过后台服务端访问页面服务端会寻找这些标签将服务端对应的数据替换到相应位置实现动态页面！大体区别可以参照下图：
![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjkzMTcx%2Csize_1%2Ccolor_FFFFFF%2Ct_70-1681457937220-13.png)

上图的意思就是如果直接打开这个html那么浏览器会对th等标签忽视而显示原始的内容。如果通过服务端访问那么服务端将先寻找th标签将服务端储存的数据替换到对应位置。具体效果可以参照下图,下图即为一个动静结合的实例。

-  右上角为动态页面通过服务端访问，数据显示为服务端提供的数据，样式依然为html的样式
-  右下角为静态页面可通过浏览器直接打开，数据为初始的数据
   ![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/20200713174930621.png)

动态页面每次修改打开都需要重新启动程序、输入链接，这个过程其实是相对漫长的。如果界面设计人员用这种方式进行页面设计时间成本高并且很麻烦，可通过静态页面设计样式，设计完成通过服务端访问即可达成目标UI的界面和应用，达到动静分离的效果。这个特点和优势是所有模板引擎中Thymeleaf所独有的！

### 使用Thymeleaf

Idea创建步骤查看[点击](./案例/idea创建thymeleaf项目.md)

使用步骤：

导入Thymeleaf的坐标

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
    <version>3.0.4</version>
</dependency>
```

### 编写Controller

在编写Controller和Thymeleaf之前，先看下最终项目的目录结构，有个初略的印象和概念:

**<span alt="wavy" style="color:red">特别注意：图中画红框框的必须创建出来否则就会出错</span>** 

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/watermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwNjkzMTcx%2Csize_1%2Ccolor_FFFFFF%2Ct_70-1681460114809-16.png)

在其中：

-  pom.xml：是项目中的Maven依赖，因为Springboot使用Maven来管理外部jar包依赖，我们只需填写依赖名称配置即可引入该依赖，在本系统中引入Spring web模块(Springmvc)和Thymeleaf的依赖.我们不需要自己去招相关jar包。
-  application.properties: 编写Springboot与各框架整合的一些配置内容。
-  controller：用来编写控制器，主要负责处理请求以及和视图(Thymeleaf)绑定。
-  static：用于存放静态资源，例如html、JavaScript、css以及图片等。
-  templates：用来存放模板引擎Thymeleaf(本质依然是.html文件)

项目基于Springboot框架，且选了Spring web(Springmvc)作为mvc框架，其中Thymeleaf就是v(view)视图层，我们需要在controller中指定Thymeleaf页面的url，然后再Model中绑定数据。

==Controller== 

<font style="color:red">注意事项:千万不要使用@RestController注解来配置类,因为它包含了@ResponseBody会让方法返回的数据为Json就会发生页面展示的是一串字符串index</font> 

```java
@Controller
public class IndexController {
    @GetMapping("/user")//访问controller改方法的路径
    public String getIndex(Model model){//Model给页面追加数据的
        model.addAttribute("name","刘桑");
        return "index";//与templates中index.html对应
    }
}
```

上述代码就是一个完整的controller。部分含义如下：

-  @controller 注解的意思就是声明这个java文件为一个controller控制器。
-  @GetMapping(“index”) 其中@GetMapping的意思是请求的方式为get方式(即可通过浏览器直接请求)，而里面的index表示这个页面(接口)的url地址(路径)。即在浏览器对项目网页访问的地址。
-  getindex() 是@GetMapping(“index”)注解对应的函数，其类型为String类型返回一个字符串，参数Model类型即用来储存数据供我们Thymeleaf页面使用。
-  model.addAttribute(“name”,“bigsai”) 就是Model存入数据的书写方式，Model是一个特殊的类，相当于维护一个Map一样，而Model中的数据通过controller层的关联绑定在view层(即Thymeleaf中)可以直接使用。
-  return “hello”：这个index就是在templates目录下对应模板(本次为Thymeleaf模板)的名称，即应该对应hello.html这个Thymeleaf文件(与页面关联默认规则为：templates目录下==返回字符串.html==)。


### 编写Thymeleaf页面

在templates文件夹中创建一个与上面controller方法中返回名称对应的文件名index.html然后在这个文件中的html标签中引入thymeleaf比如这样:`<html xmlns:th="www.thymeleaf.org"></html>` 这样在thymeleaf中就可以使用thymeleaf的语法规范了

对于一个Thymeleaf程序，只需将index.html文件改成这样即可

```html
<!DOCTYPE html>
<html  xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>title</title>
</head>
<body>
hello 第一个Thymeleaf程序
<div th:text="${name}">name是bigsai(我是离线数据)</div>
</body>
</html>
```

你可能会对<div th:text="${name}">name是bigsai(我是离线数据)</div>感到陌生，这个标签中的th:text="${name}"就是Thymeleaf取值的一个语法，这个值从后台渲染而来(前面controller中在Model中存地值)，如果没网络(直接打开html文件)的时候静态数据为：name是bigsai(我是离线数据)。而如果通过网络访问那么内容将是前面在controller的Model中储存的bigsai。

### 启动程序访问controller路径

如果访问不了查看基本配置<a href="#基本配置">点击查看</a> 

当前这个项目通过浏览器访问路径:`http://localhost/user` 

![image-20230414201355792](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230414201355792.png)

打开浏览器的控制台查看index.html页面中的th:text="${name}"解析的结果

![image-20230414201454989](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230414201454989.png)

直接访问index.html页面的展示效果,动静分离方便开发,完全可以独立开发不需要依赖于我们需不需要给它传数据

![image-20230414204830685](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230414204830685.png)

![image-20230414205041082](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230414205041082.png)

在标签中还可以进行拼接

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title th:text="|dkx-${title}|">title是张三(我是离线数据)</title>
</head>
<body>
第一个Thymeleaf程序
<div th:text="|dkx-${name}|">name是刘桑(我是离线数据)</div>
</body>
</html>
```

```java
@GetMapping("/user")
public String getIndex(Model model){//Model给页面追加数据的
   model.addAttribute("title","maggie");
   model.addAttribute("name","刘桑1");
   return "index";
}
```

访问页面

![image-20230415133754503](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230415133754503.png)

### Thymeleaf语法详解

上面虽然完成了第一个Thymeleaf程序，但是那样远远满足不了我们在项目中使用Thymeleaf，所以我们要对Thymeleaf的语法规则进行更详细的学习

### 配置

虽然SpringBoot官方对Thymeleaf做了很多默认配置，但引入Thymeleaf的jar包依赖后很可能根据自己特定的需求进行更细化的配置，例如页面缓存，字体格式设置等等

SpringBoot官方提供的配置内容如下：	

```properties
# THYMELEAF (ThymeleafAutoConfiguration)
spring.thymeleaf.cache=true # Whether to enable template caching.
spring.thymeleaf.check-template=true # Whether to check that the template exists before rendering it.
spring.thymeleaf.check-template-location=true # Whether to check that the templates location exists.
spring.thymeleaf.enabled=true # Whether to enable Thymeleaf view resolution for Web frameworks.
spring.thymeleaf.enable-spring-el-compiler=false # Enable the SpringEL compiler in SpringEL expressions.
spring.thymeleaf.encoding=UTF-8 # Template files encoding.
spring.thymeleaf.excluded-view-names= # Comma-separated list of view names (patterns allowed) that should be excluded from resolution.
spring.thymeleaf.mode=HTML # Template mode to be applied to templates. See also Thymeleaf's TemplateMode enum.
spring.thymeleaf.prefix=classpath:/templates/ # Prefix that gets prepended to view names when building a URL.
spring.thymeleaf.reactive.chunked-mode-view-names= # Comma-separated list of view names (patterns allowed) that should be the only ones executed in CHUNKED mode when a max chunk size is set.
spring.thymeleaf.reactive.full-mode-view-names= # Comma-separated list of view names (patterns allowed) that should be executed in FULL mode even if a max chunk size is set.
spring.thymeleaf.reactive.max-chunk-size=0 # Maximum size of data buffers used for writing to the response, in bytes.
spring.thymeleaf.reactive.media-types= # Media types supported by the view technology.
spring.thymeleaf.servlet.content-type=text/html # Content-Type value written to HTTP responses.
spring.thymeleaf.suffix=.html # Suffix that gets appended to view names when building a URL.
spring.thymeleaf.template-resolver-order= # Order of the template resolver in the chain.
spring.thymeleaf.view-names= # Comma-separated list of view names (patterns allowed) that can be resolved.
```

上面的配置有些我们可能不常使用，因为Springboot官方做了默认配置大部分能够满足我们的使用需求，但如果你的项目有特殊需求也需要妥善使用这些配置。

比如spring.thymeleaf.cache=false是否允许页面缓存的配置，我们在开发时候要确保页面是最新的所以需要禁用缓存；而在上线运营时可能页面不常改动为了减少服务端压力以及提升客户端响应速度会允许页面缓存的使用。

再比如在开发虽然我们大部分使用UTF-8多一些，我们可以使用spring.thymeleaf.encoding=UTF-8来确定页面的编码，但如果你的项目是GBK编码就需要将它改成GBK。

另外Springboot默认模板引擎文件是放在templates目录下：spring.thymeleaf.prefix=classpath:/templates/,如果你有需求将模板引擎也可修改配置，将templates改为自己需要的目录。同理其他的配置如果需要自定义化也可参照上面配置进行修改。

### 基本配置

```xml
spring:
   thymeleaf:
       cache: false
       encoding: UTF-8
       mode: HTML5
       prefix: classpath:/templates/
       suffix: .html
     mvc:
       static-path-pattern: /static/**
```

cache: 缓存

encoding: 编码

mode: 页面模式

prefix: 资源位置

suffix: 后缀名

mvc:

​	static-path-pattern: 可访问的静态资源

### 常用标签

Thymeleaf通过特殊的标签来寻找属于Thymeleaf的部分，并渲染该部分内容，而除了上面展示过的`th:text`之外还有很多常用标签，并且Thymeleaf也主要通过标签来识别替换对应位置内容，Thymeleaf标签有很多很多，功能也很丰富，这里列举一些比较常用的标签如下：

| 标签      | 作用               | 示例                                                         |
| --------- | ------------------ | ------------------------------------------------------------ |
| th:id     | 替换id             | <input th:id="${user.id}"/>                                  |
| th:text   | 文本替换           | <p text:="${user.name}">bigsai</p>                           |
| th:utext  | 支持html的文本替换 | <p utext:="${htmlcontent}">content</p>                       |
| th:object | 替换对象           | <div th:object="${user}"></div>                              |
| th:value  | 替换值             | <input th:value="${user.name}" >                             |
| th:each   | 迭代               | <tr th:each="student:${user}" >                              |
| th:href   | 替换超链接         | < a th:href="@{index.html}">超链接</a>                       |
| th:src    | 替换资源           | <script type="text/javascript" th:src="@{index.js}"></script> |

### 链接表达式:@{...}

上面我们已经学习到Thymeleaf是一个基于html的模板引擎，但是我们还是需要加入特定标签来声明和使用Thymeleaf的语法。我们需要在Thymeleaf的头部加Thymeleaf标识：

```html
<html xmlns:th="http://www.thymeleaf.org">
```

在Thymeleaf 中，如果想引入链接比如link，href，src，需要使用`@{资源地址}`引入资源。其中资源地址可以static目录下的静态资源，也可以是互联网中的绝对资源。

**引入css** 

```html
<link rel="stylesheet" th:href="@{index.css}">
```

如果使用SpringBoot配合Thymeleaf使用要找如下的路径的话需要这么配置，如下：

![image-20240216134649737](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240216134649737.png)

```html
<link rel="stylesheet" th:href="@{/bootstrap/css/bootstrap.css}"/>
<link rel="stylesheet" th:href="@{/css/index.css}"/>
```

**引入JavaScript：** 

```html
 <script type="text/javascript" th:src="@{index.js}"></script>
```

**超链接：** 

```html
<a th:href="@{index.html}">超链接</a>
```

### 变量表达式: ${…}

在Thymeleaf中可以通过${…}进行取值

创建实体类

```java
@Getter
@Setter
@ToString
public class Test {
    private String username;
    private Integer age;
    private Integer sex;
    private Boolean isVip;
    private Date createTime;
    private List<String> tags;
    private Map<String,String> maps;
}
```

在controller中实例化对象并进行初始化赋值

```java
@GetMapping("/test")
public String getTest(Model model){
   Test t = new Test();
   Map<String,String> map = new HashMap<>();
   map.put("name","张三");
   map.put("age","23");
   map.put("sex","女");
   t.setUsername("刘桑");
   t.setAge(18);
   t.setIsVip(true);
   t.setCreateTime(new Date());
   t.setSex(1);
   t.setTags(Arrays.asList("php","Java","c","c++","c#"));
   t.setMaps(map);
   model.addAttribute("test",t);
   return "usertest";
}
```

### 取普通字符串

如果在controller中的Model直接存储某字符串，我们可以直接`${对象名}`进行取值

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--${test.username等同于test.getUsername()}-->
<!--这样我们需要一个一个的去点对应的变量有点麻烦-->
<h2 th:text="${test.username}"></h2>
<p th:text="${test.age}"></p>
</body>
</html>
```

### 取JavaBean对象:

取JavaBean对象也很容易，因为JavaBean自身有一些其他属性，所以咱们就可以使用`$`<font style="color:red">{对象名.对象属性}</font>或者`$`<font style="color:red">{对象名['对象属性']}</font>来取值，这和JavaScript语法是不是很相似呢！除此之外，如果该JavaBean如果写了get方法，咱们也可以通过get方法取值例如`$`<font style="color:red">{对象.get方法名}</font> 
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <h2 th:text="${test.username}"></h2>
   <p th:text="${test.getUsername}"></p>
   <p th:text="${test['username']}"></p>
</body>
</html>
```

### 取List集合(each):

因为List集合是个有序列表，里面内容可能不止一个，你需要遍历List对其中对象取值，而遍历需要用到标签：th:each,具体使用为<tr th:each="item:${userlist}">,其中item就相当于遍历每一次的对象名，在下面的作用域可以直接使用，而userlist就是你在Model中储存的List的名称
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <ul> <!--each会遍历多个li标签来展示数组内的数据出来,所以不能放在ul中-->
       <li th:each="tag:${test.tags}" th:text="${tag}"></li>
   </ul>
</body>
</html>
```

遍历List集合元素将最后一个变为红色其余变为蓝色

1.在static目录中创建一个app.css文件

```css
.active{
    color:red;
}
.Idle{
    color:blue;
}
```

2.html文件中引入app.css文件

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--引入外部的css样式文件如果在static目录的根则不需要加上目录路径-->
    <link rel="stylesheet" th:href="@{app.css}">
</head>
<body>
   <ul>
       <!--each会遍历多个li标签来展示数组内的数据出来,所以不能放在ul中-->
       <!--stat随意起名相当于迭代的一个索引-->
       <li th:each="tag,stat:${test.tags}"
           th:text="${tag}"
           th:classappend="${stat.last}?active:Idle"
       ><!--通过索引为最后一个使用三目运算符判断是否为红色的active是css样式属性其余为Idle属性--></li>
   </ul>
</body>
</html>
```

访问效果

![image-20230415161111373](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230415161111373.png)

### 直接取Map:

很多时候我们不存JavaBean而是将一些值放入Map中，再将Map存在Model中，我们就需要对Map取值，对于Map取值你可以`${对象名.Map名['key']}`来进行取值。也可以通过`${对象名.Map名.key}`取值，当然你也可以使用`${对象名.Map.get('key')}`(java语法)来取值

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <table bgcolor="#8fbc8f" border="1"} th:object="${test}">
       <tr>
           <td>name:</td>
           <td th:text="*{maps.name}"></td>
       </tr>
       <tr>
           <td>age:</td>
           <td th:text="*{maps['age']}"></td>
       </tr>
       <tr>
           <td>sex:</td>
           <td th:text="*{maps.get('sex')}"></td>
       </tr>
   </table>
</body>
</html>
```

### 遍历Map：

想遍历Map获取它的key和value那也是可以的使用`th:each="item:${对象名.Map名}"`进行遍历，在下面只需使用`item.key`和`item.value`即可获得值

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <table bgcolor="red" border="1">
    <tr th:each="map:${test.maps}">
        <td th:text="${map.key}"></td>
        <td th:text="${map.value}"></td>
    </tr>
	</table>
</body>
</html>
```

### 选择变量表达式: *{…}

变量表达式不仅可以写成${…}，而且还可以写成*{…}。

但是，有一个重要的区别：星号语法对选定对象而不是整个上下文评估表达式。也就是说，只要没有选定的对象，美元(==${…}==)和星号(==*{...}==)的语法就完全一样。

什么是选定对象？使用==th:object==属性的表达式的结果。就可以选定对象
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <div th:object="${test}">
       <h2 th:text="*{username}"></h2>
       <p th:text="*{age}"></p>
       <!--if判断元素是否渲染-->
       <p th:if="*{isVip}">会员</p>
	</div>
   <table bgcolor="#8fbc8f" border="1"} th:object="${test}">
       <tr>
           <td>name:</td>
           <td th:text="*{maps.name}"></td>
       </tr>
       <tr>
           <td>age:</td>
           <td th:text="*{maps['age']}"></td>
       </tr>
       <tr>
           <td>sex:</td>
           <td th:text="*{maps.get('sex')}"></td>
       </tr>
   </table>
</body>
</html>
```

### 消息表达: #{…}

文本外部化是从模板文件中提取模板代码的片段，以便可以将它们保存在单独的文件(通常是.properties文件)中,文本的外部化片段通常称为“消息”。通俗易懂的来说`#{…}`语法就是用来**读取配置文件中数据**的。在Thymeleaf你可以使用`#{...}`语法获取消息

首先在templates目录下建立`home.properties`中写入以下内容：

```properties
dkx.nane=DouKx
dkx.age=22
province=Hei Bei Sheng Sha He Shi
```

在`application.properties`中加入以下内容：

```properties
#配置读取配置文件的位置
spring.messages.basename=templates/home
```

这样我们就可以在Thymeleaf中读取配置的文件了

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
	<table bgcolor="aqua" border="1">
       <tr>
           <td th:text="#{dkx.name}"></td>
       </tr>
       <tr>
           <td th:text="#{dkx.age}"></td>
       </tr>
       <tr>
           <td th:text="#{province}"></td>
       </tr>
   </table>
</body>
</html>
```

## Thymeleaf碎片/组件fragment

fragment 片段的意思，是thymleaf为了消除前端代码的重复性而设计出来的。

**三种引入公共片段的th属性：** 

-  th:insert：将公共片段整个插入到声明引入的元素中

-  th:replace：将声明引入的元素替换为公共片段

-  th:include：将被引入的片段的内容包含进这个标签中

三者区别:

-  th:insert是最简单的：它将简单地插⼊指定宿主标签的标签体中。
-  th:replace实际上⽤指定的⽚段替换其宿主标签。
-  th:include类似于th:insert，⽽不是插⼊⽚段，它只插⼊此⽚段的内容。

在templates目录中创建一个组件component.html文件

-  使用th:fragment定义片段

-  一个页面可以定义多个片段

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <!--fragment将其定义为片段/组件-->
   <div th:fragment="com1">
       <h2>com1</h2>
   </div>
   <!--一个页面可以定义多个片段-->
   <div th:fragment="com2">
       <h2>com2</h2>
   </div>
</body>
</html>

```

将component.html页面中的片段定义到usertest.html页面中

-  使用replace将片段替换到当前div中

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <!--replace将片段替换到这个div中-->
   <div th:replace="~{component::com1}"></div>
</body>
</html>
```

访问usertest.html页面效果

![image-20230415164054230](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230415164054230.png)

-  如果想要保留原来的标签可以使用insert

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <!--replace将片段替换到这个div中-->
   <div th:replace="~{component::com1}"></div>
   <!--如果想要保留原来的标签可以使用insert-->
   <div th:insert="~{component::com1}"></div>
</body>
</html>
```

-  在component.html页面中可以使用id来定义片段

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <!--fragment将其定义为片段/组件-->
   <div th:fragment="com1">
       <h2>com1</h2>
   </div>
   <!--一个页面可以定义多个片段,还可以使用id来定义片段-->
   <div id="com2">
       <h2>com2</h2>
   </div>
</body>
</html>
```

-  usertest.html页面中替换id来命名的片段

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
   <!--replace将片段替换到这个div中-->
   <div th:replace="~{component::com1}"></div>
   <!--replace替换片段使用#来指定替换id为com2的片段-->
   <div th:replace="~{component::#com2}"></div>
</body>
</html>
```

### 组件传值

component.html页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
      <!--括号中的名称是接收数据的变量名-->
   <div th:fragment="com3(message)">
       <!--渲染出变量的数据-->
       <p th:text="${message}"></p>
   </div>

   <div th:fragment="com4(message)">
       <div th:replace="${message}"></div>
   </div>
</body>
</html>
```

usertest.html页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
      <!--替换片段并给片段传递数据,然后渲染出来-->
   <div th:replace="~{component::com3('小日子-刘桑')}"></div>
   <div th:replace="~{component::com4(~{::#message})}">
       <p id="message">替换的模块</p>
   </div>
   <!--格式化获取到对象的时间-->
   <div th:text="${#dates.format(test.createTime,'yyyy-MM-ddEHH:mm:ss')}"></div>
</body>
</html>
```

