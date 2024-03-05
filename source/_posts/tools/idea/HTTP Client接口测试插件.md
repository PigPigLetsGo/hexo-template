---
title: HTTP Client接口测试插件[idea-2022版本以上自带]
categories:
  - [tools,idea]
tags:
  - java
  - tools
  - idea
  - 项目
---

![image-20231026090939362](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310260909263.png)

使用方式：

在项目的根目录创建一个名为 api结尾的文件夹，里面存放api接口测试的代码：

![image-20231026091037630](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310260910384.png)

在该文件中编写代码如下：

```http
POST http://localhost:63040/content/course/list?pageNo=1&pageSize=2
Content-Type: application/json

{
  "auditStatus": "203001",
  "conurseName": "java",
  "publishStatus": ""
}
```

<font color='red'>注意</font>：如果写多个测试接口uri需要使用###将其分隔否则会出现红色波浪线！比如，如下：

```http
POST http://localhost:63040/content/course/list?pageNo=1&pageSize=2
Content-Type: application/json

{
  "auditStatus": "203001",
  "conurseName": "java",
  "publishStatus": ""
}

###
GET http://localhost:63040/content/course-category/tree-nodes
```

启动对应的服务后进行测试结果如下：

```
POST http://localhost:63040/content/course/list?pageNo=1&pageSize=2

HTTP/1.1 200 
Content-Type: application/json
Transfer-Encoding: chunked
Date: Thu, 26 Oct 2023 01:06:34 GMT
Keep-Alive: timeout=60
Connection: keep-alive

{
  "items": [
    {
      "id": 1,
      "companyId": 1232141425,
      "companyName": "",
      "name": "JAVA8/9/10新特性讲解",
      "users": "java爱好者,有一定java基础",
      "tags": "有个java 版本变化的新内容，帮助大家使用最新的思想和工具",
      "mt": "1",
      "st": "1-3-2",
      "grade": "204002",
      "teachmode": "200002",
      "description": null,
      "pic": "https://cdn.educba.com/academy/wp-content/uploads/2018/08/Spring-BOOT-Interview-questions.jpg",
      "createDate": "2019-09-03T17:48:19",
      "changeDate": "2022-09-17T16:47:29",
      "createPeople": "1",
      "changePeople": null,
      "auditStatus": "202004",
      "status": "203001"
    },
    {
      "id": 27,
      "companyId": 1232141425,
      "companyName": null,
      "name": "Javascript之VueJS",
      "users": "所有人",
      "tags": null,
      "mt": "1-1",
      "st": "1-1-9",
      "grade": "200002",
      "teachmode": "200002",
      "description": "Vue系列课程：从Vue1.0讲到Vue2.0，从理论讲到实战，理论与案例巧妙结合，让课程更容易理解！",
      "pic": "https://cdn.educba.com/academy/wp-content/uploads/2018/08/Spring-BOOT-Interview-questions.jpg",
      "createDate": "2019-09-04T09:56:19",
      "changeDate": null,
      "createPeople": null,
      "changePeople": null,
      "auditStatus": "202004",
      "status": "203001"
    }
  ],
  "counts": 5,
  "page": 1,
  "pageSize": 2
}
Response file saved.
> 2023-10-26T090634.200.json

Response code: 200; Time: 32ms (32 ms); Content length: 987 bytes (987 B)
```

但是如果每次请求发生变化都需要改变的话很繁琐，我们可以对其进行定义变量

创建一个名为：`http-client.env.json`的文件，内容如下：

```json
{
  "dev": {
    "access_token": "",
    "gateway_host": "localhost:63010",
    "content_host": "localhost:63040",
    "system_host": "localhost:63110",
    "media_host": "localhost:63050",
    "search_host": "localhost:63080",
    "auth_host": "localhost:63070",
    "checkcode_host": "localhost:6375",
    "learning_host": "localhost:63020"
  }
}
```

定义完成后怎么使用呢？

使用方式如下：将Post请求中的 localhost固定写法改为变量

```http
POST {{content_host}}}/content/course/list?pageNo=1&pageSize=2
Content-Type: application/json

{
  "auditStatus": "203001",
  "conurseName": "java",
  "publishStatus": ""
}
```

