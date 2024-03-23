---
title: 七、Docker
categories:
    - [计算机学科,java,spring,springcloud,Docker]
tags:
    - 计算机学科
    - springcloud
    - Docker
---

## 七、Docker:christmas_tree: 

查看docker安装：[Docker安装](./Docker/Centos7安装Docker.md).

-  初始Docker
-  Docker的基本操作
-  Dockerfile自定义镜像
-  Docker-Compose
-  Docker镜像服务

### 7.1、初始Docker:deciduous_tree: 

-  什么是Docker
-  Docker和虚拟机的区别
-  Docker架构
-  安装Docker

>  项目部署的问题
>
>  -  依赖关系复杂，容易出现兼容性问题
>  -  开发，测试，生产环境有差异
>
>  大型项目组件较多，运行环境也较为复杂，部署时会碰到一些问题：
>
>  所有的应该都会部署到服务器上，而大多数服务都会采用Linux操作系统，在安装到Linux操作系统之前要做一些准备工作。因为这些应用都会有自己所需要的依赖和函数库。这时如此复杂的依赖关系很容易出现，兼容性问题，废了半天劲把这些问题都解决了你会发现这只是开始搞定了开发环境，还有测试环境，生产环境，预发环境等等。最可怕的是这些环境它们的Linux操作系统有可能不同

![image-20231008093734523](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310080937247.png)

#### 7.1.1、Docker如何解决依赖的兼容问题？:evergreen_tree: 

-  将应用的Libs(函数库)，Deps(依赖)，配置与应用一起打包
-  将每个应该放到一个隔离**容器**去运行，避免互相干扰

![image-20231008094222259](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310080942590.png)

现在不用考虑依赖的问题了，但是这只是解决了混乱依赖的问题。有了Docker不用去管了应用程序随时可以运行但是仅限于同一个操作系统。因为在打包这个应用时肯定会基于某种操作系统打包吧，比如这个应该是ubuntu版本的那么它的函数库和依赖肯定也是ubuntu版本的。把这个打包好的程序放到CentOs上肯定不能运行，那么Docker是怎么解决这个问题的呢？

要想知道Docker如何跨系统运行，我们得先知道操作系统的结构

#### 7.1.2、基本了解操作系统结构:evergreen_tree: 

以ubuntu为例讲讲操作系统结构

![image-20231008094755054](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310080947687.png)

所有的Linux操作系统都可以分为两层：

一层是大家共享的内核(Linux内核)

而区别在于不同操作系统之间它们的上层系统应用不同

>  那内核做什么的呢？
>
>  内核与硬件交互，提供操作硬件的指令

>  系统应用做什么的呢？
>
>  将内核的指令进行一个组装，再封装形成函数，区域多的函数变成函数库，程序员可以基于这些函数库进行开发。程序调用函数库，函数库调用内核指令，指令调用计算机硬件从而实现应用的执行

![image-20231008095659859](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310080957223.png)

这就是Linux系统的基本结构以及应用的执行原理了

那么问题来了：为什么Ubuntu系统上的应用为什么不能在CentOs上运行呢？

Ubuntu和CentOS都是基于Linux内核，只是系统应用不同，提供的函数库有差异

这时把Ubuntu上的应用比如说mysql给它迁移到CentOS上尝试去执行当它去调用一个函数库时，因为代码是写死的而发现这个函数库在centos上根本不存在或者名字不一样那么程序就会报错。这就是为什么 应用不能跨系统运行的原因

![image-20231008095848662](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310080958994.png)

那针对上面的问题Docker干了什么事儿呢？

-  Docker将用户程序与所需要调用的系统(比如Ubuntu)函数库一起打包
-  Docker运行到不同操作系统时，直接基于打包的库函数，借助于操作系统的Linux内核来运行。

将当前系统的函数库与应用一起打包那么随便放到任何的Linux操作系统上只要是Linux内核，应用在执行的时候调用打包好的函数库，而这个函数库直接调用操作系统的内核而内核直接访问硬件这个调用就完成了。

![image-20231008105934407](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081059529.png)

至此，Docker就解决了跨系统的问题了，可以理解为Docker打包好的应用可以运行在任何的Linux内核的操作系统上

>  **Docker**：
>
>  Docker如何解决大型项目以来关系复杂，不同组件依赖的兼容性问题？
>
>  -  Docker允许开发中将应用，依赖，函数库，配置一起<font color='red'>打包</font>，形成可移植镜像
>  -  Docker应用运行在容器中，使用沙箱机制，相互<font color='red'>隔离</font>.
>
>  Docker如何解决开发，测试，生产环境有差异的问题
>
>  -  Docker镜像中包含完整运行环境，包括系统函数库，仅依赖系统的Linux内核，因此可以在任意Linux操作系统上运行

>  总结：
>
>  Docker是一个快速交付应用，运行应用的技术：
>
>  1.  可以将程序及其依赖，运行环境一起打包为一个镜像，可以迁移到任意Linux操作系统
>  2.  运行时利用沙箱机制形成隔离容器，各个应用互不干扰
>  3.  启动，移除都可以通过一行命令完成，方便快捷

### 7.2、Docker与虚拟机:deciduous_tree: 

我们知道Docker可以实现一个应用可以在不同的Linux操作系统上运行和部署。而虚拟机也可以达到类似的效果，那它们两个的实现又有什么样的差别呢

首先回顾下Docker的实现原理，Docker会把应用及其所需要的依赖，函数库，甚至于操作系统函数库也一起打包，这样 当应用运行时可以直接调用本地函数库而后与操作系统的内核进行交互。这样就不用关心操作系统是什么系统了，于是就能实现跨系统运行了

![image-20231008111240399](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081112122.png)

那虚拟机呢？

它怎么实现在一个系统里装了另一个系统呢。它是用了一个名为：Hyperisor的技术，可以模拟出一个计算机的各种各样的硬件比如CPU，内存等等，然后在这个模拟出的计算机上就可以去安装任意你想要的操作系统了，而操作系统又可以安装任意适合的依赖，函数库以及应用，那这样它不就实现跨系统的部署了吗

![image-20231008111617105](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081116627.png)

虚拟机是在一个系统里装了另外一个系统，所以当应用执行时，它会以为自己在一台真实的电脑上。因此它会先调用虚拟机内置的操作系统而它在与Hypervisor进行交互，然后再把信息传递给外部的操作系统，外部操作系统再去调用计算机硬件，于是应用执行就完成了。

可以看到虚拟机启动应用经过了层层传递所以性能是比较差的

<center>两者之间的差异</center>

| 特性     | Docker   | 虚拟机   |
| -------- | -------- | -------- |
| 性能     | 接近原生 | 性能较差 |
| 硬盘占用 | 一般为MB | 一般为GB |
| 启动     | 秒级     | 分钟级   |

>  **总结**：
>
>  Docker和虚拟机的差异：
>
>  -  Docker是一个系统进程；虚拟机是在操作系统中的操作系统
>  -  Docker体积小，启动速度快，性能好；虚拟机体积大，启动速度慢，性能一般

### 7.2、镜像和容器:deciduous_tree: 

**镜像 (Image)**：Docker将应用程序及其所需的依赖，函数库，环境，配置等文件打包在一起，称为镜像。

简单可以理解为，镜像就是一个文件。

![image-20231008112658929](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081127213.png)

**容器 (Container)**：镜像中的应用程序运行后形成的进程就是<font color='red'>容器</font>，只是Docker会给容器做隔离，对外不可见。

简单可以理解为，把这个mysql应用运行起来形成的这个进程就是容器，只不过在Docker里面容器还要做隔离

也就是说我们可以将容器看做是一个小盒子，这个盒子里面将来会利用Linux手段给它形成隔离空间。里面会有自己独立的CPU资源，内存资源，甚至于还有独立的文件系统。那么因此在这个容器内运行的这个进程，它就会以为自己是在这台计算机上的唯一进程了从而起到一种隔离的效果，将来mysql镜像不管是启动一个容器还是多个容器，它们之间都是相互隔离的，当容器运行的过程中必然会做数据读写的操作。比方说mysql要去存数据，存储到data目录中。但是不能将数据写到镜像的data目录中去。镜像都是只读的

![image-20231008124821994](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081248648.png)

### 7.3、共享镜像:deciduous_tree: 

#### 7.3.1、Docker和DockerHub:evergreen_tree: 

-  DockerHub：是一个Docker镜像的托管平台。这样的平台称为Docker Registry。

程序员可以利用Docker提供的一些命令去完成镜像的构建，构建出mysql，redis等等各种各样的镜像

然后把这些镜像上传到DockerHub服务器上

-  国内也有类似于DockerHub的公开服务，比如 网易云镜像服务，阿里云镜像库等。
-  可以搭建私有的云服务

这些都叫Docker Registry

![image-20231008125404165](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081254620.png)

我们要怎么利用Docker完成镜像构建或者是从远端拉取镜像呢又该怎么去运行容器呢？

这就需要了解Docker的架构了

### 7.4、Docker架构:deciduous_tree: 

Docker是一个CS架构的程序，由两部分组成：

-  服务端(server)：Docker守护进程，负责处理Docker指令，管理镜像，容器等

   负责接收用户指令完成镜像和容器的各种各样的操作，不管是构建镜像还是从远端拉取镜像还是运行容器都是由server端来完成的 ，那谁给server端下指令呢？自然就是client端了

