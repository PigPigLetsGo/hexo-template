---
title: C语言链接Mysql
categories:
	- [计算机学科,C]
tags:
	- 计算机学科
	- C
	- mysql
---

查看安装好的mysql中是否存在这些文件

在mysql serverxx.xx中有一个include 和 lib文件目录里面包含了大量的.h头文件和lib，dll文件

![image-20230924161109494](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241611363.png)

![image-20230924161118596](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241611093.png)

确保有这些文件后，创建一个project

![image-20230924161159930](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241612164.png)

复制include和lib文件目录到创建好的这个project项目目录中，注意复制不是剪切 ，剪切就完蛋了。

![image-20230924161448672](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241614792.png)

然后将lib目录中的libmysql.dll文件单独的复制一份放外面

![image-20230924161535710](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241615418.png)

右键project点击属性

![image-20230924161226373](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241612862.png)

点击vc++ / 包含目录

![image-20230924161311066](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241613014.png)

点击包含目录的右边的一个小三角然后点击编辑

![image-20230924161708111](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241617933.png)

选择include文件目录然后确定应用

![image-20230924161744513](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241617248.png)

然后就是库目录同样点开编辑选择lib目录 ，注意这里选择lib目标不是include

![image-20230924161921025](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241619308.png)

点击应用后点击 链接器 -> 输入 -> 附加依赖项 点击编辑 添加 libmysql.lib文件的名字

![image-20230924162130072](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241621695.png)

简单的向数据库中添加一条数据：

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>
#include <winsock.h>
#include <mysql.h>
#define USERNAME "root"
#define PASSWORD "dkx."
#define LOCALHOST "localhost"
#define DATABASE "ctest"
#define PORT 3306

int main(void)
{
	// 初始化mysql的连接
	MYSQL* mysql = mysql_init(0);
   // 链接数据库 并 判断 数据库是否链接成功了
	if (!mysql_real_connect(mysql, LOCALHOST, USERNAME, PASSWORD, DATABASE, PORT, NULL, NULL))
	{
		printf("MYSQL官方提示您：mysql 链接失败！！！ cause ：[%s]\n", mysql_error(mysql));
		goto exit; // goto一般不推荐使用，但是对于异常处理还是可以用用的
	}
	else
	{
		printf("MYSQL官方提示您：mysql 链接成功！！！\n");
		mysql_set_character_set(mysql, "utf8");
	}
	// 定义要添加到数据库中的一个字符串
	char ch[] = "acc";
	// 定义缓冲区来存储执行的SQL语句
	char sql[1024];
	// 通过使用sprintf可以使用%s来快速的对其进行赋值然后将完整的语句赋值给缓冲区sql
	sprintf(sql, "insert into `c_test` (`name`) values ('%s')", ch);
	// 传入mysql对象，传入sql语句执行操作返回结果。。。一气呵成
	if (mysql_query(mysql, sql))
	{
		printf("MYSQL官方提示您：[%s]\n", mysql_error(mysql));
		goto exit;
	}
	else
	{
		printf("MYSQL官方提示您：insert执行成功!!!\n");
	}
	exit:
	mysql_close(mysql); // 关闭连接，不关闭会造成连接过多卡死
	return 0;
}
//-----------------------运行结果-----------------------
MYSQL官方提示您：mysql 链接成功！！！
MYSQL官方提示您：insert执行成功!!!

E:\C\Demo\Project3\Project5\x64\Debug\Project5.exe (进程 9472)已退出，代码为 0。
按任意键关闭此窗口. . .
```

查看数据库

![image-20230924163008521](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051549235.png)
