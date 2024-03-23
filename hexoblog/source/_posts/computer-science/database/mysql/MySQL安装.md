---
title: Windows安装MySQL
categories:
    - [计算机学科,database,mysql]
tags:
    - mysql
    - databse
    - 计算机学科
---

## Windows安装MySQL

1. 首先找到MySQL的官网,点击下载后进入这个页面点击如下图所示

![](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031529779.png)

2. 有两个安装的路径它们的含义分别不同我们选择第二个离线安装

![image-20240103153114444](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031531541.png)

3. 这里我们直接点击下载

![image_2022-12-03-23-07-06](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031531098.png)

4. 安装完成后我们进入下一个阶段:安装与配置

    - 注意事项:

        1. 确保电脑的UserName(用户名)中不含有中文,如果有需要重命名更改

5. 右键安装包点击安装

![image_2022-12-03-23-13-07](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031531431.png)

6. 稍等片刻后就会出现这样的一个页面

![image_2022-12-03-23-14-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031531334.png)

- Developer Default:安装MySQL服务器以及开发MySQL所需的工具

- Server only:仅安装MySQL服务器

- Client only:仅安装客户端

- Full:安装MySQL所有可用组件

- Custom:(经典)自定义安装

7. 我们点击Custom进行自定义安装

![image_2022-12-03-23-18-32](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031534042.png)

可以看到它让我们选择所需的MySQL工具,但是我们怎么知道要怎么选择呢不着急这样操作

1. 点击back退回去,并选中第一个

![image_2022-12-03-23-20-10](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031531954.png)

2. 点击next

![image_2022-12-03-23-21-03](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031531061.png)

3. 再点击back退回

![image_2022-12-03-23-21-53](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031531888.png)

4. 选中Custom后点击Next

![image-20240103153205611](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031532709.png)

我们可以看到它既然帮我们自己选好了

![image_2022-12-03-23-23-00](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031531997.png)

进行下一步操作,点击右侧的工具进行添加路径

![image_2022-12-03-23-25-54](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031531352.png)

当然这两个路径需要自己提前创建好:D:\MySQL和D:\MySQLData

![image_2022-12-03-23-27-20](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031532910.png)

再进行下一个同样的操作

![image_2022-12-03-23-28-59](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031532202.png)

![image_2022-12-03-23-30-11](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533089.png)

![image_2022-12-03-23-31-55](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533608.png)

![image_2022-12-03-23-32-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031534933.png)

![image_2022-12-03-23-33-25](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533340.png)

![image_2022-12-03-23-34-27](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533439.png)

![image_2022-12-03-23-35-14](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031534929.png)

![image_2022-12-03-23-35-47](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533124.png)

![image_2022-12-03-23-36-09](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533800.png)

![image_2022-12-03-23-36-33](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533597.png)

![image_2022-12-03-23-36-58](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533473.png)

完成后点击Next,选择yes

![image_2022-12-03-23-40-10](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533394.png)

点击Execute

![image_2022-12-03-23-40-46](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533453.png)

完成后点击next

![image-20240103153445664](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031534750.png)

这里默认就行直接next

![image_2022-12-03-23-43-03](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031534789.png)

选择第二个

![image_2022-12-03-23-44-10](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533869.png)

设置管理员密码 一定要记住这个密码

![image_2022-12-03-23-46-58](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533750.png)

然后一路下一步就行了

这里输入刚才设置的密码之后点击check验证然后通过next

![image_2022-12-03-23-49-38](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533690.png)

安装成功后会弹出两个窗口我们关闭了,来查看下MySQL的目录

这里是工具套件目录

![image_2022-12-03-23-52-12](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031533603.png)

这里是我们安装的MySQL数据库目录

![image-20240103153521470](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031535512.png)

我们可以切换到D:\MySQL\MySQL Server8.0\bin 这个目录下执行以下cmd

![image_2022-12-03-23-58-16](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031534739.png)

在cmd中输入mysql -u root -p然后输出刚才设置的密码就可以进入mysql了

![image_2022-12-03-23-59-55](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031534708.png)

## 配置环境变量

1. 打开配置的页面,选择path

![image_2022-12-04-00-02-16](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031534905.png)

2. path中新建输入mysql的bin目录的路径

![image_2022-12-04-00-04-41](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401031534873.png)

大功告成!查看下是否可以在cmd任意打开的位置都可以进入mysql了可以那么就是成功了

