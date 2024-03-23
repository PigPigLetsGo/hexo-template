---
title: while循环
categories: 
      - [计算机学科,linux,shell编程]
tags:
      - 计算机学科
      - linux
---

> 基本语法格式

while [ 条件判断式 ]

do

程序

done

- 注意:while关键字和 [ 空格 ]之间都有空格

案例:从命令行输入一个数n,统计从1+...+n的值是多少

```bash
#!/bin/bash
SUM=0
i=0
while [ $i -le $1 ]
do
        SUM=$[ $SUM + $i ]
        #i自增
        i=$[ $i+1 ]
done
echo    "总和:$SUM"
```
