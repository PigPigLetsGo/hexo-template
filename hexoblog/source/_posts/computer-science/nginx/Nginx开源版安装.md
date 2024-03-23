---
title: nginx开源版安装
categories:
    - [计算机学科,nginx]
tags:
    - 计算机学科
    - nginx
---

打开官网页面点击`Download` ，会跳转到一个下载的页面

![image_2023-01-28-09-02-32](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-09-02-32_20230225140136.png)

点击下载Linux版的Nginx，之后上传到Linux中

解压Nginx

![image_2023-01-28-09-22-01](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-09-22-01_20230225140146.png)

解压完成后进入到解压后的目录中

![image_2023-01-28-09-23-03](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-09-23-03_20230225140157.png)

目录中有一个`configure` 脚本,用这个脚本来安装,安装过程中需要一些依赖

执行该脚本进行安装可能会报错提示需要的依赖没有找到的错误

需要的依赖：

- gcc：执行命令`sudo yum install -y gcc` 

错误提示:

```
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
```

**缺少pcre** 库

- pcre: 执行命令：`sudo yum install -y pcre pcre-devel` 

**同样的报错可能缺少`zlib库`** 

- zlib:执行命令：`sudo yum install -y zlib zlib-devel`  

报错解决！！执行下一步操作

![image_2023-01-28-09-34-26](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-09-34-26_20230225140212.png)

执行`make` 

![image_2023-01-28-09-36-32](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-09-36-32_20230225140226.png)

执行`sudo make install` 

![image_2023-01-28-09-38-18](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-09-38-18_20230225140240.png)

安装成功

启动Nginx进入sbin目录在这个目录中有一个可执行的文件为`nginx` 

![image_2023-01-28-09-39-54](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-09-39-54_20230225140252.png)

启动Nginx

