---
title: maven-概述
categories:
    - [计算机学科,java,maven,入门]
tags:
    - maven
    - 基础
---

> 什么是Maven

Maven的正确发音,而不是"马文"以及其它什么瘟,Maven在美国是一个口语化的词语,代表专家,内行的意思

一个对Maven比较正式的定义是这么说的:Maven是一个项目管理,它包含了一个项目对象模型(POM:Project Object Model),一组标准集合,一个项目生命周期(Project Lifecycle),一个依赖管理系统(Dependency Management System),和用来运行定义在生命周期阶段(phase)中插件(plugin)目标(goal)的逻辑

下载地址：https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip

> Maven能解决什么问题

可以用更通俗的方式来说明,我们知道,项目开发不仅仅是写写代码而已,期间会伴随着各种必不可少的事情要做,下面列举几个感受一下:

1. 我们需要引用各种jar包,尤其是比较大的工程,引用的jar包往往有几十个乃至上百个,每用到一种jar包,都需要手动引入工程目录,而且经常遇到各种让人抓狂的jar包冲突,版本冲突

2. 我们辛辛苦苦写好了java文件,可是只懂0和1的白痴电脑且完全读不懂,需要将它编译成二进制字节码,好歹现在这项工作可以由各种集成开发工具帮我们完成,Eclipse,IDEA等都可以将代码即时编译,当然,如果你嫌生命漫长,何不铺张,也可以用记事本来敲代码,然后用javac命令一个个地去编译,逗电脑玩

3. 世界上没有不存在bug的代码,计算机喜欢bug就和人们总是喜欢美女帅哥一样,为了追求美为了减少bug.因此写完了代码,我们还要写一些单元测试,然后一个个的运行来检验代码质量

4. 再优雅的代码也是要出来买卖的,我们后面还需要把代码与各种配置文件,资源整合到一起,定型打包,如果是web项目,还需要将之发布到服务器,供人蹂躏

试想,如果现在有一种工具,可以把你从上面的繁琐工作中解放出来,能帮你构建工程,管理,jar包,编译代码,还能帮你自动运行单元测试,打包,生成报表,甚至能帮你部署项目,生成web站点,你会心动吗?Maven就可以解决上面所提到的这些问题.

> Maven的优势举例

前面我们通过web阶段项目,要能够将项目运行起来,就必须将该项目所依赖的一些jar包添加到工程中,否则项目就不能运行了,试想如果具有相同架构的项目有十个,那么我们需要将这一份jar包复制到是个不同的工程中,我们一齐来看一个CRM项目的工程大小

使用传统web项目构建的CRM项目如下:

![image-20240208130517388](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208130517388.png)

原因主要是因为上面的web程序要运行,我们必须将项目运行所需的jar包复制到工程目录中,从而导致了工程很大

同样的项目,如果我们使用Maven工程来构建,会发现总体上工程的大小会少很多,如下图:

![image-20240208130533755](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208130533755.png)

![image_2023-01-10-21-48-43](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-10-21-48-43.png)

> 项目的一键构建

我们的项目,往往都要经历编译,测试,运行,打包,安装,部署,等一系列过程

- 什么是构建?

指的是项目从编译,测试,运行,打包,安装,部署整个过程都交给maven进行管理,这个过程称为构建

- 一键构建

指的是整个构建过程,使用maven一个命令可以轻松完成整个工作

Maven规范化构建流程如下:

![image-20240208130552359](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208130552359.png)

> 脱离IDE环境仍需构建

![image-20240208130600343](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208130600343.png)

> 构建

构建过程包含的主要的环节:

- 清理:删除上一次构建的结果,为下一次构建做好准备

- 编译:java源程序编译成 *.class字节码文件

- 测试:运行提前准备好的测试程序

- 报告:针对刚才测试的结果生成一个全面的信息

- 打包:
    - java工程:jar包
    - web工程:war包
- 安装:把一个Maven工程经过打包操作生成的jar包或war包存入Maven仓库

- 部署:
    - 部署jar包:把一个jar包部署到Nexus私服服务器上
    - 部署war包:借助相关Maven插件(例如:cargo),将war包部署到Tomcat服务器上

