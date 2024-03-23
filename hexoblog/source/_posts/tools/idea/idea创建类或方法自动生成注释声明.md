---
title: idea创建类或方法自动生成注释声明
categories:
  - [tools,idea]
tags:
  - java
  - tools
  - idea
  - 项目
---

创建类时自动生成注释声明:操作步骤:

`File --> settings --> Editor --> File and Code Templates` 

点击`Includes` 选择`File Header` 在右边内容框中输入如下内容:

```java
/**
* @author Dkx
* @${DATE}${TIME}
* @version 1.0
* @function
* @comment
*/
```

![![image_2023-01-14-21-04-28](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309152140275.png)](idea%E5%88%9B%E5%BB%BA%E7%B1%BB%E6%88%96%E6%96%B9%E6%B3%95%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E6%B3%A8%E9%87%8A%E5%A3%B0%E6%98%8E_md_files/image_2023-01-14-21-04-28_20230323080818.png?v=1&type=image&token=V1:OyTyou1tw5AZ_pnL9RTe-fkw0H5HbvIq8k4G9iSL4wQ)

创建方法时自动生成注释声明:操作步骤:

`File --> settings --> Editor --> Live Templates` 

1. 点击+号,再点击`Template Group...` 

创建名称随意,建议使用名称:`Method` 翻译为方法更为规范

![image-20240105144937718](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051449757.png)

选中刚才创建好的Method再次点击+号,选择Live Template

取名为:`/**` 注意:不能随便起名建议使用这个名称

在展开`Method` 选项点击`/**` 标签

![![image_2023-01-14-21-09-33](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309152140923.png)](idea%E5%88%9B%E5%BB%BA%E7%B1%BB%E6%88%96%E6%96%B9%E6%B3%95%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E6%B3%A8%E9%87%8A%E5%A3%B0%E6%98%8E_md_files/image_2023-01-14-21-09-33_20230323080843.png?v=1&type=image&token=V1:GFig0QnwaLoCcCiUdET-hb1squrrMdI7wj9-sC1e6Eg)

在下面的内容框中输入如下内容:

```java
/**
*@param:$params$
*@return:$returns$
*@Date:$date$
*/
```

![![image_2023-01-14-21-10-29](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309152140068.png)](idea%E5%88%9B%E5%BB%BA%E7%B1%BB%E6%88%96%E6%96%B9%E6%B3%95%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E6%B3%A8%E9%87%8A%E5%A3%B0%E6%98%8E_md_files/image_2023-01-14-21-10-29_20230323080857.png?v=1&type=image&token=V1:oOF5UI_XfVYuwA4p0r6yVvsDFSuhwdzMsDlJnf4BZRM)

然后点击`EDIT VARIABLES` 

设置如下设置:

![![image_2023-01-14-21-11-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309152140908.png)](idea%E5%88%9B%E5%BB%BA%E7%B1%BB%E6%88%96%E6%96%B9%E6%B3%95%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E6%B3%A8%E9%87%8A%E5%A3%B0%E6%98%8E_md_files/image_2023-01-14-21-11-14_20230323080909.png?v=1&type=image&token=V1:L_nMc_WJc9fBed2PoQQBavSbR0qlPNK_SiUOE_3b66E)

更改调用方式为按Enter后触发

![![image_2023-01-14-21-11-47](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309152140723.png)](idea%E5%88%9B%E5%BB%BA%E7%B1%BB%E6%88%96%E6%96%B9%E6%B3%95%E8%87%AA%E5%8A%A8%E7%94%9F%E6%88%90%E6%B3%A8%E9%87%8A%E5%A3%B0%E6%98%8E_md_files/image_2023-01-14-21-11-47_20230323080922.png?v=1&type=image&token=V1:qKjGfIUD9JY-i1meZnGKDZa0GGQ7vOGDMNdeY8a6-7U)

点击APPLY,OK即可
