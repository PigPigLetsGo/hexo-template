---
title: JSON-SERVER
categories:
    - [tools,前端]
tags:
    - tools
    - web
---

# JSON-SERVER

>  在30秒内获得零编码的完整 RESUTFULL API (认真的)

使用 <3 为需要快速后端进行原型设计和模拟的前端开发人员创建。

目标：基于json-server工具，准备后端接口服务环境

1.  安装全局工具json-server (全局工具仅需要安装一次) 

    `yarn global add json-server 或 npm i json-server -g` 

2.  代码根目录创建一个db目录 ，用于模拟数据库 存放一些数据 然后编写vue代码的时候直接使用接口方便编写功能

3.  将资料index.json移入db目录

4.  进入db目录，执行命令，启动后端接口服务

    `json-server --watch index.json`

5.  访问接口测试：http://localhost:3000/cart 

index.json

```json
{
  "cart": [
    {
      "id": 100001,
      "name": "低帮城市休闲户外鞋天然牛皮COOLMAX纤维",
      "price": 128,
      "count": 1,
      "thumb": "https://yanxuan-item.nosdn.127.net/3a56a913e687dc2279473e325ea770a9.jpg"
    },
    {
      "id": 100002,
      "name": "网易味央黑猪猪肘330g*1袋",
      "price": 39,
      "count": 10,
      "thumb": "https://yanxuan-item.nosdn.127.net/d0a56474a8443cf6abd5afc539aa2476.jpg"
    },
    {
      "id": 100003,
      "name": "KENROLL男女简洁多彩一片式室外拖",
      "price": 128,
      "count": 2,
      "thumb": "https://yanxuan-item.nosdn.127.net/eb1556fcc59e2fd98d9b0bc201dd4409.jpg"
    },
    {
      "id": 100004,
      "name": "云音乐定制IN系列intar民谣木吉他",
      "price": 589,
      "count": 1,
      "thumb": "https://yanxuan-item.nosdn.127.net/4d825431a3587edb63cb165166f8fc76.jpg"
    }
  ],
  "firend": [
    {"id": 1,"name": "张三", "age": 18},
    {"id": 2,"name": "王五","age": 20},
    {"id": 3,"name": "刘桑","age": 19}
  ]
}
```

提供了数据它就能基于数据生成增删改查的接口，而提供这个接口也只需要一个命令就可以了。

在db目录打开powerShell窗口 出现一下的场面就说明启动成了。两个地址可以进行访问

![image-20230827211353269](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308272113461.png)