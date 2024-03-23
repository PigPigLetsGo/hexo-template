---
title: PowerShell配置ohmyzsh
categories:
   - [tools,windows]
tags: 
   - tools
   - windows
---

第一步

windows应用商店安装 WindowsTerminal

![image-20230826193816724](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308261938192.png)

确保自己电脑有PowerShell,或者去下载一个也是在应用商店

![image-20230826193848906](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308261938122.png)

安装字体否则一些显示有问题：字体上传到了云端可以去拉取获取下载链接：https://ohmyposh.dev/docs/installation/fonts点击 [Meslo LGM NF](https://github.com/ryanoasis/nerd-fonts/releases/download/v3.0.2/Meslo.zip)即可下载字体包

```sh
# 安装oh-my-posh
winget install JanDeDobbeleer.OhMyPosh -s winget

# 使用的是哪个 shell
oh-my-posh get shell

# 直接加载：
& ([ScriptBlock]::Create((oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\night-owl.omp.json" --print) -join "`n"))

# 编辑PowerShell 配置文件脚本，每次启动之后自动加载
notepad $PROFILE

# 当上述命令出错时，请确保先创建配置文件
New-Item -Path $PROFILE -Type File -Force

# 在配置文件里添加以下行：
& ([ScriptBlock]::Create((oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\night-owl.omp.json" --print) -join "`n"))
或者添加这行：
oh-my-posh init pwsh --config '$env:POSH_THEMES_PATH\night-owl.omp.json' | Invoke-Expression

# 重新加载配置文件以使更改生效
. $PROFILE

# 查看所有themes:
Get-PoshThemes
#运行上面命令后，最后3行显示如下：
# ---theme存放的位置：
Themes location: C:\Users\admin\AppData\Local\Programs\oh-my-posh\themes
# --- 如果输入$profile, 得出的路径跟以下是一致的：
To change your theme, adjust the init script in C:\Users\admin\Documents\PowerShell\Microsoft.PowerShell_profile.ps1.
# --- 之前$profile配置文件，也可以改成以下这句（之前这句，向防病毒软件添加例外）里面路径写自己对应的
oh-my-posh init pwsh --config 'C:\Users\Administrator\AppData\Local\Programs\oh-my-posh\themes\night-owl.omp.json' | Invoke-Expression

# 安装文件图标库
Install-Module -Name Terminal-Icons -Repository PSGallery

#使用图标，可以把以下这条命令加到$PROFILE里（保存，.$profile使生效），单独运行就是一次性：
Import-Module -Name Terminal-Icons


#BONUS：
# 设置随机主题：
# 在powershell输入code $profile，输入下面的脚本命令：
oh-my-posh init pwsh --config 'C:\Users\Administrator\AppData\Local\Programs\oh-my-posh\themes\night-owl.omp.json' | Invoke-Expression
```

如果命令操作中出现创建文件的错误问题而无法解决则自己去对应的路径下创建一个如下：然后将下面配置信息写入

```sh
oh-my-posh init pwsh --config 'C:\Users\Administrator\AppData\Local\Programs\oh-my-posh\themes\night-owl.omp.json' | Invoke-Expression
```

-  **注意**：里面的路径按照自己执行命令：Get-PoshThemes 的时候最后一行看到的填写打开路径的目录可以按照自己喜欢的样式改。

![image-20230826193942959](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308261939548.png)

最后就是Windows Terminal的配置了

![image-20230826194133851](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308261942396.png)![image-20230826194150186](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308261942154.png)

![image-20230826194225862](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308261942041.png)

![image-20230827165110165](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308271651695.png)

效果

![image-20230828092941987](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308280929677.png)

也可以配合着neofetch使用 

```sh
oh-my-posh init pwsh --config 'C:\Users\Administrator\AppData\Local\Programs\oh-my-posh\themes\night-owl.omp.json' | Invoke-Expression

neofetch --ascii_distro Mac
```

