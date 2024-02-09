---
title: for循环语句
categories: 
      - [计算机学科,linux,shell编程]
tags:
      - 计算机学科
      - linux
      - shell编程
---

> 基本语法格式

for 变量 in 值1 值2 值3 ...

do

程序代码

done

> 基本语法格式2

for ((初始值;循环;控制条件;变量变化))

do

程序

done

```bash
#!/bin/bash
#打印1-100的值
for (( i=1; i<=100; i++ ))
do
        echo "$i"
done
echo "==================================================="
#打印1-100的值
SUM=0
for(( i=1; i<=$1; i++ ))                                                                                                                           
do
        #如果这里使用echo输出那么下面的输出就是0
        SUM=$[ $SUM+$i ]
done
echo "总和:$SUM"
```
