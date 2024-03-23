---
title: 找回root密码
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

1.  首先,启动系统,进入开机界面,在界面中按"e"进入编辑界面,如图

![image-20240208120620948](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208120620948.png)

2.  进入编辑界面,使用键盘上的上下键把光标往下移动,找到以"Linux16"开头的所在行数,在行的最后面输入:`init=/bin/sh`

![image-20221019165625164](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20221019165625164.png)

3.  接着,输入`Ctrl+x`进入单用户模式

![image-20221019165642791](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20221019165642791.png)

4.  接着,在光标闪烁的位置输入:

```bash
mount -o remount,rw[空格]/
```

注意: 各个单词间有空格,完成后按Enter键

![image-20221019165949956](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20221019165949956.png)

5.  在新的一行最后面输入:`passwd`,完成后按键盘的Enter,输入密码,然后再次输入确认密码,密码修改后,会显示`passwd ...`的样式,说明密码修改成功

![image-20221019170156539](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20221019170156539.png)

输入密码

![image-20221019170232196](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20221019170232196.png)

再次输入密码

![image-20221019170246789](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20221019170246789.png)

修改成功

6.  接着,在鼠标闪烁的位置中(最后一行中)输入:

```bash
touch /.autorelabel
```

注意格式: touch[空格]/ 输入完成后按Enter

7.  继续在光标闪烁的位置中,输入:

```bash
exec /sbin/init
```

注意格式:exec[空格]/ 等待系统自动修改密码.完成后,系统就会自动重启,新密码就生效了

![image-20221019170930729](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20221019170930729.png)

![image-20240208120648476](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208120648476.png)
