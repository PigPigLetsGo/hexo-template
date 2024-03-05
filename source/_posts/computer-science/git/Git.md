---
title: Git版本控制系统
categories:
   - [计算机学科,git]
tags:
   - git
   - 基础
---

## Git版本控制系统

##### git初始

>  <span alt='solid'>概念</span>：
>
>  一个免费开源，分布式的<font title='red'>代码版本</font>控制系统， 帮助开发团队维护代码
>
>  <span alt='solid'>作用</span>：
>
>  <font title='red'>记录</font>代码内容，<font title='red'>切换</font>代码版本，多人开发时高效<font title='red'>合并</font>代码内容

<span alt='solid'>如何学</span>：

**个人本机使用**：git基础命令和概念

**多人共享使用**：团队开发同一个项目的代码版本管理

![image-20230816213516967](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308162135287.png)

### Git配置用户信息

**配置**：用户名和邮箱，应用在每次提交代码版本时==表明自己身份==.

**命令**：

```sh
git config --global user.name '用户名'
```



```sh
git config --global user.email '邮箱'
```

>  总结：
>
>  1.  为何学习 Git ?
>      -  <font title='red'>管理代码版本</font>，记录，切换，合并代码
>  2.  Git 学习
>      -  现在本机自己使用
>      -  再学习多人共享使用

### 掌握Git仓库

Git 仓库 (repository) ：记录文件<font title='red'>状态</font>内容的地方，存储着修改的<font title='red'>历史记录</font>。

**创建**：

1.  把本地文件夹<font title='red'>转换</font>成 Git 仓库：命令：<font title='red'>git init</font>.

    ![image-20230817114756556](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171148966.png)

2.  从其它服务器上<font title='red'>克隆 </font>Git 仓库

需求：创建一个空白的 Git 仓库

打开GitBash到项目目录下，执行命令：git init

![image-20230817123047004](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171230894.png)

查看项目目录下的变化

![image-20230817123104694](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171231139.png)

>  <span alt='solid'>总结</span>：
>
>  1.  什么是Git仓库？
>      -  记录文件状态内容和<font title='red'>历史记录</font>的地方 (.git文件夹)
>  2.  如何创建Git仓库？
>      -  把本地文件夹转换成Git仓库：命令 <font title='red'>git init</font>
>      -  从其它服务器上克隆Git仓库

### Git的三个区域

**Git使用时**：

<span alt='solid'>工作区</span>：实际<font title='red'>开发</font>时操作的文件夹

<span alt='solid'>暂存区</span>：保存之前的<font title='red'>准备区域</font> (暂存改动过的文件)

<span alt='solid'>版本库</span>：提交并<font title='red'>保存</font>暂存区中的内容，产生一个版本快照

| 命令                          | 作用                         |
| ----------------------------- | ---------------------------- |
| git add 文件名                | 暂存指定文件                 |
| git add .                     | 暂存所有改动的文件           |
| git commit -m ‘注释说明’      | 提交并保存，产生版本快照     |
| git ls-files                  | 查看当前暂存区记录了哪些文件 |
| `git log --oneline`           | 查看版本库历史日志           |
| `git reflog --online`         | 查看完整的版本库历史日志     |
| `git rm --cached 路径/文件名` | 从暂存区中移除指定文件       |

<blockquote alt='danger'>
	<div>
      <p>
         <span><font title=red>注意</font>：图中的 git_study 也就是自己的当前项目路面</span>
      </p>
   </div>
</blockquote>

![image-20230817134010660](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171340651.png)

**需求**：把登录页面新增后，暂存并提交

![image-20230817135326555](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171353405.png)

>  总结：
>
>  1.  Git 使用时有哪些区域？
>      -  工作区，暂存区，版本库
>  2.  工作区的内容，最终要如何保存在版本库中？
>      -  <font title='red'>git add</font> 添加到暂存区
>      -  等待时机后 <font title='red'>git commit</font> 提交保存到版本库，产生一次版本快照记录

### Git 文件状态

Git 文件2种状态：

-  未跟踪：新文件，从未被 Git 管理过
-  已跟踪：Git 已经知道和管理的文件

**使用**：修改文件，暂存，提交保存记录，如此反复

**需求**：新增 css 文件，并使用 <font title='red'>git status -s</font> 查看文件状态，并最终提交

