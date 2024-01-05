---
title: Java-IDEA错误-Error-java-非法字符-ufeff-Error-java-需要class-interface或enum
categories:
    - [问题总汇]
tags:
    - 报错
    - 编码问题
---

问题描述:

```
Java-IDEA错误-Error: java: 非法字符: ‘\ufeff‘ Error: java: 需要class, interface或enum
```

错误原因:字符编码问题,转换下字符编码即可

解决方案:

将下面状态栏的`File Encoding` 编码将`utf-8` 改为`GBK` 

弹出提示框点击`Convert` 没有提示`RELOAD GBK` 

![image_2023-01-14-23-38-09](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051448448.png)

如果页面上有红线提示`RELOAD GBK` 直接点击`RELOAD GBK` 

再次点击`File EnCoding` 切换回`UTF-8` 同样选择`Convert` 

再次运行项目,正常

![image_2023-01-14-23-39-24](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051448126.png)

解决方案2: 整个项目出现问题

点击`settings --> 搜索 File EnCoding` 查看是不是编码有问题如图:

![image_2023-01-14-23-42-25](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051448893.png)

然后`File --> New Project Settings --> settings For New Project --> 搜索 File Settings` 做同样修改

执行方案1,把出问题的java代码页修改,然后一切正常
