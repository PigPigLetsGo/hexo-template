---
title: renren-generate
categories:
   - [计算机学科,java,开源项目框架]
tags:
   - 计算机学科
   - java
   - 开源项目框架
---

我们拉取renren-generate项目到本地

项目仓库地址：https://gitee.com/renrenio/renren-generator

使用git拉取命令：https://gitee.com/renrenio/renren-generator.git

使用idea打开该项目

![image-20240318205508278](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240318205508278.png)

配置  application.yml

其中只配置了自己的数据库的连接地址和数据库是哪个还有账号和密码

```yaml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    #MySQL配置
    driverClassName: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://192.168.56.10:3306/gulimall_pms?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai
    username: root
    password: root
```

修改  generator.properties

这里面修改的是生成的路径的一些规则，主要修改如下的一些信息

```properties
mainPath=com.atguigu
#\u5305\u540D
package=com.atguigu.gulimail
moduleName=product
#\u4F5C\u8005
author=dkx
#Email
email=1851644015@qq.com
#\u8868\u524D\u7F00(\u7C7B\u540D\u4E0D\u4F1A\u5305\u542B\u8868\u524D\u7F00)
tablePrefix=pms_
```

在该项目的resource/template目录中里面是生成代码的一些模版文件，我们可以修改文件内容来达到我们生成时的预期效果，比如我要让他生成时不带shiro自带的权限注解，我可以将其在模版文件里面给它注释掉如下：

![image-20240318210028979](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240318210028979.png)

启动该项目

![image-20240318205526487](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240318205526487.png)

点击端口号进入网页

![image-20240318205555660](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240318205555660.png)

我们可以看到这个它根据配置信息读取到了我们数据库中里的表信息了

我们点击 ==生成代码== 它就会下载生成好的代码为一个.zip压缩文件给我们

![image-20240318210150268](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240318210150268.png)

下面是解压后的文件内容

![image-20240318210209084](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240318210209084.png)