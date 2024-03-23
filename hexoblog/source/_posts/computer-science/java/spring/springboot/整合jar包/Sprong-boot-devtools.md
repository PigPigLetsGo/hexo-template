---
title: Sprong-boot-devtools
categories:
   - [计算机学科,java,spring,springboot,整合jar包]
tags:
   - 计算机学科
   - springboot
   - 整合jar包
   - devtools
---

## Sprong-boot-devtools

==热部署== 

>  监听到如果有Class文件改动了，就会创建一个新的ClaassLoader进行加载该文件，经过一系列的过程，最终将结果呈现在我们眼前

==spring-boot-devtools== 

>  是一个为开发者服务的一个模块，其中最重要的功能就是**自动将应用代码更改到最新的App上面去，即在我们改变了一些代码或者配置文件的时候，应用可以自动重启**，这在我们开发的时候，非常有用。

==重新启动 vs 重新加载== 

>  Spring Boot提供的重启技术通过使用两个类加载器来工作。
>
>  不改变的类（例如来自第三方jar的类）被加载到base classloader中。
>
>  我们正在开发的类会加载到restart classloader中。当应用程序重新启动时，restart classloader将被丢弃并创建一个新类。这种方法意味着应用程序重启通常比"cold starts"快得多，因为基类加载器已经可用并且已经被填充。

### spring-boot-devtools使用

#### maven依赖

```xml
<!--spring-boot-devtools-->
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-devtools</artifactId>
   <scope>runtime</scope>
   <optional>true</optional>
</dependency>
```

#### yml配置

```xml
# Spring
spring:
  devtools:
    restart:
      # 默认为true
      enabled: true 
```

#### idea配置

#### 代码自动编译

>  File --> Settings --> Compiler --> Build Project automatically

![img](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040852708.png)

#### 运行期间自动编译

<font style="color:red">**注意：这个配置是idea-2021.2版本以及以下版本配置没有的解决看**<a href="#idea-version:2021.2版本以及以下的版本的配置"><u>这个标题中的方式来进行 配置</u></a> </font> 

>  Mac使用快捷键shift+option+command+/，window上的快捷键是Shift+Ctrl+Alt+/，打开Registry，勾选compiler.automake.allow.when.app.running

![img](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040852384.png)

通过以上的设置就可以在不重启服务的情况下加载html，但如果修改java文件，服务在几秒后会自动重启，如果不希望服务重启需要在application.properties或application.yml中添加**spring.devtools.reatart.enable=false** 

### Thymeleaf模板引擎

>  如果使用Thymeleaf模板引擎，需要把模板默认缓存设置为false

```xml
#禁止thymeleaf缓存（建议：开发环境设置为false，生成环境设置为true）
# Spring
spring:
  thymeleaf:
    cache: false
```

### spring-boot-devtools 高级功能

1. **排除资源** 
    某些资源不一定需要在更改时触发重新启动。例如，可以就地编辑Thymeleaf模板。默认情况下，在改变资源/META-INF/maven，/META-INF/resources，/resources，/static，/public，或 /templates不会触发重启但会触发 重新加载。可以使用spring.devtools.restart.exclude属性来自定义排除的资源。例如，要仅排除/static，/public可以设置以下属性：

  ```properties
  spring.devtools.restart.exclude=static/**,public/** #不加载静态资源
  additional-exclude: WEB-INF/** #不加载bian'yi'hou
  ```

2. **监控其他路径** 
如上所述，DevTools监控类路径资源的变动，但如果我们想更改不在类路径中的文件时重新启动或重新加载应用程序，该怎么办呢？这时可以使用spring.devtools.restart.additional-paths属性来配置其他路径以监视更改。

```properties
# Spring
spring:
  devtools:
    restart:
      # 默认为true
      enabled: true
      #排除那个目录的文件不需要restart
      additional-exclude: static/**,public/**
      #添加那个目录的文件需要restart
      additional-paths: src/main/java
```

#### LiveReload

LiveReload在做前端开发的时候，经常会用到。

spring-boot-devtools模块包含嵌入式LiveReload服务器，可以在资源更改时用于触发浏览器刷新。 LiveReload浏览器扩展程序支持Chrome，Firefox和Safari，你可以从livereload.com免费下载。

下面是Chrome的Remote Live Reload插件地址。安装即可拥有这个酷炫的功能。
```
https://chrome.google.com/webstore/detail/remotelivereload/jlppknnillhjgiengoigajegdpieppei?hl=en-GB
```

![image-20230414211255514](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040852342.png)

devtools也会在后台开启一个LiveReload Server，浏览器会与这个Server保持着一个长连接，当后端有`前端资源`变动的时候，将会通知浏览器进行刷新，实现热部署。

### idea-version:2021.2版本以及以下的版本的配置

解释: 在idea-2021.2版本中由于没有 compiler.automake.allow.when.app.runing这个选项,所以我们配置后会发现热部署好像并没有生效,下面就是2021.2以及以下版本的解决方式

操作步骤:

1.修改settings里compiler选项下的配置,将下图中红色方框里的选项全部勾上

![image-20230415130642187](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040852315.png)

2.修改Advanced settings里的配置，将下图中红色方框里的选项勾上

![image-20230415130728664](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040852346.png)

<font style="color:red">注意：如果创建项目以上的配置还得配置一遍</font> 
