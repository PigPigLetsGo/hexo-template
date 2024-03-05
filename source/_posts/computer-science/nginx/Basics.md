---
title: Basics
categories:
    - [计算机学科,nginx]
tags:
    - 计算机学科
    - nginx
    - 基础
---

###  启动Nginx

进入安装好的目录`/user/local/nginx/sbin` 

```
./nginx 启动
./nginx -s stop 快速停止
./nginx -s quit 优雅关闭，在退出前完成已经接收的连接请求
./nginx -s reload 重新加载配置
```

nginx启动后如果使用kill -9 来进行关闭的话会很费劲所以不建议干这么蠢的事儿

访问：浏览器输入自己Linux的IP

访问效果：

![image-20230128164349106](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230128164349106_20230225095816.png)

如果不能访问的解决方案如下：

- 查看是否开启了防火墙

如果开启了就关闭防火墙

- 查看自己上次是否改了端口号或者端口并不是80的话，80是浏览器默认的端口所以访问时不用输入但是如果改了端口号那么需要在ip地址后加冒号：跟端口号我改的是8080所以要加上8080完整的访问地址为

`192.168.244.128:8080` 

## 安装成系统服务

创建服务脚本

`vim /usr/lib/systemd/system/nginx.service` 

服务脚本内容

- 注意如果如下配置的路径与安装时的路径不匹配则需要更改否则不会生效因为会报错

```bash
[Unit]
Description=nginx - web server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
ExecQuit=/usr/local/nginx/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

重新加载系统服务

```
systemctl daemon-reload
```

![image_2023-01-28-10-39-03](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-10-39-03_20230225095831.png)

可以看到之前启动的Nginx服务还在并没有停止,也是依旧可以访问得到的

停止服务

![mage_2023-01-28-10-41-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-10-41-02_20230225095841.png)

停止nginx服务后,开启系统服务

```
systemctl start nginx.service
```

![image_2023-01-28-10-42-31](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-10-42-31_20230225095856.png)

可以看到服务已经启动了说明我们配置的没有任何问题

![image_2023-01-28-10-43-31](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-28-10-43-31_20230225095916.png)

可以看到系统服务确实没有报错并开启了

将系统服务设置为**开启自启动**

```
systemctl enable nginx.service 
```

