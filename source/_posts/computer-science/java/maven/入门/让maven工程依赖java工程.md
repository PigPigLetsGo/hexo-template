---
title: 让Maven工程依赖java工程
categories:
    - [计算机学科,java,maven,入门]
tags:
    - maven
    - 基础
---

## 观念

明确一个意识:从来只有web工程依赖java工程,没有反过来java工程依赖web工程,本质上来说,web工程依赖的java工程其实就是web工程里导入的jar包,最终java工程会变成jar包,放在web工程的WEB-INF/lib目录下

### 操作

在pro02-maven-web工程的pom.xml中,找到dependencies标签,在dependencies标签中做如下配置:

```xml
<!-- 配置对java工程pro01-maven-java的依赖 -->
<!-- 具体的配置方式:在dependency标签内使用坐标实现依赖 -->
<dependency>
    <groupId>com.dkx.maven</groupId>
    <artifactId>pro01-maven-java</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

### 补充创建目录

pro02-maven-web\src\test\java\com\dkx\maven

### 确认web工程依赖了junit

### 创建测试类

把java工程的CalculatorTest.java复制到pro02-maven-web\src\test\java\com\dkx\maven目录下

### 执行maven命令

> 测试命令

`mvn test` 

说明:测试操作中会提前自动执行编译操作,测试成功寿命编译也是成功的

打包命令

`mvn package` 

![image-20240208131736660](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208131736660.png)

通过查看war包内的结构,我们看到被web工程依赖的java工程确实是会变成web工程的WEB-INF/lib目录下的jar包

![image-20240208131802221](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208131802221.png)

查看当前web工程所依赖的jar包列表

`mvn dependency:list` 

![image_2023-01-12-13-54-50](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-12-13-54-50.png)

这样我们的格式虽然和我们XML配置文件中坐标的格式不同,但是本质上还是坐标信息,大家需要能够认识这样的格式,将来从Maven命令的日志或错误信息中看到这样格式的信息,就能够识别出来这是坐标,进而根据坐标到Maven仓库找到对应的jar包,用这样的方式解决我们遇到的报错的情况

### 以树形结构查看当前web工程的依赖信息

`mvn dependency:tree` 

![image-20240208131819600](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208131819600.png)

我们在pom.xml中并没有依赖hamcrest-core,但是它却被加入了我们依赖的列表,原因是:junit依赖了hamcrest-core,然后基于依赖的传递性,hamcrest-core被传递到我们的工程了

