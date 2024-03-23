---
title: https证书配置
categories:
    - [计算机学科,nginx]
tags:
    - 计算机学科
    - nginx
---

# https证书配置

### 不安全的http协议

[1](./不安全的http协议.md)

### https原理

#### 非对称加密算法原理

![image_2023-02-03-09-50-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-03-09-50-02_20230225135318.png)

浏览器将公钥+算法得到密文传输给服务器通过443端口下载浏览器传输过来的公钥然后私钥+算法进行解密得到明文,传输给浏览器后,同样浏览器得到私钥然后+算法得到明文

公钥加密后不能使用公钥解密,如果可以则说明不够安全

#### 同样不安全的非对称算法

![image_2023-02-04-11-20-35](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-11-20-35_20230225135333.png)

#### CA机构

![image_2023-02-04-12-16-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-16-34_20230225135346.png)

- https21世纪最伟大的互联网发明

#### 证书

查看证书:

win+R执行命令:`certmgr.msc` 

![image_2023-02-02-10-15-51](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-02-10-15-51_20230225135400.png)


#### 客户端(浏览器)

#### 服务器端

### OpenSSL

openssl包含:SSL协议库,应用程序以及密码算法库

### 自签名

#### OpenSSL

系统内置

#### 图形化工具XCA

下载地址:[1](https://www.hohnstaedt.de/xca/index.php/download)

#### CA签名

### 证书字签名

### 在线证书申请

![image_2023-02-04-12-29-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-29-05_20230225135417.png)

![image_2023-02-04-12-30-13](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-30-13_20230225135428.png)

![image_2023-02-04-12-31-30](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-31-30_20230225135440.png)

![image_2023-02-04-12-32-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-32-05_20230225135453.png)

![image_2023-02-04-12-33-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-33-14_20230225135507.png)

密钥算法:RSA:非对称算法

![image_2023-02-04-12-34-15](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-34-15_20230225135529.png)

![image_2023-02-04-12-37-10](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-37-10_20230225135540.png)

### 将申请证书配置到Nginx上

![image_2023-02-04-12-40-56](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-40-56_20230225135551.png)

![image_2023-02-04-12-41-26](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-12-41-26_20230225135614.png)

解压缩后上传服务器上,传到Nginx得跟目录下

![image_2023-02-04-13-34-59](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-13-34-59_20230225135629.png)

#### 证书安装

将如下配置到Nginx的conf文件中

```
server{
    listen  443 ssl;
    server_name     aa.abc.com;
    
    ssl_certificate     /data/cert/server.crt;
    ssl_certificate_key     /data/cert/server.key;
}
```

将两个解压缩后的文件放到conf目录下

![image_2023-02-04-14-44-58](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-04-14-44-58_20230225135642.png)

### http协议跳转https

`return 301 https://$server_name$request_uri` 
