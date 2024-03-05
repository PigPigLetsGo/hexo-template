---
title: CentOs安装forune+cowsay
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

先找下看有没有

```bash
yum search fortune
```

安装

```bash
sudo yum -y install fortune-mod
```

安装cowsay

```bash
sudo yum install cowsay
```

现在就可以玩了!

```bash
fortune|cowsay
```

安装有意思的`pokemonsay` 宝可梦

![image-20240208123610659](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240208123610659.png)

安装

拿到连接

![image_2023-01-03-23-09-48](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-03-23-09-48.png)

执行命令:

```bash
git clone https://gitee.com/sunfei2021/pokemonsay.git
cd pokemonsay
./install.sh
```

退回到根目录下试一下命令`pokemonsay` 这个软件被安装到了根目录了

执行命令:

```bash
pokemonsay Hello World
```

```bash
fortune|pokemonsay
```

- 添加`$fortune|pokemonsay 内容自己定义` 到`~/.zshrc` 配置文件的末尾, 使得每次打开终端都可以随机输出一个宝可梦
