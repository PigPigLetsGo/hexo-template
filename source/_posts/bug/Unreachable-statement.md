---
title: Java出现unreachable statement异常 原因检查
categories:
    - [问题总汇]
tags:
    - 报错
---

# Java出现unreachable statement异常 原因检查

问题描述:

```
unreachable statement异常：
```

![image_2023-01-23-21-21-25](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081447369.png)

我这个问题很显然了就是上面truen所导致的下面查看下会导致这种情况的规则:

## 原因一

> <font style="color:red">java编译器</font>把unreachable statement标记为运行时错误,一个unreachable statement就是编译器决定永远不会执行它

下面的几种情况会出现:`unreachable statement` 

1. 在return语句后写语句

2. 在throw语句后写语句

3. break,continue语句之后定义语句

4. "\u10100" `//合法`,相当于'\u1010'和字符串"0"

5. 移动位运算符可以用于long int char short byte

6. 类的访问控制符可以是public,或什么都不加

7. goto是保留字但不是关键字,then什么都不是

8. 把超类的类型强制转换后赋予子类的对象时,编译无异常,但运行时会出现异常

## 原因二

> 不可达语句的造成是因为:**在此语句前面有一个返回操作,或者其它操作导致不管什么条件都无法执行到这一语句** <br>最重要的是:**检查最前面语句是否有返回,并查看是否因为自己的疏忽,即使没有语法等错误,导致的任何条件都会在此语句前返回** 

因为自己的疏忽,好几次在if或for条件后面加了`;` 导致下面的return语句不会执行之后的任何语句就会返回1;

![image_2023-01-23-21-30-27](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081447316.png)

千万要细心再细心
