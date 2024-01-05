---
title: 解决ideaVim输入法困扰
categories:
    - [问题总汇]
tags:
    - java
    - vim
    - 输入法
---

在idea中使用vim,insert模式用中文输入法,切换到normal模式后仍然是中文,针对这个痛点,idea中有`ideaVimExtension` 插件可以解决

首先要安装`ideaVimExtension` 插件,然后在`.ideavimrc` 文件中配置两个代码如下:

```
set keep-english-in-normal " 开启输入法自动切换功能
set keep-english-in-normal-and-restore-in-insert " 回到insert模式时恢复输入法
```

如果配置后,不生效则进行下一步操作

1. 打开设置选择 时间和语言

![image_2023-01-25-15-58-30](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051446260.png)

2. 选择语言

![image_2023-01-25-15-58-50](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051447704.png)

3. 点击添加语言

![image_2023-01-25-15-59-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051447308.png)

4. 搜索`English` 选择美国的

![image_2023-01-25-16-00-11](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051447199.png)

切换按:win+Leader
