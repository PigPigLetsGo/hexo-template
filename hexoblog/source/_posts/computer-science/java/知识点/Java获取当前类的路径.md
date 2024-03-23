---
title: Java获取当前类的路径
categories:
    - [计算机学科,java,知识点]
tags:
    - 知识点
---

# Java获取当前类的路径

>  简介：
>
>  1.  如何获得当前文件路径常用：
>      1.  Test.class.getResource("")：得到的是当前类Test.class文件的URL目录，不包括自己！
>      2.  Test.class.getResource("/")得到的是当前的classpath的绝对URL路径

## 一、 如何获得当前文件路径

### 1.1 普通工程获取类路径

先看下当前工程的目录：

![image-20240308161752441](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240308161752441.png)

常用：

1.  Test.class.getResouce("")

    作用：得到的是当前类Test.class的文件URL目录。不包括自己！

    代码演示：

    ```java
    @Test
    public void test() {
       URL resource = TestDemo.class.getResource("");
       System.out.println(resource);
    }
    ```

    打印结果：

    ```
    file:/C:/Users/Administrator/Desktop/Demo/nio_channel/out/production/nio_channel/com/dkx/
    ```

2.  Test.class.getResouce("/")

    作用：得到的是当前的classpath的绝对URL路径

    代码演示：

    ```java
    @Test
    public void test1() {
       URL resource = TestDemo.class.getResource("/");
       System.out.println(resource);
    }
    ```

    打印结果：

    ```
    file:/C:/Users/Administrator/Desktop/Demo/nio_channel/out/production/nio_channel/
    ```

3.  Thread.currentThread().getContextLoader().getResource("")

    作用：得到的也是当前classpath的绝对URL路径

    代码演示：

    ```java
    @Test
    public void test2() {
       URL resource = Thread.currentThread().getContextClassLoader().getResource("");
       System.out.println(resource);
    }
    ```

    打印结果：

    ```
    file:/C:/Users/Administrator/Desktop/Demo/nio_channel/out/production/nio_channel/
    ```

4.  Test.class.getClassLoader().getResource("")

    作用：得到的也是当前classpath的绝对URL路径

    代码演示：

    ```java
    @Test
    public void test3() {
       URL resource = TestDemo.class.getClassLoader().getResource("");
       System.out.println(resource);
    }
    ```

    打印结果：

    ```
    file:/C:/Users/Administrator/Desktop/Demo/nio_channel/out/production/nio_channel/
    ```

5.  ClassLoader.getSystemResource("")

    作用：得到的也是当前classpath的绝对URL路径

    代码演示：

    ```java
    @Test
    public void test4() {
       URL systemResource = ClassLoader.getSystemResource("");
       System.out.println(systemResource);
    }
    ```

    打印结果：

    ```
    file:/C:/Users/Administrator/Desktop/Demo/nio_channel/out/production/nio_channel/
    ```

    注意：尽量不要使用System.getProperty("user.dir")当前用户目录的相对路径，后面可以看出得出结果五花八门。

6.  new File("").getAbsolutePath

    作用：得到的是当前工程的绝对路径从主盘开始到当前工程

    代码演示：

    ```java
    @Test
    public void test5() {
       String absolutePath = new File("").getAbsolutePath();
       System.out.println(absolutePath);
    }
    ```

    打印结果：

    ```
    C:\Users\Administrator\Desktop\Demo\nio_channel
    ```

### 1.2 Web服务器获取路径

1.  Tomcat

    类中输出System.getProperty("user.dir")显示的是%Tomcat_Home%/bin

    代码演示：

    ```java
    <%--
      Created by IntelliJ IDEA.
      User: Administrator
      Date: 2024年03月08日 0008
      Time: 16:33:27
      To change this template use File | Settings | File Templates.
    --%>
    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
        <title>Title</title>
    </head>
    <body>
    <% String property = System.getProperty("user.dir"); %>
    <%= property %>
    </body>
    </html>
    ```

    打印结果：

    ![image-20240308170806707](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240308170806707.png)

2.  Resin

    不是你的JSP放的相对路径，是JSP引擎执行这个JSP编译成SERVLET的路径为根，比如用新建文件法测试File f = new File("a.html")这个a.html的resin的安装目录下

3.  如何读文件

    使用ServletContext.getResourceAsStream()就可以

4.  获得文件真实路径

    String file_real_path = ServletContext.getRealPath("mypath/filename")；不建议使用request.getRealPath("/")