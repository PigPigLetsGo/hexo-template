---
title: web-xml文件springframework-web-servlet-DispatcherServlet-is-not-assignable-to-javax-servlet-Servlet爆红
categories:
    - [问题总汇]
tags:
    - 问题总汇
    - SpringBoot
---

问题描述:

```
org.springframework.web.servlet.DispatcherServlet‘ is not assignable to ‘javax.servlet.Sevlet
翻译:
'org.springframework.web.servlet.DispatcherServlet' 不可分配给 'javax.servlet.Servlet，jakarta.servlet.Servlet'
```

![![image_2023-03-17-21-53-33](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-17-21-53-33_20230323081424.png)](web-xml%E6%96%87%E4%BB%B6springframework-web-servlet-DispatcherServlet-is-not-assignable-to-javax-servlet-Servlet%E7%88%86%E7%BA%A2_md_files/image_2023-03-17-21-53-33_20230323081424.png?v=1&type=image&token=V1:9yXfpQzxGfCxyQlyxYxFgeP_1L2FSC9JPgPXEvCoUhw)

Tomcat-information:

- version:8以上

- DeploymentMethod:local-Tomcat -> EditConfiguration:Idea

可能引发问题的原因:

**这里的意思是DispatcherServlet不能作为一个Servlet,需要查看是否导入了servlet依赖** 

摘自[点击博客链接查看](https://blog.csdn.net/weixin_42756682/article/details/128821110?spm=1001.2101.3001.6650.3&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPosition-3-128821110-blog-109740597.pc_relevant_3mothn_strategy_recovery&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EYuanLiJiHua%7EPosition-3-128821110-blog-109740597.pc_relevant_3mothn_strategy_recovery&utm_relevant_index=4)

- 解决方案:

降低spring-webmvc的版本,将其降为5.3.1版本