| 文件状态     | 概念              | 场景                 |
| ------------ | ----------------- | -------------------- |
| 未跟踪 (U)   | 从未被Git管理过   | 新文件               |
| 新添加 (A)   | 第一次被 Git 暂存 | 之前版本记录无此文件 |
| 未修改 (‘ ’) | 三个区域统一      | 提交保存后           |
| 已修改 (M)   | 工作内容变化      | 修改了内容产生       |

其它的状态：

| 状态 | 描述                            |
| ---- | ------------------------------- |
| D    | 你本地删除的文件 (服务器上还在) |
| R    | 文件名被修改                    |
| T    | 文件的类型被修改                |
| X    | 未知状态                        |



![image-20230817140620456](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171406687.png)

第一列是暂存区状态

第二列是工作区状态

![image-20230817140706529](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171407660.png)

**演示**：

#### 1 新增一个css文件

![image-20230817141012331](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171410453.png)

#### 2 执行：git add . 

将所有文件添加到暂存区，然后git status -s查看状态

![image-20230817141140352](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171411783.png)

css 文件处于 新添加状态 (A)

#### 3 改动css内容

如果改动一下css文件的内容，然后执行命令：git status -s查看状态

![image-20230817141443339](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171414244.png)

后面就会跟着一个M 已修改状态 (M)  被修改过的文件。

#### 4 改动内容同步到暂存区

如果想让工作区里的改动同步到暂存区则需要再重复执行命令：git add .

![image-20230817141704591](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171417881.png)

工作区里面就是最新的了

#### 5 将暂存区的文件提交到版本库

执行命令：git commit -m ‘备注’ ，再执行 git status -s 后 可以看到当前没有任何变化的文件了，现在就是未修改状态了 (‘’) 三个区域统一。

![image-20230817142029345](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171420856.png)

如果再对css文件进行内容的更改就有 处于 已修改状态了 (M) 

![image-20230817142323012](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171423614.png)

>  总结：
>
>  1.  Git 文件状态分为哪2种？
>      -  未跟踪和已跟踪 (新添加，未修改，已修改)
>  2.  如何查看暂存区和工作区文件状态？
>      -  git status -s

### Git 暂存区使用

暂**存区**：暂时存储，可以临时恢复代码内容，与版本库解耦

暂存区 -》 覆盖 -》 工作区 ，**命令**：git restore 目标文件 (<span alt='solid'>注意</span>：<font title='red'>完全确认覆盖时使用</font>)

从暂存区移除文件，**命令**：`git rm --cached`目标文件

![image-20230817143038646](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171430065.png)

**演示**：

#### 演示 git restore

查看暂存区的文件列表：

![image-20230817143328392](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171433851.png)

暂存区中有两个代码文件，我想重写一个html文件的代码

原本的代码内容：里面是经过webpack打包压缩的 不过没关系 照改

![image-20230817143518094](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171435559.png)

更改后的代码内容：

![image-20230817143625820](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171436138.png)

查看页面样式

![image-20230817143653837](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171436381.png)

我有觉得太丑了没有之前那个写的好，但是那么多代码和思路 ， 总不能完美复刻上一次的代码把。怎么办呢？

git中执行命令：git restore 将index.html在暂存区中的那个代码文件覆盖工作区的这个 index.html

![image-20230817143945385](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171439985.png)

查看index.html文件的内容，可以看到之前的代码就回来了

![image-20230817143917985](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171439980.png)

#### 演示 `git rm --cached` 

执行命令：git ls-files 查看暂存区中的文件

![image-20230817144237248](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171442503.png)

将index.css从暂存区中移除，执行命令：`git rm --cached [目标文件]` 

![image-20230817144355199](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171443352.png)

### 练习—登录页面

**需求**：新增JS代码并暂存提交产生新的版本快照

**步骤**：

1.  新增JS文件和内容
2.  临时存放在暂存区
3.  提交保存到版本库

![image-20230817145450013](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171454405.png)

将登录页面的项目代码拷贝到git仓库的目录下

![image-20230817151633818](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171516124.png)

使用命令将项目所有代码都添加到暂存区里面

![image-20230817151732684](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171517420.png)

提交到版本库中

![image-20230817151804855](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171518156.png)

查看提交的历史记录

![image-20230817151835956](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171518633.png)