-  客户端(client)：通过命令或RestAPI向Docker服务端发送指令。可以在本地或远程向服务端发送指令。

   这个要看在哪发送指令，如果是在Docker所在的机器上就直接用命令就可以了。如果是远程操作Docker就用RestAPI发

比方说就是本地发通过命令：dokcer build可以构建镜像，这个命令到达docker server后会被docker的守护进程docker daemon接收和处理。它会利用你提供的数据构建成一个镜像。除了这种方式获取镜像以外我们还可以去docker registry中拉取镜像，使用命令：docker pull 这个命令发送到docker server端后docker daemon就会去docker registry中拉取我们指定的镜像了。下面就是运行镜像创建容器了，使用命令：docker run 这个命令发送到docker server端后docker daemon就会去帮助我们完成容器的创建，然后部署就完成了。

![image-20231008130700555](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081307167.png)

>  **总结**：
>
>  镜像：
>
>  -  将应用程序及其依赖，环境，配置打包在一起
>
>  容器：
>
>  -  镜像运行起来就是容器，一个镜像可以运行多个容器
>
>  Docker结构：
>
>  -  服务端：接收命令或远程请求，操作镜像或容器
>  -  客户端：发送命令或者请求到Docker服务端
>
>  DockerHub：
>
>  -  一个镜像托管的服务器，类似的还有阿里云镜像服务，统称为DockerRegistry

### 7.5、Docker基本操作:deciduous_tree: 

-  镜像操作
-  容器操作
-  数据卷 (容器数据管理)

#### 7.5.1、镜像相关命令:evergreen_tree: 

-  镜像名称一般分两部分组成：[repository]:[tag]

   tag一般指 版本

-  在没有指定tag时，默认是latest，代表最新版本的镜像

![image-20231008160421226](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081604324.png)

##### 7.5.1.1、镜像操作命令:palm_tree: 

Docker镜像的操作无非就是CRUD，比如local(本地)就是本地的Docker，我要在这个上面完成镜像的操作。比方说获取镜像，一般有两种获取方法。第一种为：本地获取，第二种为：远端获取。

本地获取：

需要一个名为Dockerfile文件，利用docker build命令把它构建成一个镜像，使用命令：docker pull从docker registry的服务里拉取镜像，获取到镜像想查看下可以使用命令：docker images，删除镜像使用命令：docker rmi

分享镜像有两种方式：

方式一：

使用命令docker push将镜像推送到镜像服务器

方式二：

使用docker save将镜像打包为zip然后使用u盘拷贝给别人。使用docker load将zip加载为镜像

![image-20231008161250665](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081612068.png)

#### 7.5.2、案例，从DockerHub中拉取一个nginx镜像并查看:evergreen_tree: 

**需求**：从DockerHub中拉取一个nginx镜像并查看

