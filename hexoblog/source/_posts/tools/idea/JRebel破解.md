---
title: JRebel破解以及问题解决
categories:
  - [tools,idea]
tags:
  - java
  - tools
  - idea
---

## 破解

# Jrebel License Server! (兼容 2023.4.0 +)

JRebel 激活地址: http://42.193.18.168:8088/d0cdedde-09ed-44e7-b291-b50d81ab46c0

JRebel 激活邮箱: 295429404@qq.com

## JRebel 无限试用，请将以下内容拷贝到命令提示符中执行:

```bat
curl https://register.jpy.wang/ReRegister/src/main/java/jrebel/JrebelMain.java -o tmp.java && java tmp.java && del tmp.java
```

执行后会弹出一个提示框，点击中间的按钮后过一会儿它就会生成 一个加密字符串并存储到idea缓存中。这样就可以了

## 问题

### 无法启动项目或模块

![img](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/516ef17db5574de0a061c87b047a536b.png)