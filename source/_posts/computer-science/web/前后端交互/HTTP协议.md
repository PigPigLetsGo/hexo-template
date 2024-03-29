---
title: 初始Http协议
categories:
   - [计算机学科,web,前后端交互]
tags:
   - 计算机学科
   - web
   - 前后端交互
---

# 初始HTTP

## HTTP是什么

**HTTP全称**：HyperText Transfer Protocol ，**翻译**： ==超文本传输协议==.

**超文本**：==原先一个个单一的文本，通过超链接将其联系起来，由原先的单一的文本变成了可无限延伸，扩展的超级文本，立体文本==。

**传输协议**：==数据传输的规范==.

HTML，JS，CSS，图片，字体，音频，视频等等文件，都是通过HTTP(超文本传输协议)在服务器和浏览器之间传输。

==每一次前后端通信，前端需要主动向后端发送请求，后端接收到前端的请求后，可以给出响应==。

<font style="color:red">HTTP是一个请求-响应协议</font>。

## HTTP请求响应的过程

**流程**：

浏览器查看是否有缓存，通过DNS域名解析服务器得到服务器的具体IP位置，跟服务器进行TCP连接通信请求到服务器，服务器响应回数据到浏览器。

浏览器查看缓存中是否有缓存的记录，根据浏览器的不同缓存的处理方式也不同，有的会直接使用，有的需要再次跟服务器进行一次确认查看缓存是否过期再进行使用

![image-20230528151325021](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040735413.png)

## HTTP报文

### HTTP报文是什么

==浏览器向服务器发送请求时==，==请求本身就是信息==，叫==请求报文==。

==服务器向浏览器发送响应时传输的信息==，叫==响应报文==。

![image-20230528204626626](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040736213.png)

### HTTP报文格式

**请求**：

**请求头**：起始行+首部

请求体

请求方式：GET请求没有请求体^在地址栏上^，POST请求有请求体

**GET** 

![image-20230528210416093](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040738305.png)

![image-20230528210432601](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040738654.png)

**POST** 

![image-20230528210619125](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040738906.png)

![image-20230528210624990](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308040738103.png)

**响应**：

响应头：起始行+首部

响应体

## HTTP方法

### 常用的HTTP方法

浏览器发送请求时采用的方法，和响应无关

GET，POST，PUT，DELETE，...

### HTTP方法的语义

增(POST)删(DELETE)改(PUT)查(GET)，用来定义对于资源采取什么样的操作的，有各自的语义。

这些方法虽然有各自的语义，但是并不是强制性的。

### RESTful接口设计

一种接口设计风格，充分利用HTTP方法的语义。

详情查看[点击查看](../../Spring/SpringMVC/REST风格.md) 

## GET和POST方法的对比

### 1.语义

**GET**：获取数据

**POST**：创建数据

### 2.发送数据

**==GET==通过地址栏在请求头中携带数据** 

能携带的数据量和地址的长度有关系，一般最多就几K

**==POST==既可以通过地址在请求头中携带数据，也可以通过请求体携带数据** 

能携带的数据量理论上是无限的

<u>携带少量数据，可以使用GET请求，大量的数据可以使用POST请求</u>。 

### 缓存

**GET**：可以被缓存

-  GET请求携带的数据在地址中的，而地址是会被浏览器缓存的。
   -  比如，输入一个网址，后面再次访问只需要输入开头几个浏览器就自动补全了这就是被浏览器缓存了。

**POST**：不会被缓存

### 安全性

<font style="color:red">GET 和 POST 都不安全</font>。

发送密码或其它敏感信息时不要使用GET，主要是避免直接被它人窥屏或通过历史记录找到你的密码。

## HTTP状态码

### 1.HTTP状态码是什么

定义服务器对请求的处理结果，是服务器返回的。

### 2.HTTP状态码的语义

==100~199消息==：代表请求已被接收，需要继续处理。

-  websocket

==200~299成功==：请求成功，服务器正常返回数据。

==300~399==：重定向

-  **301**：Moved Permanently 永久性的移动
   -  有请求只要不出意外就有响应，重定向的地址就在响应中`Location: https://www.xxx.com` 
   -  使用301方式是后端不可控的，因为301跳转地址会缓存到用户的本地，需要用户手动清理缓存才能跳转有更新的地址，使用比较谨慎
-  **302**：Move Temporarily 临时性的移动
   -  不会被缓存每一次都会向服务器发送一次请求确认一下往那个地址跳转。
-  **304**：Not Modified 没有修改
   -  向服务器发送请求确认资源是否过期，没有过期则使用过期则重新请求响应数据。

==400~499==：请求错误，错误一般在前端，比如：请求地址不存在，请求数据错误导致异常

-  **400**：Bad Request
   -  请求异常
-  **404**： Not Found
   -  请求找不到资源

==500~599==：服务器错误，错误一般在后端

-  500：Internal Server Error 服务器内部错误，后端代码报错之类的