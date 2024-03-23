---
title: nginx基础配置
categories:
    - [计算机学科,nginx]
tags:
    - 计算机学科
    - nginx
---

### 最小配置文件

**Worker_processes** 

`Worker_processes  1;`默认为1,表示开启一个业务进程

**Worker_connections** 

`Worker_connection  1024;` 单个业务进程可接受连接数

**include  mime.types;** 

`include   mime.types;` 引入http mime类型

**default_type application/octet-stream** 

`default_type application/octet-stream` 如果mime类型没匹配上,默认使用二进制流的方式传输

**sendfile on** 

`sendfile on` 使用Linux 的sendfile(socket,file,len)高效网络传输,也就是数据0拷贝

未开启 sendfile

### 核心配置 需要注意=>:==以下是购买的域名可以这样,使用本地IP或者主机名不能这么玩== 

```
worker_processes  1;	#开启n个进程

events {	#事件驱动模块
    worker_connections  1024;	#每个worker可以创建出多少个连接
}

http {
    include       mime.types;	#引入,将其它的配置文件引入到主的配置文件里
    default_type  application/octet-stream;	#如果mime.types中没有匹配类型则执行默认的方式传输给客户端

    sendfile        on;	#数据0拷贝

    keepalive_timeout  65;

#虚拟主机 vhost
    server {
        listen       8080; #端口号
        server_name  ~^[0-9]+\.mmban\.com$; #当前主机的主机名,域名
        location / {
            root   /www/vod;	#项目的跟目录
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

### 虚拟主机配置

- ~符号ServerName开始，写正则^开始$结尾分号;ServerName结尾

```
#虚拟主机 vhost
    server {
        listen       8080; #端口号
        server_name  ~^[0-9]+\.mmban\.com$; #当前主机的主机名,域名
        location / {
            root   /www/vod;	#项目的跟目录
            index  index.html index.htm;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

### ServerName配置规则

- 本地IP,可以使用关键字反向代理`proxy_pass` 

- servername配置Linux的hostname这个hostname不能在win浏览器中被解析可在Linux本机被解析,因为它对应的是Linux本机的ip而win访问不到除非win在hosts文件中配置域名则可用配置的域名来访问

```
server {                                                                                                                                              
    listen       8080;
    #server_name 192.168.244.128;
    #server_name localhost;
    #在win中浏览器输入Linux的IP也能访问到项目,但不能win中输入bogon,Linux的主机名这样访问不到
    server_name bogon;
    location / {
            #反向代理关键字:proxy_pass
            proxy_pass http://www.qq.com;
            #root 只能查找本机中根目录的项目
        #root   http://www.qq.com;
        index  index.html index.htm;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }
}  
```

![image_2023-01-29-08-49-45](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-29-08-49-45_20230225135924.png)

#### 在同一个ServerName中匹配多个域名

- 小技巧: 访问域名时在后面加上/?xxx就不会带有缓存来请求了

```
server {
   listen       8080; #端口号

   server_name  www.mmban.com www1.mmban.com; #当前主机的主机名,域名

   location / {
       root   /www/vod;	#项目的跟目录
       index  index.html index.htm;
   }

   error_page   500 502 503 504  /50x.html;
   location = /50x.html {
       root   html;
   }
}
```

#### 完整匹配

- 顾名思义就是`www.mmban.com` 

#### 通配符匹配 ,可定义在www开头也可定义在com结尾

```
server {
    listen       8080; #端口号
    server_name  *.mmban.com 或者 www.mmban.*; #当前主机的主机名,域名
    location / {
        root   /www/vod;	#项目的跟目录
        index  index.html index.htm;
    }
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   html;
    }
}
```

#### 正则匹配 

```
server {
   listen       8080; #端口号
   server_name  ~^[0-9]+\.mmban\.com$; #当前主机的主机名,域名
   location / {
       root   /www/vod;	#项目的跟目录
       index  index.html index.htm;
   }
   error_page   500 502 503 504  /50x.html;
   location = /50x.html {
       root   html;
   }
}
```

#### 基于互联网的几种需求解析

![image_2023-01-28-21-52-38](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-21-52-38_20230225135941.png)

#### 配置多个站点

**server_name 只能配置localhost 或者IP地址 这两个不能使用www.或者.com来拼接会访问不到** 

1. 在跟目录下/创建一个www目录  因为location 是从/跟目录开始的

![image_2023-01-28-20-24-20](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-20-24-20_20230225135954.png)

2. 进入www目录中创建两个站点

![image_2023-01-28-20-26-10](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-20-26-10_20230225140006.png)

3. 在进入两个站点分别创建不同的页面以区分

![image_2023-01-28-20-29-47](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-20-29-47_20230225140017.png)

4. 在nginx配置文件中,配置两个站点

**不同端口号** 

```
 server {
     listen       8080;
     server_name  localhost;
     location / {
         root   /www/www;
         index  index.html index.htm;
     }
     error_page   500 502 503 504  /50x.html;
     location = /50x.html {
         root   html;
     }
 }
#1
 server {
     listen       8088;
     server_name  localhost;
     location / {                                                                                                                                      
         root   /www/vod;
         index  index.html index.htm;
     }
     error_page   500 502 503 504  /50x.html;
     location = /50x.html {
         root   html;
     }
 }
```

5. 保存退出后重启nginx服务

- 端口为:8080的

![image_2023-01-28-20-35-52](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-20-35-52_20230225140032.png)

- 端口为:8088的

![image_2023-01-28-20-36-23](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-20-36-23_20230225140052.png)
