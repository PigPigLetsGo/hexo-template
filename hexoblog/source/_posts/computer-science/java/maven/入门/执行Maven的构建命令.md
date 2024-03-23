---
title: 执行Maven的构建命令
categories:
    - [计算机学科,java,maven,入门]
tags:
    - maven
    - 基础
---

## 要求

运行Maven中和构建操作相关的命令时,必须进入到pom.xml所在目录,如果没有在pom.xml所在的目录运行Maven的构建命令,那么会看到下面的信息

```
The goal you specified requires a project to execute but there is no PoM in thisdirectory
```

mvn -v 命令和构建操作无关,只要正确配置了PATH,在任何目录下执行都可以

## 清理操作

`mvn clean` 

效果:删除target目录

## 编译操作

主程序编译: `mvn compile` 

测试程序编译: `mvn test-compile` 

主体程序编译结果存放目录: `target/class` 

测试程序编译结果存放的目录: `target/test-class` 

## 测试操作

`mvn test` 

测试的报告存放的目录: `target/surefilre-reports` 

## 打包操作

`mvn package` 

打包的结果--jar包,存放的目录:target

## 安装操作

`mvn install` 

```
[INFO] Installing E:\maven\maven-work-space\space01\pro01-maven-java\target\pro01-maven-java-1.0-SNAPSHOT.jar to E:\maven\mavenRepository\com\dkx\maven\pro01-maven-java\1.0-SNAPSHOT\pro01-maven-java-1.0-SNAPSHOT.jar
[INFO] Installing E:\maven\maven-work-space\space01\pro01-maven-java\pom.xml to E:\maven\mavenRepository\com\dkx\maven\pro01-maven-java\1.0-SNAPSHOT\pro01-maven-java-1.0-SNAPSHOT.pom
```

安装的效果是将本地构建过程中生成的jar包存入Maven本地仓库,这个jar包在Maven仓库中的路径是根据它的坐标生成的

坐标信息如下:

```xml
<!-- 坐标信息 -->
  <!-- groupId 标签:坐标向量之一;代表公司或组织开发的某一个项目 -->
  <groupId>com.dkx.maven</groupId>
  <!-- artifactId 标签:坐标向量之一;代表项目下的某一模块 -->
  <artifactId>pro01-maven-java</artifactId>
  <!-- version 标签:坐标向量之一;代表当前模块的版本 -->
  <version>1.0-SNAPSHOT</version>
```

在Maven仓库中生成的路径如下:

`E:\maven\mavenRepository\com\dkx\maven\pro01-maven-java\1.0-SNAPSHOT` 

另外,安装操作还会将pom.xml文件转换为XXX.pom文件一起存入本地仓库,所以我们在Maven的本地仓库中想看一个jar包原始的pom.xml文件时,查看对应XXX.pom文件即可,它们是名字发生了改变,本质上是同一个文件