>  上图其中的 HEAD 表示本次提交分支，它指向了 主线 (主仓库) ，最前面的 字符串序列 是版本序列号

### Git 回退版本

>  概念：把版本库某个版本对应的内容快照，恢复到工作区 / 暂存区

查看提交历史：`git log --oneline` 

![image-20230817152320802](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171523118.png)

**回退命令**：

![image-20230817153622766](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171536327.png)

###### 第一种模式

`git reset --soft 版本号`(其它文件未跟踪)

执行命令后，会将对应<font title='red'>版本库</font>的序列号的文件以及内容<span alt='wavy'>恢复</span>到 <font title='red'>暂存区</font> 和 <font title='red'>工作区</font>^覆盖原有文件的内容^ 尽可能<span alt='wavy'>保留 工作区 和 暂存区 里面的文件</span> ，但是这些<span alt='wavy'>保留的文件会变为 未被跟踪状态 </span>(U)

![image-20230817153547119](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171535610.png)

###### 第二种模式

`git reset --hard 版本号` 

执行命令后，<font title='red'>暂存区</font>就<span alt='wavy'>只有版本库中对应的文件以及内容了其它的都会被</span><font title='red'>清除掉</font>而且<font title='red'>工作区</font>也是<span alt='wavy'>同样的效果</span>。这种模式<span alt='solid'>比较彻底</span>.

![image-20230817153610464](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171536906.png)

###### 第三种模式

`git reset --mixed 版本号`(与 git reset 等价)

执行命令后，<font title='red'>暂存区</font>里面的文件以及内容<span alt='wavy'>被版本库对应的文件覆盖</span>，但是在<font title='red'>工作区</font>中<span alt='wavy'>保留它原有的文件以及内容</span>.

![image-20230817153648781](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171536075.png)

<blockquote alt='danger'>
	<div>
      <p>
         <span><font title=red>注意1</font>：只有记录在版本库的提交记录才能恢复</span>
      </p>
      <p>
         <span><font title=red>注意2</font>：回退后，继续修改 -> 暂存 -> 提交操作即可 (产生新的提交记录过程)</span>
      </p>
   </div>
</blockquote>

#### 演示

##### 第三种模式

执行命令查看暂存区中的文件：git ls-files

![image-20230817155035750](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171550533.png)

查看所有版本库对应的序列号，对应序列号进行回退

执行命令：

`git log --oneline` 

`git reset --mixed 90d394d` 

然后查看暂存区中的文件以及内容：git ls-files 可以看到里面的文件以及内容被对应序列号版本库里面的文件以及内容覆盖了，其它的被清除掉了，但是并不会清除 工作区的文件以及内容

![image-20230817155053525](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171550378.png)

查看工作区的情况

![image-20230817155331841](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171553373.png)

##### 第二种模式

执行命令进行回退：`git reset --hard 90d394d` 

![image-20230817155536935](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171555624.png)

查看工作区的情况

![image-20230817155624192](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171556134.png)

工作区没有被覆盖，但是上面说，暂存区 和 工作区 都会被覆盖 并 清除其它的文件以及内容的啊。

**解释**：

因为在上面使用第三种模式回退时。暂存区 与 对应版本库 文件以及内容一致 是 未修改状态(‘’) ，而且清除了其它的文件以及内容。而工作区 只是覆盖了 对应版本库 的文件以及内容 ，其它的文件变成了 未跟踪状态了 (U) 。所以 工作区 再执行 回退 那些 未跟踪的 文件以及 内容也不会发生任何事情。

它也是从右向左进行 对照 影响的。

**解决**：

执行命令：git add . 让 工作区 与 暂存区 的文件以及内容一致 然后在进行回退

![image-20230817160110680](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171601944.png)

查看工作区情况

![image-20230817160131858](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171601270.png)

回退之后再进行查看版本库的历史版本

![image-20230817160430318](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171604957.png)

#### 查看完整版本库历史日志

因为回退到了对应的版本后之后的版本都不见了，怎么办呢？

![image-20230817160720559](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171607163.png)

**解决**：

使用完整查看 版本库历史 日志的命令：`git reflog --online` 

![image-20230817160742375](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171607640.png)

再次回退到 最新的版本库 ，也就是 :  登录页面-开发完毕 的这个

