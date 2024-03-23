---
title: SpringBoot不同的创建方式和启动方式
categories:
   - [计算机学科,java,spring,springboot]
tags:
   - 计算机学科
   - springboot
   - 基础
---

# SpringBoot

- SpringBoot是由Pivotal团队提供的全新框架,其设计目的是用来简化Spring应用的初始搭建以及开发过程

创建过程:

1. 创建工程模块

![image_2023-03-08-16-03-43](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-16-03-43_20230309093026.png)

2. 选择版本号与工程为web

![image_2023-03-08-16-05-11](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-16-05-11_20230309093038.png)

![image_2023-03-08-16-09-24](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-16-09-24_20230309093053.png)

- 创建完成后创建一个web方法来验证以下是否可以使用

![image_2023-03-08-16-09-29](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-16-09-29_20230309093106.png)

执行mvn命令`mvn spring-boot:run` 

- 使用PostMan对http:localhost:8080/books/1发送一个get请求

![image_2023-03-08-16-11-43](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-16-11-43_20230309093119.png)

- Spring程序与SpringBoot程序对比

| 类/配置文件            | Spring   | SpringBoot |
|------------------------|----------|------------|
| pom文件中的坐标        | 手工添加 | 勾选添加   |
| web3.0配置类           | 手工配置 | 无         |
| Spring/SpringMvc配置类 | 手工制作 | 无         |
| 控制器                 | 手工制作 | 手工制作   |

**注意事项:** 

基于idea开发SpringBoot程序需要确保联网且能够加载到程序框架结构

- 访问Spring的官网:[点击](https://spring.io/) 

1. 点击SpringBoot

![image_2023-03-08-18-38-42](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-18-38-42_20230309093139.png)

2. 向下拉到最低点击:Spring lnitializr

![image_2023-03-08-18-39-47](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-18-39-47_20230309093152.png)

3. 创建工程

![image_2023-03-08-18-43-22](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-18-43-22_20230309093203.png)

- 输入Spring web后选择第一个

![image_2023-03-08-18-43-54](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-18-43-54_20230309093218.png)

4. 点击generate

![image_2023-03-08-18-46-22](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-18-46-22_20230309093231.png)

- 点击后就会出现Download的弹窗

![image_2023-03-08-18-48-08](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-18-48-08_20230309093243.png)

- 查看压缩包中的内容

![image_2023-03-08-18-50-43](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-18-50-43_20230309093256.png)

如我们在idea中创建的项目结构是一样的

### 前后端分离问题

- SpringBoot程序快速启动

![image_2023-03-08-19-05-13](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-19-05-13_20230309093312.png)

1. 将工程打包:mvn package

2. 将打包好的jar包给前端人员

![image_2023-03-08-19-06-51](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-19-06-51_20230309093323.png)

3. 让前端人员执行命令:`java -jar maven-springboot-demo05-1.0-SNAPSHOT.jar` 

- 前端人员需要配置有jdk否则命令无效

![image_2023-03-08-19-08-19](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-08-19-08-19_20230309093336.png)

**注意事项:** 

jar支持命令启动需要依赖maven插件支持,请确认打包时是否具有SpringBoot对应的maven插件

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

