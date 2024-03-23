---
title: Read读取控制台输入
categories: 
      - [计算机学科,linux,shell编程]
tags:
      - 计算机学科
      - linux
---

> 基本语法

read (选项)(参数)

选项:

-p  指定读取值时的提示符

-t  指定读取值时等待的时间(秒),如果没有在指定的时间内输入,就不再等待了

参数:

变量:指定读取值的变量名

```bash
#!/bin/bash
read -p "请输入数值=" NUM1
echo "你输入的数值为=$NUM1"
read -t 10 -p "请输入数值2=" NUM2
echo "你输入的数值2为=$NUM2"
```