执行命令：`git reset --hard 16babbc`

![image-20230817160921582](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171609122.png)

查看回退之后的暂存区和工作区的情况

![image-20230817161045206](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171610890.png)

>  <span alt='solid'>总结</span>：
>
>  1.  什么是Git回退版本？
>      -  把版本库某个对应的内容快照，恢复到工作区/暂存区
>  2.  强制覆盖暂存区和工作区的命令？
>      -  `git reset --hard 版本号` 
>  3.  如何查看提交历史？
>      -  `git log --oneline` 
>      -  查看完整的：`git reflog --oneline` 

### 删除文件

**需求**：删除 editor.js 文件，并产生一次版本记录

**步骤**：

1.  手动删除 工作区 文件
2.  暂存变更/手动删除暂存区文件造成变更
    -  `git add .` 
    -  `rm -f --cached 路径/文件名` 
3.  提交保存

>  <span alt='solid'>总结</span>：
>
>  工作区只要改变，都可以暂存提交产生新记录

### 忽略文件

>  概念：`.gitignore`配置文件可以让 git 彻底 <font title='red'>忽略跟踪</font> 指定文件

**目的**：让 git 仓库更小更快，避免重复无意义的文件管理

**例如**：

1.  系统或软件自动生成的文件
2.  编译产生的结果文件
3.  运行时生成的日志文件，缓存文件，临时文件等
4.  涉密文件，密码，密钥等文件

![image-20230817162932902](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171629450.png)

**创建**：

1.  项目根目录新建`.gitignore` 文件
2.  填入相应配置来忽略指定文件
3.  不用管要忽略的文件在哪个位置直接写它的名字就行

<blockquote alt='danger'>
	<div>
      <p>
         <span><font title=red>注意</font>：如果文件已经被暂存区跟踪过，可以从暂存区移除即可</span>
      </p>
   </div>
</blockquote>

演示：

项目根目录创建`.gitignore` 忽略配置文件 里面配置 忽略 叫 password.txt 的文件

![image-20230817163519830](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171635902.png)

在项目的tuils目录中有一个 password.txt 文件

![image-20230817163600805](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171636948.png)

之前的时候 将项目添加到暂存区的时候是有 password.txt 文件的并且 添加到了暂存区

![image-20230817163700550](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171637426.png)

将password.txt文件从暂存区中移除

![image-20230817163837250](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171638717.png)

然后再将工作区的代码 添加到暂存区 这时候是有 忽略配置文件的。

![image-20230817164138303](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171641922.png)

这样password.txt指定要被忽略的文件就被忽略了

### 分支

>  概念：本质上是指向 <font title='red'>提交节点</font> 的可变 <font title='red'>指针</font>，默认名字是 master

<blockquote alt='danger'>
	<div>
      <p>
         <span><font title=red>注意</font>：<span alt='solid'>HEAD指针</span></span>影响工作区/暂存区的代码状态</span>
      </p>
   </div>
</blockquote>

![image-20230817164441779](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171644086.png)

**场景**：开发<font title='red'>新需求/修复BUG</font>，保证主线代码随时可用，多人协同开发提高效率

![image-20230817164853276](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171656807.png)

假如说公司招了一个前端新人，我想让它接着我的项目代码接着写。但是我不想让它影响到我现在已经开发完的主线代码。怎么办呢？

这就需要用到分支概念了

**例如**：在现有代码上创建新分支完成内容列表业务

所以可以让在现有的分支基础上创建一个content分支，让这个分支下来实现内容列表业务的开发

![image-20230817165128330](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171656679.png)

这样就不会影响我master默认主分支的代码了。

也就是说content分支下暂存提交产生的版本记录不会影响master分支下看到的代码，开发完成后再合并回到master主分支下

---

突然需要紧急修复BUG—单独创建分支解决BUG

![image-20230817165556763](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171655189.png)

#### 创建分支

**需求**：创建内容列表 content 分支，并产生3次提交记录

**步骤**：

1.  创建分支命令：`git branch 分支名` 

    ![image-20230817165818460](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171658414.png)

    <blockquote alt='danger'>
       <div>
          <P>
             <span>创建分支以当前HEAD指针，指向的提交记录作为起点。将新的分支指向HEAD指向的提交记录</span>
          </P>
       </div>
    </blockquote>

