---
title: 安装LNMP环境防火墙配置
categories:
    - [计算机学科,nginx]
tags:
    - 计算机学科
    - nginx
---

**网络术语**:

**LNMP** 是指一组通常一起使用来运行动态网站或者服务器的自由软件名称首字母缩写,L指Linux,N指Nginx,M一般指MySQL,也可以指MariaDB,P一般指PHP,也可以指Perl或Python

集成环境网站:`oneinstack.com` 

![image_2023-02-02-11-54-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-02-11-54-14_20230225140420.png)

![image_2023-02-02-11-54-55](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-02-11-54-55_20230225140432.png)

选择好要安装的后复制wget命令

![image_2023-02-02-11-56-36](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-02-11-56-36_20230225140443.png)

在Linux中直接执行命令即可安装

安装完成后的软件位置

![image_2023-02-02-11-57-19](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-02-11-57-19_20230225140455.png)

如果访问不到oneinstack则如下操作:

![image_2023-02-02-12-01-03](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-02-12-01-03_20230225140508.png)

![image_2023-02-02-12-01-20](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-02-12-01-20_20230225140521.png)

![image-20240212202537236](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240212202537236.png)

再次访问ip

![image_2023-02-02-12-02-48](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-02-12-02-48_20230225140544.png)

但是网站连接是不安全的

![image-20240212202524096](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240212202524096.png)

### 修改Nginx默认主页

从给出的路径cd到Nginx的html目录下

![image_2023-02-02-14-50-18](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-02-14-50-18_20230225140608.png)

配置到html
