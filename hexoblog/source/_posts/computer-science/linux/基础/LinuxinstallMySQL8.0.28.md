---
title: CentOs安装MySQL8.0.28
categories: 
      - [计算机学科,linux,基础]
tags:
      - 计算机学科
      - linux
---

1.  去官网下载MySQL

2.  创建一个存放MySQL的目录

3.  在解压之前查看以下系统是否自带了mariadb数据库

    -  查看指令：

    ```bash
    rpm -qa|grep mariadb
    ```

    -  可能会列出两个出来，将都卸载掉输入指令：

    ```bash
    rpm -e --nodeps mariadb
    ```

4.  解压MySQL输入指令：

```bash
tar -zxvf [mysql]
```

5.  在当前解压MySQL的目录中输入顺序安装MySQL==注意：顺序很重要==

```bash
rpm -ivh mysql-community-common-8.0.28-1.el7.x86_64.rpm
```

```bash
rpm -ivh mysql-community-client-plugins-8.0.28-1.el7.x86_64.rpm
```

```bash
rpm -ivh mysql-community-libs-8.0.28-1.el7.x86_64.rpm
```

```bash
rpm -ivh mysql-community-client-8.0.28-1.el7.x86_64.rpm
```

```bash
rpm -ivh mysql-community-icu-data-files-8.0.28-1.el7.x86_64.rpm
```

```bash
rpm -ivh mysql-community-server-8.0.28-1.el7.x86_64.rpm
```

6.  安装完成后进行初始化MySQL

```bash
mysqld --initialize --console
```

7.  修改MySQL所有者与所属组[便以使用]

```bash
chown mysql:mysql -R /var/lib/mysql/
```

8.  启动MySQL服务

```bash
systemctl start mysqld
```

9.  MySQL会自动帮我们创建一个密码使用指令查看密码

```bash
cat /var/log/mysqld.log|grep localhost
# Enter后显示root@localhost:后面就是密码
[root@localhost opt]# cat /var/log/mysqld.log|grep localhost
2022-10-21T14:55:24.788588Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: AqFwbqjyd1;_
```

10.  输入上面MySQL自动创建的密码进入MySQL

```bash
mysql -u root -p
Enter passwd：AqFwbqjyd1;_ [最好复制粘贴因为容易出错]
```

11.  更改这个密码

```sql
alter user 'root'@'localhost' identified by 'dkx';
```