2.  切换分支命令：`git checkout 分支名` 

    ![image-20230817170110799](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171701078.png)

    让HEAD指针指向新创建的分支，HEAD会影响 工作区和暂存区 的代码。

3.  工作区 准备 代码 并暂存提交，重复3次

    ![image-20230817170330021](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171703585.png)

    命令：`git branch` 查看当前git仓库当中有哪些分支

    ![image-20230817170607707](./Git.assets/image-20230817170607707.png)

    提交三次版本库代码

    ![image-20230817172211736](./Git.assets/image-20230817172211736.png)

    查看提交记录

    ![image-20230817172259808](./Git.assets/image-20230817172259808-1692264248011-5.png)

    content分支 新增加的代码内容

    ![image-20230817172420105](./Git.assets/image-20230817172420105.png)

    切换到master主分支，切换后 content 分支创建的项目就没有了

    ![image-20230817172500666](./Git.assets/image-20230817172500666.png)

>  总结：
>
>  1.  什么是Git分支？
>      -  指针，指向提交记录
>  2.  HEAD指针的作用？
>      -  影响暂存区和工作区的代码
>  3.  如何创建和切换指针？
>      -  git branch 分支名
>      -  git checkout 分支名

### 练习—登录BUG修复

需求：新建 login-bug 分支，做2 次提交记录 (对手机号长度，验证码长度做判断)

步骤：

1.  切回到主分支：git checkout master
2.  创建新分支：git branch login-bug
3.  切换新分支：git checkout login-bug
4.  修改代码，暂存，提交产生版本记录

![image-20230817173236845](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171732512.png)

修复点代码后添加到暂存区然后提交到版本库中

![image-20230817174037592](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171740320.png)

### 分支—合并与删除

**需求**：把login-bug合并回到master分支并删除login-bug分支

**步骤**：

1.  切回到合入的分支上：git checkout master
2.  合并其它分支过来：git merge login-bug
3.  删除合并后的分支指针：git branch -d login-bug

合并分支：

![image-20230817174655075](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171749315.png)

![image-20230817174748472](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171749622.png)

分支合并后master就拿到了login-bug修复bug后的代码了那么它的任务也就完成了。

#### 移除分支：git branch -d 分支名

![image-20230817174944702](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171749922.png)

### 分支—合并与提交

**合并提交**：发生于 <font title='red'>原分支</font> 产生了<font title='red'> 新的提交</font> 记录后，再<font title='red'>合并</font>回去时发生，自动使用多个快照记录合并后产生一次新的提交

步骤：

1.  切回到要合入的分支上：git checkout master
2.  合并其它分支过来：git merge content
3.  删除合并后的分支：git branch -d content

![image-20230817175358515](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171758341.png)

最后合并回到主分支上时，提交记录流程图：

![image-20230817175811579](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171758112.png)

<blockquote alt='danger'>
   <div>
      <P>
         <span><font title='red'>注意</font>：提交记录的顺序按照产生的先后顺序排列，而非合并后的先后顺序</span>
      </P>
   </div>
</blockquote>

![image-20230817180223745](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171924499.png)

合并后发现，content 分支还在指向它原来的分支，在HEAD指向的master分支下，及拥有login-bug分支的代码，又拥有content提交的最新代码，它将这两个分支的代码进行了ort的策略的合并提交

合并后content分支就没有用处了，将其移除。

![image-20230817192727577](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171927001.png)

### 分支—合并冲突

需求1：基于master新建publish分支，完成发布文章业务，然后修改内容页面的html文件的title标签内容，并提交一次。

需求2：切换到master，也在修改内容页面的html文件的title标签修改内容，并提交一次。

冲突：把publish分支合并到master回来，产生合并冲突

概念：<font title='red'>不同分支中</font>，对于<font title='red'>同一个文件</font>的<font title='red'>同一部分修改</font>，Git 无法干净的合并，产生合并冲突

解决：

1.  找到冲突文件并手动解决
2.  解决后需要提交一次记录

![image-20230817193939951](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171939278.png)

演示

在publish分支中，在项目中创建一个publish目录并将黑马头条中的publish模块代码放入

![image-20230817195427952](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308171954472.png)

在publish分支下修改content/index.html页面的titile标签内容

![image-20230817195602595](./Git.assets/image-20230817195602595.png)

