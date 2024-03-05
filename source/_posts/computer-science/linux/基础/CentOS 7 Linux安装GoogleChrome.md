---
title: CentOS 7 Linux安装GoogleChrome
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

# CentOS 7 Linux安装GoogleChrome

直接操作步骤:

```bash
cd /
cd etc/yum.repos.d
sudo vim google-chrome.repo
```

编辑输入vim

```bash
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
```

安装谷歌

```bash
sudo yum -y install google-chrome-stable --nogpgcheck
```

OK!!!

解决安装谷歌后root用户打不开情况

终端输入:

找到安装google的路径里面再找到google-chrome配置文件修改最后一行

```bash
vim /opt/google/chrome/google-chrome
```

配置文件的最后一行修改为:

```bash
exec -a "$0" "$HERE/chrome" "$@" --no-sandbox
```

保存就OK了!