> 依赖

如果A工程里面用到了B工程的类,接口,配置文件等等这样的资源,那么我们就可以说A依赖B,例如:

- junit-4.12依赖hamcrest-core-1.3

- thymeleaf-3.0.12.RELEASE依赖ognl-3.1.26
    - ognl-3.1.26依赖javassist-3.20.0-GA
- thymeleaf-3.0.12.RELEASE依赖attoparser-2.0.5.RELEASE

- thymeleaf-3.0.12.RELEASE依赖unbescape-1.1.6.RELEASE

- thymeleaf-3.0.12.RELEASE依赖slf4j-api-1.7.26

依赖管理中要解决的具体问题:

- jar包的下载:使用Maven之后,jar包会从规范的远程仓库下载到本地

- jar包之间的依赖:通过以来的传递性自动完成

- jar包之间的冲突:通过对依赖的配置进行调整,让某些jar包不会导入

<font size=4>maven的工作机制</font>

![image_2023-01-10-22-53-36](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-10-22-53-36.png)

> 安装解压maven核心程序

核心程序压缩包:apche-maven-3.8.4-bin.zip,解压到<font color='blue'>非中文,没有空格</font>的目录,例如:

![image-20240208130617834](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208130617834.png)

在解压目录中,我们需要着重关注Maven的核心配置文件:<font color='red'>conf/settings.xml</font>

> 指定本地仓库

本地仓库默认值:用户家目录/.m2/repository,用于本地仓库的默认位置是在用户的家目录下,而家目录往往是在C盘,也就是系统盘,将来Maven仓库中jar包越来越多,仓库体积越来越大,可能会拖慢C盘运行速度,影响系统性能,所以建议将Maven的本地仓库放在其它盘符下,配置方式如下:

```txt
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->

  <localRepository>E:\maven\mavenRepository</localRepository>
```

本地仓库这个目录,我们手动创建一个空的目录即可

<font color='blue'>记住:</font> 一定要把localRepository标签<font color='blue'>从注释中拿出来</font>

<font color='blue'>注意:</font> 本地仓库本身也需要使用一个<font color='blue'>非中文,没有空格</font>的目录

> 配置阿里云提供的镜像仓库

Maven下载jar包默认访问境外的中央仓库,而国外网站速度很慢,改成阿里云提供的镜像仓库,<font color='blue'>访问国内网站</font>,可以让Maven下载jar包的速度更快,配置的方式是:将下面mirror标签整体复制到settings.xml文件的mirror标签的内部

```
<id>nexus-aliyun</id>
<mirrorOf>central</mirrorOf>
<name>Nexus aliyun</name>
<url>http://maven.aliyun.com/nexus/content/group/public</url>
```

> 配置Maven工程的基础JDK版本

如果按照默认配置运行,java工程使用的默认JDK版本是1.5,而我们熟悉和常用的是JDK1.8版本,修改配置的方式是:将profile标签整个复制到settings.xml文件的profiles标签内

```
  <profile>
	  <id>jdk-11</id>
	  <activation>
		<activeByDefault>true</activeByDefault>
		<jdk>11</jdk>
	  </activation>
	  <properties>
		  <maven.compiler.source>11</maven.compiler.source>
		  <maven.compiler.target>11</maven.compiler.target>
		  <maven.compiler.compilerVersion>11</maven.compiler.compilerVersion>
	  </properties>
  </profile>
```

> 配置环境变量

检查JAVA_HOME配置是否正确

maven是一个用java语言开发的程序,它必须基于JDK来运行,需要通过JAVA_HOME来找到JDK的安装位置

![image_2023-01-11-10-55-56](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-11-10-55-56.png)


> 配置MAVEN_HOME

![image_2023-01-11-11-14-24](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-11-11-14-24.png)

配置环境变量的规律

XXX_HOME 通常指向的是bin目录的上一级

PATH指向的是bin目录

> 配置PATH

![image_2023-01-11-11-14-48](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-11-11-14-48.png)

输入指令查看版本是否配置成功

`mvn -v` 

![image-20240208130633331](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208130633331.png)

