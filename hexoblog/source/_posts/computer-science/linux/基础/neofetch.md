---
title: CentOs安装neofetch
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

安装`dnf命令` 

```bash
sudo yum install dnf
```

安装`dnf-plugins-core` 

```bash
sudo yum install dnf-plugins-core
```

启用COPR仓库然后安装neofetch

```bash
sudo dnf copr enable konimex/neofetch
sudo dnf install neofetch
```

执行命令`neofetch` 命令后查看

![image_2023-01-03-17-45-11](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-03-17-45-11.png)

指定显示系统的Logo

```shell
neofetch --ascii_distro [系统比如:Mac]
```

![image-20230603110201318](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230603110201318.png)