提交记录

![image-20230817194212620](./Git.assets/202308171942914.png)

切换到master分支，并修改content/index.html的title标签的内容。

![image-20230817195730476](./Git.assets/image-20230817195730476.png)

![image-20230817195757409](./Git.assets/image-20230817195757409.png)

提交记录

![image-20230817195855697](./Git.assets/image-20230817195855697.png)

将publish分支进行合并

![image-20230817200320155](./Git.assets/image-20230817200320155.png)

此时就产生了合并冲突

### Git常用命令

| 命令                             | 作用                                 | 注意                                                         |
| -------------------------------- | ------------------------------------ | ------------------------------------------------------------ |
| git -v                           | 查看git版本                          |                                                              |
| git init                         | 初始化git仓库                        |                                                              |
| git add 路径/文件名              | 暂存某个文件                         | 文件标识以终端为起始的相对路径                               |
| git add .                        | 暂存所有文件                         |                                                              |
| git commit -m ‘’                 | 提交产生版本记录                     | 每次提交，把暂存区内容快照一份                               |
| git status                       | 查看文件状态-详细信息                |                                                              |
| git status -s                    | 查看文件状态-简略信息                | 第一列是暂存区状态，第二列是工作区状态                       |
| git ls-files                     | 查看暂存区文件列表                   |                                                              |
| `git restore 路径/文件名`        | 从暂存区恢复到工作区                 | 如果文件标识为 . 则恢复所有文件                              |
| `git rm --cached 路径 /文件名`   | 从暂存区移除文件                     | 不让git跟踪文件变化                                          |
| `git log`                        | 查看提交记录-详细信息                |                                                              |
| `git log --oneline`              | 查看提交记录-简略信息                | 版本号 分支指针 提交时说明注释                               |
| `git reflog --oneline`           | 查看完整历史-简略信息                | 包括提交， 切换，回退等所有记录                              |
| `git reset --hard 版本号` (常用) | 切换版本代码到暂存区和工作区         | `--soft` 模式保留暂存区和工作区原本内容<br />`--hard`模式不保留暂存区和工作区原本内容<br />`--mixed`模式不保留暂存区，工作区保留(默认)<br />先覆盖到暂存区，再用暂存区对比覆盖工作区 |
| `git branch 分支名`              | 创建分支                             |                                                              |
| `git  branch`                    | 查看本地分支                         |                                                              |
| `git branch -d 分支名`           | 删除分支                             | 请确保记录已经合并到别的分支下，再删除分支                   |
| `git checkout 分支名`            | 切换分支                             |                                                              |
| `git checkout -b 分支名`         | 创建并立刻切换分支                   |                                                              |
| `git merge 分支名`               | 把分支提交历史记录合并到当前所在分支 |                                                              |
| `git branch -D 分支名`           | 强制删除分支                         | 一般不建议使用                                               |
| `git branch -m 分支命名`         | 修改分支的名称                       |                                                              |

### Git 远程仓库

**概念**：托管在因特网或其它网络中的你的项目的<font title='red'>版本库</font>.

**作用**：保存版本库的历史记录，多人协作

**创建**：公司自己服务器/第三方托管平台 (Gitee，GitLab，GitHub … )

![image-20230817203135688](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172031783.png)

**步骤**：

1.  新建仓库得到远程仓库Git地址

2.  本地Git仓库添加远程仓库原点地址

    命令：`git remote add 远程仓库别名 远程仓库地址`

    例如：`git remotea add origin https://xxx.xxx.git`

3.  本地Git仓库推送版本记录到远程仓库

    命令：`git push -u 远程仓库别名 本地和远程分支名`

    例如：`git push -u origin master`

    完整写法：`git push --set-upstream origin master:master`

![image-20230817203743706](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172037228.png)

![image-20230817204421000](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172057840.png)

命令：git remote -v 查看本地git仓库中都有哪些远程仓库的地址

fetch：从哪个远程仓库取来对应版本库的内容

push：往哪个远程仓库版本库里面推送

如果远程仓库的地址填错了，想要换一个不能直接再添加一次而是移除后再添加如下：

移除远程仓库的地址：`git remote remove origin` 

![image-20230817204740674](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172057018.png)

推送代码到gitee上

执行命令：`git push -u origin master` 

![image-20230817205150194](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172057935.png)

