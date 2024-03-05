---
title: Scoop安装mysql
categories:
   - [tools,windows]
tags: 
   - tools
   - windows
   - mysql
---

## Scoop安装mysql

1.  添加仓库：`main` 

    ```sh
    scoop bucket add main
    ```

2.  执行命令安装mysql

    ```sh
    scoop install mysql
    ```

3.  启动mysql数据库

    ```sh
    mysqld --console
    ```

    1.  这里启动的方式分为两种

        1.  后台启动`--standalone` 
        2.  终端启动 `--console` 

    2.  如果报错则执行下面代码：

        ```sh
        mysqld --initialze --user=mysql --console
        ```

4.  执行命令：`mysqld -install` 安装服务

    1.  如果出现下面的情况

        ![image-20230904093551765](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309040935540.png)

    2.  则执行命令：`sc delete mysql` 

        ![image-20230904093618438](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309040936057.png)

    3.  再次执行：`mysqld -install` 

        ![image-20230904093638396](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309040936016.png)

5.  最后启动服务：`net start mysql`启动！

    ![image-20230904093707195](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309040937628.png)

最后一步可能会想，为什么不直接去任务管理器里面的服务去启动服务呢？因为这里有可能在任务管理器中启动不了，但是终端可以。

启动后你想执行一下sql命令比如：`show databses;` 

却报错了，不要慌它提示的是需要重置密码。

重置密码执行命令：`ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';` 然后你就可以正常的使用了。