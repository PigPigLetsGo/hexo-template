---
title: CentOS 7 Linux安装ohmyzsh
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

最重要的一点:安装`zsh` 

```bash
yum install -y zsh
```

确保在`root` 的用户下, 将zsh设置为默认的shell, 然后再更换普通用户切换

```bash
chsh -s /bin/zsh
```

查看是否切换成功输入指令:

```bash
cat /etc/shells
```

![image_2023-01-03-18-26-19](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-03-18-26-19.png)

如果还没有`git` 的话就执行命令, 安装`git` 有的话就跳过

```bash
yum install -y git
```

安装最关键最有灵魂的插件`oh-my-zsh` !!!

- 官方推荐的安装方式

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

上面的方法尽管一步到位, 但是在国内, 我用上面的方法一直出现clone不下来的情况(不光作者我也是)

- 如果出现一直`clone` 不下来, 就采用下面的方法

- 先把`install.sh` 文件下载下来(这是`gieet` 的国内镜像源)

```bash
wget https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh
```

- 然后给`install.sh` 添加权限

```bash
chmod +x install.sh
```

- 用`vim` 打开`install.sh` 发现 , 有的地方还是`clone github` 的代码, 所以做如下修改

```bash
vim install.sh
```

- 找到以下部分

```bash
# Default settings
ZSH=${ZSH:-~/.oh-my-zsh}
REPO=${REPO:-ohmyzsh/ohmyzsh}
REMOTE=${REMOTE:-https://github.com/${REPO}.git}
BRANCH=${BRANCH:-master}
```

- 把中间两行修改为:

```bash
REPO=${REPO:-mirrors/oh-my-zsh}
REMOTE=${REMOTE:-https://gitee.com/${REPO}.git}
```

- 然后`:wq!` 保存

- 然后执行`install.sh` 

```bash
sh install.sh
```

- 出现如下说明安装成功了

![image_2023-01-03-18-32-29](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-01-03-18-32-29.png)

oh-my-zsh常用插件

zsh-syntax-highighting:提供了语法高亮

zsh-autosuggestions:它会根据历史记录和完成情况建议您键入的命令, 而且快速/不干扰自动提示

zsh-completions:命令自动补全

安装:

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
```

- 你可能会发现, 又双叒叕`clone` 不下来, 那就复制仓库名, 去`gieet` 中搜索, 替换连接即可

```bash
git clone https://gitee.com/mo2/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

git clone https://gitee.com/yantaozhao/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://gitee.com/wangnd/zsh-completions.git ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
```

- 下载好了之后, 修改`~/.zshrc` 配置文件

1. 取消配置文件中的第二行注释`注意这不是profile文件` 

```bash
export PATH=$HOME/bin:/usr/local/bin:$PATH
```

2. 设置插件当然这个操作也是在配置文件中进行的

修改`plugins=(git)` 改成以下命令:

```bash
plugins=(git zsh-completions zsh-autosuggestions zsh-syntax-highlighting)
autoload -U compinit && compinit
```

最后重载下配置文件即可

```bash
source .zshrc
```

安装zsh主题`powerlevel9k` 

```bash
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k
```

- 如果`clone` 不下来可以不屑的多尝试几次毕竟只是网络所造成的

编辑`~/.zshrc` 来启用主题, 在配置文件中找到`ZSH_THEME="xxxx"` 将内容更改为

```bash
ZSH_THEME="powerlevel9k/powerlevel9k"
```

执行命令:来重载一下就生效了

```bash
source .zshrc
```