查看gitee对应的仓库的变化

![image-20230817210012892](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172100506.png)

第一次使用git的时候，它会弹出gitee的一个登录器，让你用账号密码登录才能推送代码，使用https开头的协议它需要账号，密码进行连接。如果要是换了gitee账号 那怎么办呢？

<span alt='solid'>解决如下</span>：

在windows上打开控制面板，进入到 凭据管理器，点击windows凭据 中看到有一个 git:gitee的网址 点击展开，将其删除。我们再使用https开头的地址它就会再次让我们输入对应新的账号，密码了。

![image-20230817205535447](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172058682.png)

>  总结：
>
>  1.  远程版本库的作用？
>      -  保存提交历史记录，多人共享
>  2.  远程版本库使用步骤？
>      -  创建远程版本库 (自己服务器/第三方托管平台)
>      -  本地版本库设置远程地址
>      -  推送本地版本库到远程
>  3.  推送的命令？
>      -  `git push -u origin master`

### Git 远程仓库—克隆

**克隆**：拷贝一个Git仓库到本地，进行使用

>  <span alt='solid'>得到一个Git仓库有两种方式</span>：
>
>  1.  使用`git init`自己在本地转换
>  2.  克隆仓库到本地使用

**命令**：<font title='red'>git clone 远程仓库地址</font>，例如：`git clone https://xxx.xxx.git` 

**效果**：在运行命令所在文件夹，生成work项目文件夹 (包含版本库，并映射到暂存区和工作区)

<blockquote alt='danger'>
	<div>
      <P>
         <span><font title='red'>注意1</font>：Git 本地仓库已经建立好和远程仓库的链接</span>
      </P>
      <P>
         <span><font title='red'>注意2</font>：仓库公开随意克隆，推送需要身为仓库团队成员</span>
      </P>
   </div>
</blockquote>

演示：

终端cd到指定的目录下执行命令：`git clone https://xxx.xxx.git` 

复制要克隆的地址

![image-20230817213248571](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172132553.png)

![image-20230817213640684](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172138708.png)

克隆之后进入目录中，里面有一个.git隐藏文件表示是一个仓库

![image-20230817213722667](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172138542.png)

执行命令查看提交的历史记录：`git log --oneline` 

![image-20230817213836924](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202308172138187.png)

### 多人协同开发

需求：小传新代码共享给小智

步骤：

1.  小传开发代码  -> 工作区 -> 暂存区 -> 提交 -> 拉取 (可选) -> 推送
2.  小智 -> 拉取 (后续也可以开发代码 -> … -> 推送)
3.  想要看别人同步上去的最新内容：`git pull origin master`等价于
    1.  `git fetch origin master:master`(获取远程分支记录到本地，未合并)
    2.  `git merge origin/master`(把远程分支记录合并到所在分支下)



### Git 命令总结

| 命令                                       | 作用               | 注意                                                         |
| ------------------------------------------ | ------------------ | ------------------------------------------------------------ |
| `git remote add 远程仓库别名 远程仓库地址` | 添加远程仓库地址   | 别名唯一，地址是.git结尾的网址                               |
| `git remote -v`                            | 查看远程仓库地址   |                                                              |
| `git remote remove 远程仓库别名`           | 删除远程仓库地址   |                                                              |
| `git pull 远程仓库别名 分支名`             | 拉取               | 完整写法：`git pull`远程仓库别名 远程分支名:本地分支名<br />等价于：`git fetch`和`git merge` |
| `git push 远程仓库别名 分支名`             | 推送               | 完整写法：`git push`远程仓库别名 本地分支名:远程分支名 -u: 建立通道以后可以简写 git push |
| `git pull --rebase 远程仓库别名 分支名`    | 拉取合并           | 合并没有关系的记录                                           |
| `git clone 远程仓库地址`                   | 克隆               | 从0得到一个远程的Git仓库到本地使用                           |
| `git push -u -f 远程仓库别名 分支名`       | 强制推送           | 会覆盖原有的内容 慎用                                        |
| `git clone -b [分支名] 远程仓库地址`       | 拉取指定的分支项目 |                                                              |

### 推送超出文件大小范围的解决方式

打开当前初始化.git的文件里面的config配置文件 配置如下

```sh
[http]  
postBuffer = 924288000
```