1、首先去镜像仓库搜索nginx镜像，比如[DockerHub](https://hub.docker.com/):

![image-20231008161836139](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081618473.png)

nginx后面的勋章表示这个是官网的正版镜像

![image-20231008162135908](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081621315.png)

点开后可复制命令，默认拉取的是最新版本的nginx

![image-20231008162412714](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081624383.png)

2、根据查看到的镜像名称，拉取自己需要的镜像，通过命令：docker pull nginx

![image-20231008162646280](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081626723.png)

3、通过命令：docker images 查看拉取到的镜像

![image-20231008162714111](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081627490.png)

#### 7.5.3、案例，利用docker save将nginx镜像导出磁盘，然后再通过load加载回来:evergreen_tree: 

**步骤**：

1、利用`docker xx --help`命令查看docker save和docker load的语法

```sh
[root@localhost ~]# docker save --help
# 命令示例：docker save [选项] 镜像名称
Usage:  docker save [OPTIONS] IMAGE [IMAGE...]
# 保存一个或多个镜像到一个tar文件中(tar压缩文件)
Save one or more images to a tar archive (streamed to STDOUT by default)

Aliases:
  docker image save, docker save
# 选项说明
Options:
# -o 输出的意思，输出到一个指定的文件当中
  -o, --output string   Write to a file, instead of STDOUT
[root@localhost ~]#
```

docker save 导出镜像为压缩文件

```sh
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    61395b4c586d   2 weeks ago   187MB
[root@localhost ~]# docker save -o nginx.tar nginx:latest
[root@localhost ~]# ls -ahl
total 183M
dr-xr-x---.  4 root root  191 Oct  8 04:47 .
dr-xr-xr-x. 17 root root  224 Jul 15 05:11 ..
-rw-------.  1 root root 1.4K Jul 15 05:12 anaconda-ks.cfg
-rw-------.  1 root root   78 Jul 15 05:20 .bash_history
-rw-r--r--.  1 root root   18 Dec 28  2013 .bash_logout
-rw-r--r--.  1 root root  176 Dec 28  2013 .bash_profile
-rw-r--r--.  1 root root  176 Dec 28  2013 .bashrc
-rw-r--r--.  1 root root  100 Dec 28  2013 .cshrc
drwxr-xr-x.  3 root root   20 Oct  8 03:52 etc
-rw-------.  1 root root 183M Oct  8 04:47 nginx.tar
drwxr-----.  3 root root   19 Jul 15 05:30 .pki
-rw-r--r--.  1 root root  129 Dec 28  2013 .tcshrc
-rw-------.  1 root root  841 Oct  8 03:56 .viminfo
[root@localhost ~]#
```

docker load 导入 压缩文件

先将docker中的镜像删除

```sh
[root@localhost ~]# docker rmi nginx:latest
Untagged: nginx:latest
Untagged: nginx@sha256:32da30332506740a2f7c34d5dc70467b7f14ec67d912703568daff790ab3f755
Deleted: sha256:61395b4c586da2b9b3b7ca903ea6a448e6783dfdd7f768ff2c1a0f3360aaba99
Deleted: sha256:1c69f36a0d9b59b762eaba410fa9fd01b85140670a8d49199a7b37702cc956c0
Deleted: sha256:bac209bfab6997cccf20779ae98d5f77a66867734499ecf604a50a5826f6b8a4
Deleted: sha256:859676f4cd3004af025a02dade096ad6f9391d94d1b1a983fc6098debe435055
Deleted: sha256:cbbd97cee0d824e5e82f9a4b2e93c5eb3c66fd72a2624d5c1521dc3395bfd1e2
Deleted: sha256:0b41545d8c3de3b78778d591a8da3d9dfa5fa8807baef5edf21e5eb94ded792d
Deleted: sha256:7e87866b23143eb30086086a669b2e902368a5836446a885b2411d3feef18bef
Deleted: sha256:d310e774110ab038b30c6a5f7b7f7dd527dbe527854496bd30194b9ee6ea496e
[root@localhost ~]#
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
[root@localhost ~]#
```

使用`docker load --help` 来查看命令的说明

```sh
[root@localhost ~]# docker load --help
# 命令示例：docker load [选项]
Usage:  docker load [OPTIONS]
# 从tar存档或STDIN加载图像
Load an image from a tar archive or STDIN

Aliases:
  docker image load, docker load
# -i ：输入，指定读取哪一个文件
# -q ：不会打印日志
Options:
  -i, --input string   Read from tar archive file, instead of STDIN
  -q, --quiet          Suppress the load output
[root@localhost ~]#
```

使用docker load命令加载镜像

```sh
[root@localhost ~]# docker load -i nginx.tar
d310e774110a: Loading layer [==================================================>]  77.81MB/77.81MB
eb7e3384f0ab: Loading layer [==================================================>]    113MB/113MB
1dc45c680d0f: Loading layer [==================================================>]  3.584kB/3.584kB
ea43d4f82a03: Loading layer [==================================================>]  4.608kB/4.608kB
9c6261b5d198: Loading layer [==================================================>]   2.56kB/2.56kB
a7e2a768c198: Loading layer [==================================================>]   5.12kB/5.12kB
d26d4f0eb474: Loading layer [==================================================>]  7.168kB/7.168kB
Loaded image: nginx:latest
[root@localhost ~]#
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    61395b4c586d   2 weeks ago   187MB
[root@localhost ~]#
```

>  **总结**：
>
>  镜像操作有哪些？
>
>  -  docker images
>  -  docker rmi
>  -  docker pull
>  -  docker push
>  -  docker save
>  -  docker load

#### 7.5.4、(自己练习)去DockerHub搜索并拉取一个Redis镜像:evergreen_tree: 

1、去DockerHub搜索Redis镜像

![image-20231008170209077](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081702255.png)

2、查看Redis镜像的名称和版本

![image-20231008170243226](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081702756.png)

3、利用docker pull命令拉取镜像，并使用docker images查看拉取的情况

```sh
[root@localhost ~]# docker pull redis
Using default tag: latest
latest: Pulling from library/redis
a803e7c4b030: Already exists
57e05a1ddec3: Pull complete
4635c857038a: Pull complete
10001c6989b4: Pull complete
f93feb85ceb4: Pull complete
4f4fb700ef54: Pull complete
320349bd761d: Pull complete
Digest: sha256:b68c6efe2c5f2d7d7d14a2749f66d6d81645ec0cacb92572b2fb7d5c42c82031
Status: Downloaded newer image for redis:latest
docker.io/library/redis:latest
[root@localhost ~]#
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    61395b4c586d   2 weeks ago   187MB
redis        latest    7c4b517da47d   4 weeks ago   153MB
[root@localhost ~]#
```

4、利用docker save命令将redis:latest打包为一个redis.tar包

```sh
[root@localhost ~]# docker save -o redis.tar redis:latest
[root@localhost ~]#
[root@localhost ~]# ls
anaconda-ks.cfg  etc  redis.tar
[root@localhost ~]#
```

5、利用docker rmi删除本地的redis:latest

```sh
[root@localhost ~]# docker rmi redis:latest
Untagged: redis:latest
Untagged: redis@sha256:b68c6efe2c5f2d7d7d14a2749f66d6d81645ec0cacb92572b2fb7d5c42c82031
Deleted: sha256:7c4b517da47d331a47827390b9e8eb1be7ee68133af9c332660001b4d447828d
Deleted: sha256:06dafc2b2808183db1be226fd99e9dd9d374db9ef86480ccdec56bcb644c821a
Deleted: sha256:d31af4bfba5cce8499ed4df2f666ef1cf911d0936156af46da79d4b510c7687a
Deleted: sha256:f14f2bfc00d1db007798b7b81d043df4130673ae33591960d0917c484eded7f0
Deleted: sha256:7778474585964f4869bd7b40e56a375312fc91284c26f7b84967efd383219a20
Deleted: sha256:8fbe6eb1ebe310862ce5ebf21c01744994203df7d3f53fecabf53db86c3f4578
Deleted: sha256:57489dc8f39be6c5bfe929829682b1dcd6c7497e2f5878f450557a381afacb52
[root@localhost ~]#
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    61395b4c586d   2 weeks ago   187MB
[root@localhost ~]#

```

6、利用docker load重新加载redis.tar文件

```sh
[root@localhost ~]# docker load -i redis.tar
ae2dc73bba26: Loading layer [==================================================>]  10.75kB/10.75kB
7c13dd56eefc: Loading layer [==================================================>]  4.141MB/4.141MB
33409257a91d: Loading layer [==================================================>]  75.28MB/75.28MB
c0a7ac48293c: Loading layer [==================================================>]  1.536kB/1.536kB
5f70bf18a086: Loading layer [==================================================>]  1.024kB/1.024kB
e6642d3d7b17: Loading layer [==================================================>]  4.096kB/4.096kB
Loaded image: redis:latest
[root@localhost ~]#
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    61395b4c586d   2 weeks ago   187MB
redis        latest    7c4b517da47d   4 weeks ago   153MB
[root@localhost ~]#
```

#### 7.5.5、容器相关命令:evergreen_tree: 

创建比较常见的一个命令就是docker run了，这个命令不仅仅可以帮我们创建一个容器而且还可以让这个容器处于运行状态。容器除了运行状态还有，暂停，停止两个状态。只需要简单的命令就可以实现两个状态之间的切换了

docker puse 可以让容器从运行状态进入到暂停状态

docker unpuse 让容器从暂停状态恢复到运行状态

docker stop 可以让容器从运行状态进入停止状态

docker start 让容器从停止状态恢复到运行状态

docker stop $(docker ps -q) 停止全部运行中的容器

docker rm $(docker ps -aq) 删除全部容器 运行中的需要强制删除 -f

`docker stop $(docker ps -q) & docker rm $(docker ps -aq)` 先停止全部容器再删除全部容器

>  那为什么暂停可以用puse 和 unpause 而停止却是 stop 和 start呢？为什么不是 unstop。
>
>  **原因**：
>
>  它们在操作系统的处理方式不同，如果容器进入暂停状态，操作系统会将容器内的进程挂起，容器关联的内存暂存起来。CPU不再执行这个进程，如果将它恢复那么内存空间恢复程序接着运行，那么容器就又进入运行状态了。
>
>  但是停止不同，停止直接把这个进程杀死，容器所占的内存回收。把保留下来的就仅剩容器的文件系统了也就是那些静态的东西，因此停止是没有办法进行恢复的因为进程已经被杀死了。
>
>  这就是暂停和停止的区别了。

![image-20231008172423860](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081724327.png)

##### 7.5.5.1、查看容器的状态:palm_tree: 

Docker提供了一个命令叫：docker ps 可以查看所有运行的容器以及状态，还有一个命令为：docker logs查看容器运行的日志

如果想深入内部了解一下，Docker还提供了一个命令：docker exec 可以进入容器的内部进行操作

docker rm 删除容器

![image-20231008172804953](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081728094.png)

##### 7.5.5.2、创建运行一个Nginx容器:palm_tree: 

1、去docker hub查看Nginx的容器运行命令

![image-20231008174414666](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081744053.png)

```sh
docker run --name some-nginx -d -p 8080:80 some-content-nginx
```

**命令解读**：

-  docker run：创建并运行一个容器
-  `--name`：给容器起一个名字，比如叫做mn
-  -p：将宿主机端口与容器端口映射，冒号左侧是宿主机端口，右则是容器端口
-  -d：后台运行容器
-  nginx：镜像名称，例如nginx

>  将宿主机端口与容器端口映射是什么意思呢？
>
>  比方说有一台centos的服务器ip地址为：192.168.150.101现在我们在这台服务器上部署了一台Nginx容器它的端口是80，如果用户想要来访问Nginx容器不能直接访问，因为容器是对外隔离的。不能访问怎么办呢，所以我们要做一个端口映射
>
>  **端口映射的解释**：
>
>  比如说宿主机有自己很多端口，其中一个端口为：80，让宿主机的80跟容器的80产生一个关联映射，那这样一来任何进入宿主机80的这个请求就都会被转发到容器的80端口去执行了。这样就等于是nginx处理了这个请求

![image-20231008175824602](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081758073.png)

所以端口映射的作用是把一个本来完全隔离的容器对外暴漏让用户可以访问

2、执行命令创建容器并运行

```sh
[root@localhost ~]# docker run --name mn -p 80:80 -d nginx
9672ddfc088cdf53966332bf06da44b6daf7093427c12ed636818bcaae20e312
[root@localhost ~]#
```

9672ddfc088cdf53966332bf06da44b6daf7093427c12ed636818bcaae20e312：容器的唯一id

3、查看容器的状态

```sh
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                               NAMES
9672ddfc088c   nginx     "/docker-entrypoint.…"   14 minutes ago   Up 14 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   mn
[root@localhost ~]#
```

4、访问服务

![image-20231008182123920](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081821712.png)

可以访问说明容器部署成功。

5、查看访问nginx后的日志信息

```sh
[root@localhost ~]# docker logs mn
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/10/08 10:04:30 [notice] 1#1: using the "epoll" event method
2023/10/08 10:04:30 [notice] 1#1: nginx/1.25.2
2023/10/08 10:04:30 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2023/10/08 10:04:30 [notice] 1#1: OS: Linux 3.10.0-1160.el7.x86_64
2023/10/08 10:04:30 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/10/08 10:04:30 [notice] 1#1: start worker processes
2023/10/08 10:04:30 [notice] 1#1: start worker process 29
2023/10/08 10:04:30 [notice] 1#1: start worker process 30
2023/10/08 10:04:30 [notice] 1#1: start worker process 31
2023/10/08 10:04:30 [notice] 1#1: start worker process 32
192.168.249.1 - - [08/Oct/2023:10:21:11 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36" "-"
[root@localhost ~]#
```

###### 7.5.5.2.1、持续输出日志:tanabata_tree: 

每次请求都要执行指令才能查看日志很麻烦通过`docker logs --help`来查看有没有需要的帮助

```sh
[root@localhost ~]# docker logs --help

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Aliases:
  docker container logs, docker logs

Options:
      --details        Show extra details provided to logs
  # 持续输出日志    
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. "2013-01-02T13:23:37Z") or relative (e.g. "42m" for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. "2013-01-02T13:23:37Z") or relative (e.g. "42m" for 42 minutes)
[root@localhost ~]#
```

我们可以看到选项中有一个持续输出日志的选项

>  **总结**：
>
>  docker run命令的常见参数有哪些？
>
>  -  `--name`：指定容器名称
>  -  `-p`：指定端口映射
>  -  `-d`：让容器后台运行
>
>  查看容器日志的命令：
>
>  -  docker logs
>  -  添加-f参数可以持续查看日志
>
>  查看容器状态：
>
>  -  docker ps

##### 7.5.5.3、进入Nginx容器，修改HTML文件内容，添加 “传智教育欢迎您”:palm_tree: 

1、进入容器。进入我们刚刚创建的nginx容器的命令为：

```sh
docker exec -it mn bash
```

**命令解读**：

-  docker exec：进入容器内部，执行一个命令
-  -it：给当前进入的容器创建一个标准输入，输出终端，允许我们与容器交互
-  mn：要进入的容器的名称
-  bash：进入容器后执行的命令，bash是一个linux终端交互命令

```sh
[root@localhost ~]# docker exec -it mn bash
root@9672ddfc088c:/#
```

通过ls命令查看内部都有什么：

```sh
root@9672ddfc088c:/# ls
bin   dev                  docker-entrypoint.sh  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   lib64  media   opt  root  sbin  sys  usr
root@9672ddfc088c:/#
```

这些我们常见的是linux根目录结构，但是这是阉割版的。容器的内部会有自己的一套文件系统，看起来和linux 的系统目录一样，但是这里只有nginx运行它自己需要的文件

要想看nginx存放到哪里了我们需要去Docker Hub的nginx发布页面查看：

![image-20231008184405334](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081844719.png)

2、进入nginx的HTML所在目录 /user/share/nginx/html

```sh
cd /user/share/nginx/html
```

3、修改index.html的内容

```sh
sed -i -e 's#Welcome to nginx#传智教育欢迎您#g' -e 's#<head>#<head><meta charset="utf-8">#g' index.html
```

再去访问nginx首页

![image-20231008191300720](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310081913648.png)

#### 7.5.6、查看停止的容器:evergreen_tree: 

先查看docker容器的运行状态

```sh
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED             STATUS             PORTS                               NAMES
9672ddfc088c   nginx     "/docker-entrypoint.…"   About an hour ago   Up About an hour   0.0.0.0:80->80/tcp, :::80->80/tcp   mn
[root@localhost ~]#
```

停止该容器

```sh
[root@localhost ~]# docker stop mn
mn
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@localhost ~]#
```

可以看到停止后通过docker ps查看状态就看不到nginx容器了，我们可以使用docker ps -a来进行查看被杀死的容器

```sh
[root@localhost ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED             STATUS                          PORTS     NAMES
9672ddfc088c   nginx     "/docker-entrypoint.…"   About an hour ago   Exited (0) About a minute ago             mn
[root@localhost ~]#
```

>  总结：
>
>  查看容器状态
>
>  -  docker ps
>  -  添加-a参数查看所有状态的容器
>
>  删除容器
>
>  -  docker rm
>  -  不能删除运行中的容器，除非添加-f参数
>
>  进入容器：
>
>  -  命令是`docker exec -it [容器名][要执行的命令]`
>  -  exec命令可以进入容器修改文件，但是在容器内修改文件时不推荐的

#### 7.5.7、数据卷:evergreen_tree: 

容器与数据耦合的问题

![image-20231008204446853](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310082044250.png)

上面的问题怎么解决呢，这就要学习 ==数据卷== 来解决了

数据卷 (volume) 是一个虚拟目录，指向宿主机文件系统中的某个目录。

>  比如说有一个Docker主机，那么在这个主机上就会由Docker去管理很多很多的数据卷。而所有的数据卷一定都会指向宿主机文件系统上的一个目录这个目录就是：var/lib/docker/volumes。

![image-20231008204850370](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310082048323.png)

>  比方说我们现在利用Docker创建了一个新的数据卷，这个数据卷的名字叫html，那么Docker一定会在/var/lib/docker/volumes指定的宿主机文件系统下创建出一个html的目录

![image-20231008205044637](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310082050696.png)

>  比方说我又创建了一个新的数据卷conf，它同样会在docker指定的volumes目录下再创建一个conf真是目录

![image-20231008205151242](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310082051742.png)

>  每一个数据卷都跟一个真是目录进行映射。所以可以认为数据卷是一个虚拟的并不真实存在的。只是一个概念，而真正指向的是(宿主机)硬盘上的真实文件夹

![image-20231008205305336](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310082053614.png)

>  容器创建后就可以使用这个数据卷，nginx的内部目录与数据卷进行关联，当容器中nginx与数据卷进行关联时，它的本质其实是和宿主机文件系统上的html和conf目录进行关联的。这时docker就会去管理这个容器了，比方说在容器的html目录里写了点什么东西，这些东西会立即写入到宿主机文件系统里。而反过来如果在宿主机里对html目录里某一个文件进行了修改那么也会立即反应到容器内的html目录中
>
>  可以理解为 容器 与 宿主机 之间 的数据同步是通过数据卷这个中间桥梁完成的

![image-20231008205636159](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310082056426.png)

>  这时就再也不用进入容器内去做修改了直接在宿主机通过高级编辑工具进行修改

##### 7.5.7.1、数据共享:palm_tree: 

将另一个容器挂载到数据卷的目录上也能访问操作里面的数据和配置的信息

![image-20231008210328135](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310082103306.png)

##### 7.5.7.2、数据安全删除问题:palm_tree: 

将容器删除，数据卷不会被删除，还可以将新的容器重新挂载到数据卷上里面的配置和数据都回来了

##### 7.5.7.3、操作数据卷:palm_tree: 

数据卷操作的基本语法如下：

```sh
docker volume[COMMAND]
```

docker volume 命令是数据卷操作，根据命令后跟随的command来确定下一步的操作：

-  create 创建一个volume
-  inspect 显示一个或多个volume的信息
-  ls 列出所有的volume
-  prune 删除未使用的volume
-  rm 删除一个或多个指定的volume

###### 7.5.7.3.1、案例，创建一个数据卷，并查看数据卷在宿主机的目录位置:tanabata_tree: 

1、创建一个名为html的数据卷

```sh
[root@localhost ~]# docker volume create html
html
[root@localhost ~]#
```

2、查看创建好的数据卷 使用命令列出所有的数据卷

```sh
[root@localhost ~]# docker volume ls
DRIVER    VOLUME NAME
local     2c26106500b13248f06d9d3d89e3f9cc4395f02bc013e4ad20ef405b504b1ca0
local     96f2fa7ba9c8131ae29e9e046d86d33c48ba9a14ad145b42837e281c0f8a0c00
local     356ef50e5c9ff24e07f8bcc1f9d581f41df05ce8688025f2294d2333fd86842b
local     d3c6bed2dc6b02bd7755c7e20a02a6679d5bc1bdc08e302adb0615f6d68b3b3a
local     html
[root@localhost ~]#
```

3、查看html数据卷的所在目录

```sh
[root@localhost ~]# docker inspect html
[
    {
        "CreatedAt": "2023-10-08T20:59:39-04:00",
        "Driver": "local",
        "Labels": null,
        # 存储目录，也就是挂载点
        "Mountpoint": "/var/lib/docker/volumes/html/_data",
        "Name": "html",
        "Options": null,
        "Scope": "local"
    }
]
[root@localhost ~]#
```

4、使用prune移除本地未使用的数据卷

```sh
[root@localhost ~]# docker volume prune
WARNING! This will remove anonymous local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Deleted Volumes:
2c26106500b13248f06d9d3d89e3f9cc4395f02bc013e4ad20ef405b504b1ca0
96f2fa7ba9c8131ae29e9e046d86d33c48ba9a14ad145b42837e281c0f8a0c00
d3c6bed2dc6b02bd7755c7e20a02a6679d5bc1bdc08e302adb0615f6d68b3b3a

Total reclaimed space: 0B
[root@localhost ~]#
```

5、使用docker volume ls列出所有的数据卷

```sh
[root@localhost ~]# docker volume ls
DRIVER    VOLUME NAME
local     356ef50e5c9ff24e07f8bcc1f9d581f41df05ce8688025f2294d2333fd86842b
local     html
[root@localhost ~]#
```

少了一些数据卷那些被移除了 

6、使用rm删除一个指定的数据卷，如果无法删除可以加上参数 -f 强制删除

```sh
[root@localhost ~]# docker volume rm html
html
[root@localhost ~]# docker volume ls
DRIVER    VOLUME NAME
local     356ef50e5c9ff24e07f8bcc1f9d581f41df05ce8688025f2294d2333fd86842b
[root@localhost ~]#
```

>  数据卷的作用：
>
>  -  j将容器与数据分离，解耦合，方便操作容器内数据，保证数据安全
>
>  数据卷操作：
>
>  -  `docker volume create [名称]` 创建一个数据卷
>  -  `docker volume ls` 列出所有存在的数据卷
>  -  `docker volume inspect [名称]` 查看指定数据卷的详细信息或所在目录
>  -  `docker volume rm [名称]` 删除指定的数据卷，可以删除一个或多个，可以强制删除
>  -  `docker volume prune` 移除未使用的数据卷

##### 7.5.7.4、挂载数据卷:palm_tree: 

我们在创建容器时，可以通过 `-v` 参数来挂载一个数据卷到某个容器目录

![image-20231009091445924](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310090914664.png)

这里面唯一没见过的就是 `-v` 参数了，解释如下：

前半部分的html为数据卷：后半部分为容器内的目录。

这行命令的意思就是将名为html的数据卷挂载到容器内的 /root/html目录下

###### 7.5.7.4.1、案例，创建一个Nginx容器，修改容器内的html目录内的index.html内容:tanabata_tree: 

>  **需求说明**：
>
>  上个案例中，我们进入nginx容器内部，已经知道nginx的html目录所在位置 /user/share/nginx/html ，我们需要把这个目录挂载到html这个数据卷上，方便操作其中的内容。

**提示**：运行容器时使用 `-v` 参数挂载数据卷

**步骤**：

<font color='red'>提醒</font>：如果数据卷不存在在创建容器的时候里面`-v`参数执行数据卷就会自动被创建

1、创建容器并挂载数据卷到容器内的HTML目录

```sh
[root@localhost ~]# docker run --name mn -p 80:80 -v html:/usr/share/nginx/html -d nginx
a4e6d36a4c1ccb8948d93990839244207cb1db9856a607680350871c13fc7458
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS                               NAMES
a4e6d36a4c1c   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second   0.0.0.0:80->80/tcp, :::80->80/tcp   mn
[root@localhost ~]#
```

2、进入html数据卷所在位置，并修改HTML内容

2.1、首先看一下html数据卷的具体位置

2.2、进入到目录

```sh
[root@localhost ~]# docker volume inspect html
[
    {
        "CreatedAt": "2023-10-08T21:26:51-04:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/html/_data",
        "Name": "html",
        "Options": null,
        "Scope": "local"
    }
]
[root@localhost ~]# cd /var/lib/docker/volumes/html/_data
[root@localhost _data]# 
[root@localhost _data]# ls -ahl
total 8.0K
drwxr-xr-x. 2 root root  40 Oct  9 02:54 .
drwx-----x. 3 root root  19 Oct  8 21:26 ..
-rw-r--r--. 1 root root 497 Aug 15 13:03 50x.html
-rw-r--r--. 1 root root 615 Aug 15 13:03 index.html
[root@localhost _data]#
```

![image-20231009145716659](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091459700.png)

我们可以右键index.html直接在windows上进行修改然后保存那么nginx容器内的index.html也就被修改了

改完保存后这个工具会提示是否保存到Linux系统中index文件去还是很智能的

![image-20231009150527902](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091505065.png)

下面就是修改了内容成功了，但是乱码

![image-20231009150556316](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091505224.png)

>  **总结**：
>
>  数据卷挂载方式：
>
>  -  `-v volumeName: /targetContainerPath`
>  -  如果容器运行时volume不存在，会自动被创建出来

通过案例演示宿主机直接与容器进行挂载

###### 7.5.7.4.2、宿主机直接与容器进行挂载:tanabata_tree: 

案例，创建并运行一个MYSQL容器，将宿主机目录直接挂载到容器

提示：目录挂载与数据卷挂载的语法是类似的：

-  `-v [宿主机目录]:[容器目录]`
-  `-v [宿主机文件]:[容器内文件]`

实现思路如下：

1、在将课前资料中的mysql.tar文件上传到虚拟机，通过load命令加载为镜像

操作：直接拖进去即可

![image-20231009153911060](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091539535.png)

```sh
[root@localhost tmp]# docker load -i mysql.tar
5dacd731af1b: Loading layer [==================================================>]  58.45MB/58.45MB
f411d8bde01c: Loading layer [==================================================>]  338.4kB/338.4kB
0aa7d65147ef: Loading layer [==================================================>]  10.44MB/10.44MB
3437f67a712b: Loading layer [==================================================>]  4.472MB/4.472MB
ec41e34b35a0: Loading layer [==================================================>]  1.536kB/1.536kB
458d25c646d8: Loading layer [==================================================>]  46.15MB/46.15MB
97874ea0e7f9: Loading layer [==================================================>]  31.74kB/31.74kB
5075b9328698: Loading layer [==================================================>]  3.584kB/3.584kB
364557e875f1: Loading layer [==================================================>]  257.3MB/257.3MB
9209148debed: Loading layer [==================================================>]  9.728kB/9.728kB
82582edf9553: Loading layer [==================================================>]  1.536kB/1.536kB
Loaded image: mysql:5.7.25
[root@localhost tmp]#
[root@localhost tmp]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
nginx        latest    61395b4c586d   2 weeks ago   187MB
redis        latest    7c4b517da47d   4 weeks ago   153MB
mysql        5.7.25    98455b9624a9   4 years ago   372MB
[root@localhost tmp]#
```



2、创建目录/tem/mysql/data

```sh
[root@localhost tmp]# mkdir -p mysql/data
[root@localhost tmp]# mkdir -p mysql/conf
```

3、创建目录/tmp/mysql/conf，将课前资料提供的hmy.cnf文件上传到/tmp/mysql/conf

hmy.conf配置文件内容：

```
[mysqld]
skip-name-resolve
character_set_server=utf8
datadir=/var/lib/mysql
server-id=1000
```

![image-20231009154459349](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091545686.png)

4、去DockerHub查阅资料，创建并运行MySQL容器

```sh
docker run \
--name some-mysql \
# 环境变量 environment variables 设置mysql密码
-e MYSQL_ROOT_PASSWORD=123 \
# 设置宿主机与容器的端口都为3306
-p 3306:3306
# 挂载配置文件目录，前面是宿主机目录后面是容器内的配置文件目录可以去官网查看
-v /tmp/mysql/conf/hmy.cnf:/etc/mysql/conf.d/hmy.cnf \
# 挂载数据目录
-v /tmp/mysql/data:/var/lib/mysql \
-d mysql:5.7.25
```

可复制如下：

```sh
docker run \
--name some-mysql \
-e MYSQL_ROOT_PASSWORD=123 \
-p 3306:3306 \
-v /tmp/mysql/conf/hmy.cnf:/etc/mysql/conf.d/hmy.cnf \
-v /tmp/mysql/data:/var/lib/mysql \
-d mysql:5.7.25
```



my.conf就是mysql 的默认配置文件了但是并不建议去覆盖它，它里面东西很多，我们只是希望要简化的配置 下面文档说，my.conf配置文件里包含了两个目录一个是/etc/mysql/conf.d或/etc/mysql/mysql/conf.d其中.d后缀就是目录的意思，因此放到这两个包含的目录里面的一切文件都会被加载到一起作为合并配置

![image-20231009160248603](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091602833.png)

要求：

1.  挂载/tmp/myql/data到mysql容器内数据存储到目录
2.  挂载/tmp/myql/conf/hmy/cnf到mysql容器的配置文件
3.  设置MySQL密码

打开链接mysql的工具进行连接，连接的是Linux中的mysql

连接密码是上面创建容器的时候的密码也就是123

![image-20231009164005163](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091640299.png)

连接成功！

![image-20231009164040831](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091640185.png)

查看宿主机的data目录可以看到里面的mysql数据就全都写进来了

```sh
➜  /tmp cd mysql/data
➜  data ls
auto.cnf    ca.pem           client-key.pem  ibdata1      ib_logfile1  mysql               private_key.pem  server-cert.pem  sys
ca-key.pem  client-cert.pem  ib_buffer_pool  ib_logfile0  ibtmp1       performance_schema  public_key.pem   server-key.pem
➜  data
```

##### 7.5.7.5、数据卷挂载和目录直接挂载的区别:palm_tree: 

当我们用数据卷时，Docker会自动帮我们创建数据卷对应的目录，这样数据卷就指向了宿主机的html目录。而Docker挂载时只需要挂载到数据卷的html目录上就可以了。它不需要关系宿主机的html目录在哪里，这种方式等于全权交给了Docker去处理

这是数据卷挂载的优势，但是劣势呢？

劣势就是，目录不是我们创建的，到哪创建了我们也不知道目录结构也比较深想去找比较麻烦

![image-20231009165142878](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091651091.png)

而第二种方式直接挂载，宿主机目录是我们自己创建的，创建目录的时候挺麻烦的但是自己知道这个目录在哪，将来挂载的时候也没有人帮我做什么代理。我直接挂上去就行了。因此我想快速定位这个文件在哪就可以一目了然，这就是直接挂载的优势，劣势就是自己挂载

![image-20231009165439938](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091654555.png)

>  **总结**:
>
>  1.  docker run 的命令通过 -v 参数挂载文件或目录到容器中：
>      1.  `-v volume [名称]` ：容器内目录
>      2.  `-v`宿主机文件:容器内文件
>      3.  `-v`宿主机目录:容器内目录
>  2.  数据卷挂载与目录直接挂载的区别：
>      1.  数据卷挂载耦合度低，由docker来管理目录，但是目录较深，不好找
>      2.  目录直接挂载耦合度高，需要我们自己管理目录，不过目录容易找到

### 7.6、Dockerfile自定义镜像:deciduous_tree: 

#### 自定义镜像

##### 7.6.1、镜像结构:palm_tree: 

-  镜像是将应用程序及其需要的系统函数库，环境，配置，依赖打包而成。

![image-20231009171118975](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091711487.png)

如果要制作一个镜像，就一定要先了解它内部的构成

上面说到镜像就是系统函数库，环境，配置，依赖组成的，而且它们之间有相互之间的依赖关系或者顺序。镜像不仅仅是把一对东西组合在一起而且还要按照一定顺序去翻层

下面以mysql为例讲讲镜像结构，我们把mysql结构打散

下图中mysql镜像结构分成了n层，它就是按照上面的依赖顺序来分层的，要想构建一个镜像，最底层一定是它所在的系统函数库，而最下面就是用了Ubuntu操作系统，把mysql安装包拷贝下来然后在此基于上使用rpm进行安装mysql，安装完了进行配置文件的配置等等，所有的安装配置完流程就完成的差不多了，还差一个入口 Entrypoint 通过这个入口启动程序的，这就是镜像的构成流程

![image-20231009172404559](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091724288.png)

>  **总结**：
>
>  镜像是分层结构，每一层称为一个Layer
>
>  -  BaseImage层：包含基本的系统函数库，环境变量，文件系统
>  -  Entrypoint：入口，是镜像中应用启动的命令
>  -  其它：在BaseImage基础上添加依赖，安装程序，完成整个应用的安装和配置

### 7.6.2、什么是Dockerfile:evergreen_tree: 

Dockerfile就是一个文本文件，其中包含一个个的指令(Instruction) ，用指令来说明要执行什么操作来构建镜像。每一个指令都会形成一层Layer

| 指令      | 说明                                         | 示例                      |
| --------- | -------------------------------------------- | ------------------------- |
| FROM      | 指定基础镜像                                 | FROM centos:6             |
| ENV       | 设置环境变量，可在后面指令使用               | ENV key value             |
| COPY      | 拷贝本地文件到镜像的指定目录                 | COPY ./mysql-5.7.rpm /tmp |
| RUN       | 执行Linux的shell命令，一般是安装过程的命令   | RUN yum install gcc       |
| EXPOSE    | 指定容器运行时监听的端口，是给镜像使用者看的 | EXPOSE 8080               |
| ENTPYOINT | 镜像中应用的启动命令，容器运行时调用         | ENTPYPOINT java -jar x    |

更新详细语法说明，请参考官网文档：https://docs.docker.com/engine/reference/builder

### 7.6.3、案例，基于Ubuntu镜像构建一个新镜像，运行一个java项目:evergreen_tree: 

步骤：

1、新建一个空文件夹 docker-demo

```sh
➜  /tmp mkdir docker-demo
➜  /tmp cd docker-demo
➜  docker-demo l
total 0
drwxrwxr-x.  2 dkx  dkx    6 Oct  9 05:52 .
drwxrwxrwt. 11 root root 228 Oct  9 05:52 ..
➜  docker-demo
```

2、拷贝docker-demo.jar文件到docker-demo这个目录中

![image-20231009175606996](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091756545.png)

3、拷贝jdk8.tar.gz文件到docker-demo这个目录

![image-20231009180127529](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091801025.png)

4、拷贝Dockerfile到docker-demo目录

Dockerfile内容

```sh
# 指定基础镜像
FROM ubuntu:16.04
# 配置环境变量，JDK的安装目录
ENV JAVA_DIR=/usr/local

# 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar

# 安装JDK
RUN cd $JAVA_DIR \
 && tar -xf ./jdk8.tar.gz \
 && mv ./jdk1.8.0_144 ./java8

# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin

# 暴露端口
EXPOSE 8090
# 入口，java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar
```

![image-20231009180246866](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091802428.png)

```sh
➜  docker-demo ll
total 202M
-rw-r--r--. 1 dkx dkx  25M Oct  9 06:02 docker-demo.jar
-rw-r--r--. 1 dkx dkx  494 Oct  9 06:02 Dockerfile
-rw-r--r--. 1 dkx dkx 177M Oct  9 06:02 jdk8.tar.gz
➜  docker-demo
```

6、在docker-demo目录中运行命令：`docker build -t javaweb:1.0 .`

```sh
➜  docker-demo sudo docker build -t javaweb:1.0 .
[sudo] password for dkx:
[+] Building 46.3s (9/9) FINISHED                                                                                                          docker:default
 => [internal] load build definition from Dockerfile                                                                                                 0.0s
 => => transferring dockerfile: 591B                                                                                                                 0.0s
 => [internal] load .dockerignore                                                                                                                    0.0s
 => => transferring context: 2B                                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/ubuntu:16.04                                                                                      5.6s
 => [1/4] FROM docker.io/library/ubuntu:16.04@sha256:1f1a2d56de1d604801a9671f301190704c25d604a416f59e03c04f5c6ffee0d6                               30.9s
 => => resolve docker.io/library/ubuntu:16.04@sha256:1f1a2d56de1d604801a9671f301190704c25d604a416f59e03c04f5c6ffee0d6                                0.0s
 => => sha256:1f1a2d56de1d604801a9671f301190704c25d604a416f59e03c04f5c6ffee0d6 1.42kB / 1.42kB                                                       0.0s
 => => sha256:a3785f78ab8547ae2710c89e627783cfa7ee7824d3468cae6835c9f4eae23ff7 1.15kB / 1.15kB                                                       0.0s
 => => sha256:b6f50765242581c887ff1acc2511fa2d885c52d8fb3ac8c4bba131fd86567f2e 3.36kB / 3.36kB                                                       0.0s
 => => sha256:58690f9b18fca6469a14da4e212c96849469f9b1be6661d2342a4bf01774aa50 46.50MB / 46.50MB                                                    27.2s
 => => sha256:b51569e7c50720acf6860327847fe342a1afbe148d24c529fb81df105e3eed01 857B / 857B                                                           0.9s
 => => sha256:da8ef40b9ecabc2679fe2419957220c0272a965c5cf7e0269fa1aeeb8c56f2e1 528B / 528B                                                           0.8s
 => => sha256:fb15d46c38dcd1ea0b1990006c3366ecd10c79d374f341687eb2cb23a2c8672e 170B / 170B                                                           1.5s
 => => extracting sha256:58690f9b18fca6469a14da4e212c96849469f9b1be6661d2342a4bf01774aa50                                                            3.5s
 => => extracting sha256:b51569e7c50720acf6860327847fe342a1afbe148d24c529fb81df105e3eed01                                                            0.0s
 => => extracting sha256:da8ef40b9ecabc2679fe2419957220c0272a965c5cf7e0269fa1aeeb8c56f2e1                                                            0.0s
 => => extracting sha256:fb15d46c38dcd1ea0b1990006c3366ecd10c79d374f341687eb2cb23a2c8672e                                                            0.0s
 => [internal] load build context                                                                                                                    1.6s
 => => transferring context: 211.19MB                                                                                                                1.6s
 => [2/4] COPY ./jdk8.tar.gz /usr/local/                                                                                                             0.7s
 => [3/4] COPY ./docker-demo.jar /tmp/app.jar                                                                                                        0.1s
 => [4/4] RUN cd /usr/local  && tar -xf ./jdk8.tar.gz  && mv ./jdk1.8.0_144 ./java8                                                                  4.8s
 => exporting to image                                                                                                                               4.0s
 => => exporting layers                                                                                                                              3.9s
 => => writing image sha256:7163e4e0d2b830e1f411e6d50c40cb258e0110888af48e8ddce9642f72f564c1                                                         0.0s
 => => naming to docker.io/library/javaweb:1.0                                                                                                       0.0s
➜  docker-demo
```

可以看到镜像构建好了

```sh
[root@localhost ~]# docker images
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
javaweb      1.0       7163e4e0d2b8   About a minute ago   722MB
nginx        latest    61395b4c586d   2 weeks ago          187MB
redis        latest    7c4b517da47d   4 weeks ago          153MB
mysql        5.7.25    98455b9624a9   4 years ago          372MB
[root@localhost ~]#
```

创建容器并运行

```sh
[root@localhost ~]# docker run --name web -p 8090:8090 -d javaweb:1.0
de4fc104d20fd0c7a44fe227aa07bbba8fb5fd6c7ca04400e7b90c93375573a5
[root@localhost ~]# docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                                  NAMES
de4fc104d20f   javaweb:1.0    "/bin/sh -c 'java -j…"   4 seconds ago   Up 2 seconds   0.0.0.0:8090->8090/tcp, :::8090->8090/tcp              web
10d26568836c   mysql:5.7.25   "docker-entrypoint.s…"   2 hours ago     Up 2 hours     0.0.0.0:3306->3306/tcp, :::3306->3306/tcp, 33060/tcp   some-mysql
2ba6c6cf79f4   nginx          "/docker-entrypoint.…"   4 hours ago     Up 4 hours     0.0.0.0:80->80/tcp, :::80->80/tcp                      mn
[root@localhost ~]#
```

访问页面uri：http://192.168.249.128:8090/hello/count

访问成功

![image-20231009190718545](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091907846.png)

<font color='red'>问题</font>：

如果每个微服务构建都要进行下面的流程会很麻烦

```sh
# 指定基础镜像
FROM ubuntu:16.04
# 配置环境变量，JDK的安装目录
ENV JAVA_DIR=/usr/local

# 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar

# 安装JDK
RUN cd $JAVA_DIR \
 && tar -xf ./jdk8.tar.gz \
 && mv ./jdk1.8.0_144 ./java8

# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin

# 暴露端口
EXPOSE 8090
# 入口，java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar
```

从配置环境前n层的内容不会变，我们可以对前n层提前构建好做一个镜像。以后就都在这个构建好的基础上去构建镜像是不是就方便多了。

但实际上已经有人帮我们做好了这件事儿了。

就是如下：

### 7.6.4、基于java:8-alpine镜像，将一个java项目构建为镜像:evergreen_tree: 

**实现思路如下**：

1、新建一个空的目录，然后在目录中新建一个文件，命名为Dockerfile

2、拷贝docker-demo.jar到这个目录中

3、编写Dockerfile文件：

1.  基于java:8-alpine作为基础镜像
2.  将app.jar拷贝到镜像中
3.  暴漏端口
4.  编写入口ENTRYPOINT

4、使用docker build命令构建镜像

5、使用docker run创建容器并运行

```sh
# 指定基础镜像
FROM java:8-alpine

# 暴露端口
EXPOSE 8090
# 入口，java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar
```

直接节省了大量的代码工作

修改完成保存后，覆盖Linux，/tmp/docker-demo中的Dockerfile文件

覆盖成功

![image-20231009192320991](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091923461.png)

使用cat命令查看下修改情况

```sh
[root@localhost ~]# cat /tmp/docker-demo/Dockerfile
# 指定基础镜像
FROM java:8-alpine

# 暴露端口
EXPOSE 8090
# 入口，java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar[root@localhost ~]#
```

再次进行构建镜像

如果构建遇到报错如下：

```sh
➜  docker-demo sudo docker build -t javaweb:1.0 .
[+] Building 2.7s (3/3) FINISHED                                                                                                           docker:default
 => [internal] load .dockerignore                                                                                                                    0.0s
 => => transferring context: 2B                                                                                                                      0.0s
 => [internal] load build definition from Dockerfile                                                                                                 0.0s
 => => transferring dockerfile: 245B                                                                                                                 0.0s
 => ERROR [internal] load metadata for docker.io/library/openjava:8-alpine                                                                           2.6s
------
 > [internal] load metadata for docker.io/library/openjava:8-alpine:
------
Dockerfile:2
--------------------
   1 |     # 指定基础镜像
   2 | >>> FROM openjava:8-alpine
   3 |
   4 |     # 暴露端口
--------------------
ERROR: failed to solve: openjava:8-alpine: pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
```

则需要将Dockerfile文件中的java:8-alpine改为openjdk:8-alpine

```sh
# 指定基础镜像
FROM openjdk:8-alpine

# 暴露端口
EXPOSE 8090
# 入口，java项目的启动命令
ENTRYPOINT java -jar /tmp/app.jar
```

再次进行构建镜像

```sh
➜  docker-demo sudo docker build -t javaweb:1.0 .
[+] Building 89.8s (5/5) FINISHED                                                                                                          docker:default
 => [internal] load .dockerignore                                                                                                                    0.0s
 => => transferring context: 2B                                                                                                                      0.0s
 => [internal] load build definition from Dockerfile                                                                                                 0.0s
 => => transferring dockerfile: 244B                                                                                                                 0.0s
 => [internal] load metadata for docker.io/library/openjdk:8-alpine                                                                                  6.1s
 => [1/1] FROM docker.io/library/openjdk:8-alpine@sha256:94792824df2df33402f201713f932b58cb9de94a0cd524164a0f2283343547b3                           83.6s
 => => resolve docker.io/library/openjdk:8-alpine@sha256:94792824df2df33402f201713f932b58cb9de94a0cd524164a0f2283343547b3                            0.0s
 => => sha256:a3562aa0b991a80cfe8172847c8be6dbf6e46340b759c2b782f8b8be45342717 3.40kB / 3.40kB                                                       0.0s
 => => sha256:e7c96db7181be991f19a9fb6975cdbbd73c65f4a2681348e63a141a2192a5f10 2.76MB / 2.76MB                                                      10.9s
 => => sha256:f910a506b6cb1dbec766725d70356f695ae2bf2bea6224dbe8c7c6ad4f3664a2 238B / 238B                                                           0.8s
 => => sha256:c2274a1a0e2786ee9101b08f76111f9ab8019e368dce1e325d3c284a0ca33397 70.73MB / 70.73MB                                                    80.6s
 => => sha256:94792824df2df33402f201713f932b58cb9de94a0cd524164a0f2283343547b3 1.64kB / 1.64kB                                                       0.0s
 => => sha256:44b3cea369c947527e266275cee85c71a81f20fc5076f6ebb5a13f19015dce71 947B / 947B                                                           0.0s
 => => extracting sha256:e7c96db7181be991f19a9fb6975cdbbd73c65f4a2681348e63a141a2192a5f10                                                            0.2s
 => => extracting sha256:f910a506b6cb1dbec766725d70356f695ae2bf2bea6224dbe8c7c6ad4f3664a2                                                            0.0s
 => => extracting sha256:c2274a1a0e2786ee9101b08f76111f9ab8019e368dce1e325d3c284a0ca33397                                                            2.8s
 => exporting to image                                                                                                                               0.0s
 => => exporting layers                                                                                                                              0.0s
 => => writing image sha256:d92b26b799ba6c537e486f7619b96f16ae44e8d4fb296ab05cd80e9d764f5b30                                                         0.0s
 => => naming to docker.io/library/javaweb:1.0                                                                                                       0.0s
➜  docker-demo
```

构建完成!

再次进行访问页面uri：http://192.168.249.128:8090/hello/count

![image-20231009194918244](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310091951847.png)

>  总结:
>
>  1.  Dockerfile的本质是一个文件，通过指令描述镜像的构建过程
>  2.  Dockerfile的第一行必须是FROM，从一个基础镜像来构建
>  3.  基础镜像可以是基本操作系统，如Ubuntu。也可以是其它人制作好的镜像，例如：java:8-alpine亦或者openjdk:8-alpine

### 7.6.5、DockerCompose:evergreen_tree: 

-  初始DockerCompose
-  部署微服务集群

实际生产环境中微服务很多很多，数十上百甚至上千个微服务我们如果一个一个去构建这可得累死了。所以我们需要有一种集群部署的手段，那么这就是我们要学习的DockerCompose了

#### 7.6.5.1、什么是DockerCompose:palm_tree: 

-  DockerCompose可以基于Compose文件帮我们快速的部署分布式应用，而无需手动一个一个创建和运行容器！
-  Compose文件是一个文本文件， 通过指令定义集群中的每个容器如果运行。
-  Compose文件是把Docker run命令集合进来的然后把语法转换了一下

DockerCompose文件就是把Docker run的各种参数转换成指令去定义了。

![image-20231009201930379](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310092019582.png)

-  DockerCompose的详细语法参考官网：https://docs.docker.com/compose/compose-file/

上面了解后我们就进行安装DockerCompose：[查看安装文章](./Docker/Centos7安装Docker.md).

>  **总结**：
>
>  DockerCompose有什么用？
>
>  -  帮助我们快速部署分布式应用，无需一个一个微服务去构建镜像和部署

#### 7.6.5.2、DockerCompose部署:palm_tree: 

**案例，将之前学习的cloud-demo微服务集群利用DockerCompose部署**.

实现思路如下：

1、创建与cloud-demo项目目录相同的几个文件夹

三个微服务需要依赖于mysql所以需要准备mysql的数据

![image-20231009204357169](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310092043951.png)

2、每个文件夹里面存放Dockerfile文件，内容如下：

```sh
# 基础镜像
FROM java:8-alpine
# 拷贝当前目录中的app.jar到tem目录中，将运行的jar包拷贝到镜像中
COPY ./app.jar /tmp/app.jar
# 启动脚本
ENTRYPOINT java -jar /tmp/app.jar
```

除了mysql的目录中再创建两个目录

![image-20231009204740200](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310092047841.png)

conf目录中存放mysql配置文件

![image-20231009204801023](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310092048636.png)

data目录里面存放数据库的数据

![image-20231009204831646](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310092048062.png)

3、最外层创建一个docker-compose.yml文件

![image-20231009204929278](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310092049638.png)

内容为：

```yml
version: "3.2"

services:
  nacos:
    image: nacos/nacos-server
    environment: # 环境配置，启动参数
      MODE: standalone
    ports:
      - "8848:8848"
  mysql:
    image: mysql:5.7.25
    environment:
      MYSQL_ROOT_PASSWORD: 123
    # 数据卷挂载
    volumes: # 使用Linux指令 pwd获取当前目录位置然后从这个位置找对应的目录，不写硬编码
      - "$PWD/mysql/data:/var/lib/mysql"
      - "$PWD/mysql/conf:/etc/mysql/conf.d/"
  userservice:
  # 服务构建基于dockerfile完成构建，在当前目录下的user-service目录下找到dockerfile进行构建镜像
    build: ./user-service
  # 服务构建基于dockerfile完成构建，在当前目录下的order-service目录下找到dockerfile进行构建镜像
  orderservice:
    build: ./order-service
  gateway:
  # 网关构建基于dockerfile完成构建，在当前目录下的gateway目录下找到dockerfile进行构建镜像
    build: ./gateway
    # 只有网关暴漏了端口，剩下上面的微服务都没有暴漏，因为网关才是入口而不应该将微服务暴漏出去
    ports:
      - "10010:10010"
```

将来DockerCompose就会帮我们自动的去构建这三个微服务的镜像了，实现一次性部署

4、修改自己的cloud-demo项目，将数据库，nacos地址都命名为docker-compose中的服务名

不能把nacos，数据库之类的配置地址写死，因为微服务可能配置在不同的机器上

>  用DockerCompose部署时所有的服务之间都可以用服务名称去互相访问，这是DockerCompose底层做了一些配置

user-service中的nacos配置

```yml
spring:
  application:
    name: userservice # 服务名称
  profiles:
    active: dev # 环境
  cloud:
    nacos:
      server-addr: nacos:8848 # nacos 地址
      discovery:
        # cluster-name: TJ
      config:
        file-extension: yaml # 配置文件后缀名
# 三要素：服务名称，环境，后缀名 而这三个决定了DateId
```

user-service中的mysql配置

```yml
spring:
  datasource:
    url: jdbc:mysql://mysql:3306/cloud_user?useSSL=false&characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: dkx.
    driver-class-name: com.mysql.cj.jdbc.Driver
```

同样将order-service中的mysql和nacos地址配置改过来

```yml
spring:
  datasource:
    url: jdbc:mysql://mysql:3306/cloud_order?useSSL=false&characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: dkx.
    driver-class-name: com.mysql.cj.jdbc.Driver
  application:
    name: orderservice
  cloud:
    nacos:
      server-addr: nacos:8848
      discovery:
        # cluster-name: HZ # 集群名称
        # namespace: ed32fa6f-e9e3-4483-b860-7bcd9731d0d9 # 命名空间，填 ID dev环境
        ephemeral: true # 设置是否为临时实例 设置为非临时实例
```

修改网关服务的nacos地址配置

```yml
#===========这些配置让网关能够联系上nacos实现服务注册和发现====
server:                                              #||
  port: 10010 # 网关端口                              #||
spring:                                              #||
  application:                                       #||
    name: gateway # 服务名称                           #||
  cloud:                                              #||
    nacos:                                            #||
      server-addr: nacos:8848 # nacos地址          #||
#========================================================
```

5、使用maven打包工具，将项目中的每个微服务都打包为app.jar文件 

如果要让项目打包为app.jar名称则需要在pom.xml文件中配置如下：

```xml
<build>
   <finalName>app</finalName>
</build>
```

我们在所有需要打包的服务中的pom文件里面进行配置

先进行一下clean清空一个然后再进行install安装本地

![image-20231009211435849](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310092114297.png)

6、将打包好的app.jar拷贝到cloud-demo中的每一个对应的子目录中

![image-20231009212348796](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310092123149.png)

7、将cloud-demo上传至虚拟机，利用docker-compose up -d 来部署

参数说明：up - 创建并执行容器

​			 -d - 后台运行

其它参数help参见

![image-20231009212636444](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310092126664.png)

```sh
[root@localhost tmp]# cd cloud-demo/
[root@localhost cloud-demo]# ls
docker-compose.yml  gateway  mysql  order-service  user-service
[root@localhost cloud-demo]#
```

使用命令进行部署

```sh
[root@localhost cloud-demo]# docker-compose up -d
Building userservice
[+] Building 3.5s (7/7) FINISHED                                                                                                           docker:default
 => [internal] load build definition from Dockerfile                                                                                                 0.0s
 => => transferring dockerfile: 182B                                                                                                                 0.0s
 => [internal] load .dockerignore                                                                                                                    0.0s
 => => transferring context: 2B                                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/openjdk:8-alpine                                                                                  2.7s
 => [internal] load build context                                                                                                                    0.3s
 => => transferring context: 39.30MB                                                                                                                 0.3s
 => CACHED [1/2] FROM docker.io/library/openjdk:8-alpine@sha256:94792824df2df33402f201713f932b58cb9de94a0cd524164a0f2283343547b3                     0.0s
 => [2/2] COPY ./app.jar /tmp/app.jar                                                                                                                0.2s
 => exporting to image                                                                                                                               0.3s
 => => exporting layers                                                                                                                              0.2s
 => => writing image sha256:ab22b9b33217d7b1e7287dc7b16564ba7a683bcc390e961ffb7d6f77069ea697                                                         0.0s
 => => naming to docker.io/library/cloud-demo_userservice                                                                                            0.0s
WARNING: Image for service userservice was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Building orderservice
[+] Building 1.4s (7/7) FINISHED                                                                                                           docker:default
 => [internal] load build definition from Dockerfile                                                                                                 0.0s
 => => transferring dockerfile: 182B                                                                                                                 0.0s
 => [internal] load .dockerignore                                                                                                                    0.0s
 => => transferring context: 2B                                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/openjdk:8-alpine                                                                                  0.6s
 => [internal] load build context                                                                                                                    0.3s
 => => transferring context: 41.82MB                                                                                                                 0.3s
 => CACHED [1/2] FROM docker.io/library/openjdk:8-alpine@sha256:94792824df2df33402f201713f932b58cb9de94a0cd524164a0f2283343547b3                     0.0s
 => [2/2] COPY ./app.jar /tmp/app.jar                                                                                                                0.1s
 => exporting to image                                                                                                                               0.2s
 => => exporting layers                                                                                                                              0.2s
 => => writing image sha256:4fb619347bfb2d9aac0e824d4334973c18761b4ee64c1f9fddbd3b6041b8fad1                                                         0.0s
 => => naming to docker.io/library/cloud-demo_orderservice                                                                                           0.0s
WARNING: Image for service orderservice was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Building gateway
[+] Building 1.2s (7/7) FINISHED                                                                                                           docker:default
 => [internal] load build definition from Dockerfile                                                                                                 0.0s
 => => transferring dockerfile: 182B                                                                                                                 0.0s
 => [internal] load .dockerignore                                                                                                                    0.0s
 => => transferring context: 2B                                                                                                                      0.0s
 => [internal] load metadata for docker.io/library/openjdk:8-alpine                                                                                  0.6s
 => [internal] load build context                                                                                                                    0.3s
 => => transferring context: 39.92MB                                                                                                                 0.3s
 => CACHED [1/2] FROM docker.io/library/openjdk:8-alpine@sha256:94792824df2df33402f201713f932b58cb9de94a0cd524164a0f2283343547b3                     0.0s
 => [2/2] COPY ./app.jar /tmp/app.jar                                                                                                                0.1s
 => exporting to image                                                                                                                               0.2s
 => => exporting layers                                                                                                                              0.2s
 => => writing image sha256:53993c010033dc8356f48ddd183ad1768544070ceb7e803ada30d20e9b3ae316                                                         0.0s
 => => naming to docker.io/library/cloud-demo_gateway                                                                                                0.0s
WARNING: Image for service gateway was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating cloud-demo_nacos_1        ... done
Creating cloud-demo_userservice_1  ... done
Creating cloud-demo_mysql_1        ... done
Creating cloud-demo_gateway_1      ... done
Creating cloud-demo_orderservice_1 ... done
[root@localhost cloud-demo]#
```

部署成功使用命令查看启动日志：docker-compose logs -f

```sh
[root@localhost cloud-demo]# docker-compose logs -f
Attaching to cloud-demo_orderservice_1, cloud-demo_gateway_1, cloud-demo_userservice_1, cloud-demo_mysql_1, cloud-demo_nacos_1
gateway_1       |
gateway_1       |   .   ____          _            __ _ _
gateway_1       |  /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
gateway_1       | ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
gateway_1       |  \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
gateway_1       |   '  |____| .__|_| |_|_| |_\__, | / / / /
gateway_1       |  =========|_|==============|___/=/_/_/_/
gateway_1       |  :: Spring Boot ::        (v2.3.9.RELEASE)
gateway_1       |
gateway_1       | 10-09 13:37:38:143  INFO 1 --- [           main] cn.itcast.gateway.GatewayApplication     : No active profile set, falling back to default profiles: default
gateway_1       | 10-09 13:37:41:667  INFO 1 --- [           main] o.s.cloud.context.scope.GenericScope     : BeanFactory id=87ef38f0-5652-3427-832e-5397c91e63b6
....
....
```

启动时nacos会报错因为它启动的比较慢我们需要先启动nacos执行命令：`docker-compose up -d nacos`

等待nacos启动完成后可以访问uri测试一下：http://192.168.249.128:8848/nacos

![image-20231010102946333](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310101029911.png)



如果有报错信息可以使用命令重启服务`docker-compose restart gateway userservice orderservice`

```sh
[root@localhost cloud-demo]# docker-compose restart gateway userservice orderservice
Restarting cloud-demo_orderservice_1 ... done
Restarting cloud-demo_gateway_1      ... done
Restarting cloud-demo_userservice_1  ... done
[root@localhost cloud-demo]#
```

### 7.6.6、Docker镜像仓库:evergreen_tree: 

-  搭建私有镜像仓库
-  向镜像仓库推送镜像
-  从镜像仓库拉取镜像

#### 7.6.6.1、常见镜像仓库服务:palm_tree: 

镜像仓库 (Docker Registry) 有公共和私有的两种形式：

-  公共仓库：例如Docker官方的Docker Hub，国内也有一些服务商提供类似于Docker Hub的公开服务，比如 网易云镜像服务，DaoCloud镜像服务，阿里云镜像服务等。
-  除了使用公开仓库外，用户还可以在本地搭建私有Docker Registry。企业自己的镜像最好是采用私有Docker Registry来实现。

#### 7.6.6.2、创建私服镜像仓库查看文章:palm_tree: 

[查看Docker带有图形化镜像仓库](./Docker/Centos7安装Docker.md).

#### 7.6.6.3、在私有镜像仓库推送或拉取镜像:palm_tree: 

推送镜像到私有镜像服务必须先tag，步骤如下：

1、重新tag本地镜像，名称前缀为私有仓库的地址：192.168.150.101:8080/

```sh
docker tag nginx:latest 192.168.150.101:8080/nginx:1.0
```

使用docker images查看打包好的镜像

![image-20231010121530321](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310101215960.png)

2、推送镜像

```sh
docker push 192.168.150.101:8080/nginx:1.0
```

3、拉取镜像

```sh
docker pull 192.168.150.101:8080/nginx:1.0
```

>  **总结**：
>
>  1.  推送本地镜像到仓库前都必须重命名 (docker tag) 镜像，以镜像仓库地址为前缀
>  2.  镜像仓库推送前需要把仓库地址配置到docker服务的daemon.json文件中，被docker信任
>  3.  推送使用docker push命令
>  4.  拉取使用docker pull命令
