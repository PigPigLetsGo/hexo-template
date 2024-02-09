---
title: MyBatis-运行报错java-package-org-apache-ibatis-io-does-not-exist
categories:
    - [问题总汇]
tags:
    - mybatis
    - 问题总汇
---

问题描述:

```bash
Error:(3, 28) java: package org.apache.ibatis.io does not exist
Error:(4, 33) java: package org.apache.ibatis.session does not exist
Error:(5, 33) java: package org.apache.ibatis.session does not exist
Error:(6, 33) java: package org.apache.ibatis.session does not exist
```

点开maven窗口,点击m

![image_2023-02-14-16-53-03](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-14-16-53-03.png)

执行命令: `mvn idea:module` 

idea可能没有提示直接运行就行

![image_2023-02-14-16-54-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-14-16-54-02.png)
