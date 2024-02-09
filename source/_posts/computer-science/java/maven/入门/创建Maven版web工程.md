---
title: 创建Maven版web工程
categories:
    - [计算机学科,java,maven,入门]
tags:
    - maven
    - 基础
---

> 说明

使用mvn archetype:generate命令生成Web工程时,需要使用一个专门的archetype,这个专门生成Web工程骨架的archetype可以参照官网看到它的用法:

![image_2023-01-11-19-36-11](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-11-19-36-11.png)

构建命令:

```
mvn archetype:generate -D archetypeGroupId=org.apache.maven.archetypes -D arArtifactId=maven-archetype-webapp -D archetypeVserion=1.4
```

如果上面指令不能成功的创建可以使用:`mvn archetype:generate` 

我们先不选择用哪个先输入webapp过滤出我们想要的再进行选择

![image_2023-01-11-21-25-06](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-11-21-25-06.png)

生成的pom.xml

确认打包的方式是war包形式

```
<packaging>war</packaging>
```

生成的web工程的目录结构

![image-20240208132043836](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208132043836.png)

webapp目录下有index.jsp

WEB-INF目录下有web.xml

### 创建Servlet

#### 在main目录下创建java目录

![image_2023-01-11-21-30-54](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-11-21-30-54.png)

#### 在java目录下创建Servlet类所在的包的目录

![image-20240208132055163](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208132055163.png)

#### 在包下创建Servlet类

- 在web.xml中注册Servlet

```xml
<servlet>
  <servlet-name>servlet</servlet-name>
  <servlet-class>com.dkx.maven.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>servlet</servlet-name>
  <url-pattern>/helloServlet</url-pattern>
</servlet-mapping>
```

- 在index.jsp页面编写超链接

```html
<html>
<body>
<h2>Hello World!</h2>
<a href="/helloServlet">点击跳转</a>
</body>
</html>
```

JSP全称是:Java Server Page,和Thymeleaf一样,是服务器端页面渲染技术,这里我们不必关心JSP语法细节,编写一个超链接标签即可

- 编译

此时直接运行`mvn compile` 命令出错:

![image_2023-01-11-22-02-10](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-11-22-02-10.png)

上面的错误信息说明:我们的web工程用到了HttpServlet这个类,而HttpServlet这个类属于servlet-api.jar这个jar包,此时我们说,web工程需要依赖servlet-api.jar包

![image_2023-01-11-22-03-51](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-11-22-03-51.png)

### 配置对servlet-api.jar包的依赖

对于不知道详细信息的依赖可以到[点击](https://mvnrepository.com/) 网页查询,使用关键字搜索,然后在搜索结果列表中选择适合的使用

![image-20240208132112744](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208132112744.png)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.dkx.maven-web</groupId>
  <artifactId>pro02-maven-web</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>pro02-maven-web Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>
  <build>
    <finalName>pro02-maven-web</finalName>
  </build>
</project>
```

将信息加入到pom.xml中并重新执行`mvn clean compile` 命令

### 将web工程打包为war包

运行`mvn package` 命令,生成war包的位置如下如所示:

![image_2023-01-11-22-16-35](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-11-22-16-35.png)

### 将war包部署到Tomcat上运行

将war包复制到Tomcat/webapps目录下

![image_2023-01-12-12-46-57](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-12-12-46-57.png)



