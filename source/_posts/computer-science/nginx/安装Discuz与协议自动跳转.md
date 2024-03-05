---
title: 安装Discuz与协议自动跳转
categories:
    - [计算机学科,nginx]
tags:
    - 计算机学科
    - nginx
    - 基础
---

访问官网地址:[1](https://discuz.net/)

![image_2023-02-04-14-48-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-14-48-14_20230225140313.png)

上传到Nginx的html目录下

![image_2023-02-04-14-55-22](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-14-55-22_20230225140326.png)

解压缩:`unzip [压缩包名称]` 如果unzip报红,说明没有这个指令,执行命令查看包管理器中有没有该程序

```
yum list unzip
```

选择安装

然后解压

`unzip [压缩包名]` 

![image_2023-02-04-12-13-23](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-13-23_20230225140340.png)

如果配置了安全证书那么,需要将显示的页面配置放到443安全证书配置的配置作用域下否则访问https 443端口不会走默认的80端口就不能访问到我们想要展示的页面了

安装Discuz

如果上传了upload目录访问:

https://网站地址/upload/install

只上传了upload的目录内文件及访问

浏览器访问https://网站地址/install

![image_2023-02-04-21-30-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-21-30-02_20230225140354.png)
