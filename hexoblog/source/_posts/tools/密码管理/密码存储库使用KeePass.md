---
title: KeePass的使用
categories:
    - [tools,密码管理]
tags:
    - tools
    - 密码管理
---

官网下载地址：https://keepass.info/

## 自动输入

右键数据库点击编辑，然后选择自动输入并输入以下的表达式：

`+{DELAY 100}{CLEARFIELD}{USERNAME}{TAB}{PASSWORD}{ENTER}`

![image-20231121171345488](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311211713827.png)

点击一个存储的账号密码的表然后在里面输入这个号要登录的对应的网址

![image-20231121171446704](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311211714992.png)

然后在点击自动输入选择我们自定义的表达式然后下面点击添加，添加对应的浏览器打开的对应网址

<font color='red'>注意</font>.：添加只能是这个浏览器才生效其它浏览器开启对应网址不生效

![image-20231121171605580](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311211716403.png)

然后点击输入框，按` ctrl + alt + a `就可以实现自动输入并登录了下面进行演示。

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311211717549.gif)

如果要只输入密码快捷键为：`shift + ctrl + alt + a`

演示如下：

![recording](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401201056984.gif)