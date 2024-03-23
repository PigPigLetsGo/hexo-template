---
title: maven-idea
categories:
    - [计算机学科,java,maven,入门]
tags:
    - maven
    - 基础
---

可以在命令行中自己创建好maven项目然后导入到idea中也可以在idea中自己去创建 

## 执行命令

在idea中执行命令操作:

- 注意:双击命令的时候是从上往下依次执行的如果我们想要跳过某个命令可执行:`跳过测试命令:`mvn clean install -Dmavn.test.skip=true`

- -D表示后面要附加命令的参数,字符D和后面的参数是紧挨着的,中间没有空格

![image_2023-01-13-11-24-46](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-11-24-46_20230330153414.png)

如果想要指定执行的命令比如要想执行:`mvn clean install` 

![image_2023-01-13-11-27-39](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-11-27-39_20230330153451.png)

## 创建web项目

我们直接创建一个java的项目然后改成web项目即可

加入war包打包方式

![image_2023-01-13-11-45-17](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-11-45-17_20230330153506.png)

生成WEB-INF目录和web.xml文件,点击+创建一个web.xml文件

![image_2023-01-13-11-47-09](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-11-47-09_20230330153518.png)

- 输入位置

![image_2023-01-13-11-54-06](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-11-54-06_20230330153532.png)

![image_2023-01-13-11-55-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-11-55-14_20230330153609.png)

![image_2023-01-13-11-55-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-11-55-34_20230330153625.png)

最后就完成了 创建web项目

![image_2023-01-13-11-57-22](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-11-57-22_20230330153638.png)
### 工程导入


Maven工程除了自己创建的,还有很多情况是别人创建的,而为了参与开发或者是参考学习,我们都需要导入到IDEA中,下面我们分几种不同情况说明:

#### 1.来自版本控制系统

目前我们通常使用的都是Git(本地库)+码云(远程库)的版本控制系统,结合IDEA的相关操作方式

#### 2.来自工程目录

直接使用IDEA打开工程目录即可

<font size=4>**工程压缩包** </font>

假设别人发给我们一个Maven工程的zip压缩包,maven-rest-demo.zip,从码云或Github上也可以以zip压缩格式对项目代码打包下载

<font size=4>**解压** </font>

如果你的所有IDEA工程有一个专门的目录来存放,而不是散落各处,那么首先我们就把ZIP包解压到这个指定目录中

![image_2023-01-13-13-51-28](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-13-51-28_20230330153655.png)

<font size=4>**打开** </font>

只要我们确认在解压目录下可以直接看到pom.xml,那么就能证明这个解压别就是我们的工程目录,那么接下来让IDEA打开这个目录就可以了

![image_2023-01-13-16-43-17](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-16-43-17_20230330153732.png)

![image_2023-01-13-16-43-27](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-16-43-27_20230330153745.png)

### 设置Maven核心程序位置

打开一个新的Maven工程,和新创建一个Maven工程是一样的, 此时IDEA的settings配置中关于Maven仍然是默认值:

![image_2023-01-13-16-50-35](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-16-50-35_20230330153810.png)

所以我们还需要像新键maven工程那样,指定一下Maven核心程序位置

![image_2023-01-13-16-51-19](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-16-51-19_20230330153840.png)

### 模块导入

**1 情景重现** 

在实际开发中,通常会忽略模块(也就是module)所在的项目(也就是project)仅仅导入某一个模块本身,这么做很可能是类似这样的情况:比如基于Maven学习SSM的时候,做练习需要导入老师发给我们的代码参考

![image_2023-01-13-16-58-53](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-16-58-53_20230330153856.png)
**2 导入java类型模块** 

找到老师发的工程目录

![image_2023-01-13-17-13-30](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-17-13-30_20230330153911.png)

将这两个模块复制到我们需要的工程下

![image_2023-01-13-17-14-09](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-17-14-09_20230330153924.png)
此时IDEA是识别不了的,且不能运行

![image_2023-01-13-17-14-38](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-17-14-38_20230330153936.png)

我们需要将其以模块形式导入到工程中

- 点击+选择import Module导入模块

![image_2023-01-13-17-16-51](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-17-16-51_20230330153954.png)
- 在目录中选择我们要导入的模块

![image_2023-01-13-17-20-00](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-17-20-00_20230330154015.png)

![image_2023-01-13-17-18-48](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-17-18-48_20230330154026.png)

![image_2023-01-13-17-21-10](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-13-17-21-10_20230330154038.png)

导入成功!
