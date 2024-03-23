---
title: C语言-中
categories:
    - [计算机学科,C]
tags:
    - 计算机学科
    - C
    - 基础
---

## 5 数组和字符串:christmas_tree: 

### 5.1 概述:deciduous_tree: 

在程序设计中，为了方便处理数据把具有相同类型的若干变量按有序形式组织起来 ——称为 ==数组==。

==数组就是在内存中连续的相同类型的变量空间==。同一个数组所有的成员都是相同的数据类型，同时所有的成员在内存中的地址是连续的。

>  数组本身是一个常量地址

![image-20230923105432207](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309231054063.png)

-  数组名符合标识符的书写规定 (数字，英文字母，下划线)

-  数组名不能与它变量名相同，统一作用域内是唯一的

-  方括号 [ ] 中常量表达式表示数组元素的个数

   ```c
   int a[3] 表示数组a有3个元素
   其下标从0开始计算，因此3个元素分别为 a[0]，a[1]，a[2]
   ```

-  定义数组时 [ ] 内最好是常量，使用数组时 [ ] 内即可是常量，也可以是变量

#### 5.1.1 基本使用:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	/*
	* 变量定义
	* 数据类型  变量  =  值
	* 数组定义
	* 数据类型  数组名[元素个数] = {值1，值2，值3}
	* 元素个数：可以指定也可以不指定，不指定按 {} 中有多少就是多少
	*/
	int arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
	// 通过数组的下标进行赋值或者修改 下标从0开始  作用：获取数组中的元素值
	arr[0] = 100;
	// 数组下标 数组名[下标值]  下标从0开始  作用：获取数组中的元素值
	printf("%d\n", arr[0]); // 100
	printf("%d\n", arr[1]); // 2
	printf("%d\n", arr[2]); // 3
	printf("%d\n", arr[3]); // 4
	// 使用数组的下标一个一个来获取太麻烦
	// 我们想要获取数组中的所有元素值，可以使用for来遍历这个数组
	printf("-------------遍历数组--------------\n");
	int i = 0;
	for (;i < 9;i++)
	{
		printf("%d\n", arr[i]);
	}
	return 0;
}
//----------------------------------运行结果----------------------------------
100
2
3
4
-------------遍历数组--------------
100
2
3
4
5
6
7
8
9

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 15884)已退出，代码为 0。
按任意键关闭此窗口. . .
```

![image-20230923115307834](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309231153967.png)

#### 5.1.2 计算数组的长度:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	int arr[] = {1, 2, 3, 4, 5};
	int i = 0;
	for (;i < 5;i++)
	{	
		printf("arr[%d] = %p\n", i, &arr[i]);
	}
   // arr 本身是一个数组可以直接取到地址不需要使用取地址运算符
	// 数组名是一个地址常量，指向数组首地址的常量
	printf("arr = %p\n", arr);
	// 数组的地址与第一个元素的地址是相同的
	printf("arr = %p | arr[%d] = %p\n", arr, 0, &arr[0]);
	if (arr == &arr[0])
	{
		printf("yes!\n");
	}
	// 但是它们在内存中的大小是不同的
	printf("arr = %d | arr[%d] = %d\n", sizeof(arr),0 , sizeof(arr[0]));// 20, 5
	// sizeof(arr[0])：4 为什么是4？ 因为它是int类型的占用4个字节
	// sizeof(arr)：20 为什么是20？ 因为它是总容器里面有5 个 int类型4个字节的元素
	// 那么 4 * 5 = 20 就是sizeof(arr)的内存大小了
	// arr内存大小：20，arr[0]内存大小：4 ，由此可以求出20 / 4 = 5就是数组的元素个数
	// 获取数组元素个数
	printf("数组元素个数(长度)：%d\n", sizeof(arr)/ sizeof(arr[0])); // 5
	// 既然求出数组的个数那么我们在遍历的时候就不用自己再去看数组长度了
	int i1 = 0;
	for (;i1 < sizeof(arr)/ sizeof(arr[0]);i1++)
	{
		printf("arr[%d] = %d\n", i1, arr[i1]);
	}
	return 0;
}
//-----------------------------运行结果-----------------------------
arr[0] = 000000CF606FF7E8
arr[1] = 000000CF606FF7EC
arr[2] = 000000CF606FF7F0
arr[3] = 000000CF606FF7F4
arr[4] = 000000CF606FF7F8
arr = 000000CF606FF7E8
arr = 000000CF606FF7E8 | arr[0] = 000000CF606FF7E8
yes!
arr = 20 | arr[0] = 4
数组元素个数(长度)：5
arr[0] = 1
arr[1] = 2
arr[2] = 3
arr[3] = 4
arr[4] = 5

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 19920)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 5.1.3 键盘输入对声明好的数组进行赋值:evergreen_tree: 

<font color='red'>注意</font>：声明数组必须给一个容量大小，否则会报错.

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#define SIZE 5

int main(void)
{
	int arr[SIZE];
	int i = 0;
	for (;i < SIZE;i++)
	{
		scanf("%d", &arr[i]);
	}
	int j = 0;
	for (;j < SIZE;j++)
	{
		printf("%d\n", arr[j]);
	}
	return 0;
}
//--------------------------运行结果--------------------------
10 20 30 40 50
10
20
30
40
50

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 18420)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 5.1.4 数组下标越界:evergreen_tree: 

![image-20230923145558436](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309231456242.png)

```c
#include <stdio.h>

int main(void)
{
	int arr[] = {1, 2, 3, 4, 5};
	int i = 0;
	// 数组的长度是5 循环从0开始访问下标0-4，但是长度+1 访问下标0-5 超出了数组的容量
	// 会导致读取到内存中的其它的数据，而不是数组内的数据
	// 如果内存中这个越界的数据是可读的则不会报错，如果是不可读的则会报错。
	for (;i < sizeof(arr)/ sizeof(arr[0]) + 1;i++)
	{
		printf("arr[%d] = %d\n",i , arr[i]);// 1 2 3 4 5 -858993460[越界]
	}
	return 0;
}
//---------------------------运行结果---------------------------
arr[0] = 1
arr[1] = 2
arr[2] = 3
arr[3] = 4
arr[4] = 5
arr[5] = -858993460

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 5656)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 5.1.5 求数组中最大值和最小值:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	int arr[] = {11, 25, 33, 8, 7, 15, 26};
	int max = arr[0];
	int min = 2147483647;
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(arr[0]);i++)
	{
		max = max < arr[i] ? arr[i] : max;
		min = min > arr[i] ? arr[i] : min;
	}
	printf("最大值为：%d\n最小值为：%d\n", max, min);
	return 0;
}
//--------------------------运行结果--------------------------
最大值为：33
最小值为：7

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 13180)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 5.1.6 数组逆置:evergreen_tree: 

##### for版本:palm_tree: 

```c
#include <stdio.h>

int main(void)
{
	// 将该数组逆置：9. 8. 7. 6. 5. 4. 3. 2. 1
	int arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
	// 声明起始变量指向数组头
	int i = 0; 
	// 声明末尾变量指向数组末尾 ，长度-1防止越界
	int len = sizeof(arr) / sizeof(arr[0]) - 1;
	// 获取数组的长度，为了for语句的简洁度
	int arrLen = sizeof(arr) / sizeof(arr[0]);
	// 循环i到数组长度除以2 也就是循环到一半
	for (;i < arrLen / 2;i++)
	{
		// 交换元素 i是头，len是末尾
		// 让从头过来的和从末尾过来的进行交换
		int temp = arr[i];
		arr[i] = arr[len];
		arr[len] = temp;
		// 末尾变量向前移动，i头变量遍历向后移动
		// 循环条件除2也就是 头 和 末尾两个碰着了然后就停止 否则又给换回去了
		len--;
	}
	// 遍历交换后的数组
	int j = 0;
	for (;j < sizeof(arr)/ sizeof(arr[0]);j++)
	{
		printf("arr[%d] = %d\n",j , arr[j]);
	}
	return 0;
}
//-----------------------------运行结果-----------------------------
arr[0] = 9
arr[1] = 8
arr[2] = 7
arr[3] = 6
arr[4] = 5
arr[5] = 4
arr[6] = 3
arr[7] = 2
arr[8] = 1

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 16292)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### while版本:palm_tree: 

```c
#include <stdio.h>

int main(void)
{
	int arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
	int i = 0;
	int j = sizeof(arr) / sizeof(arr[0]) - 1;
	// 判断两头的变量是否碰头了，碰头就停止工作！
	while (i != j)
	{
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
		i++;
		j--;
	}
	int k = 0;
	for (;k < sizeof(arr)/ sizeof(arr[0]);k++)
	{
		printf("arr[%d] = %d\n", k, arr[k]);
	}
	return 0;
}
//--------------------------运行结果--------------------------
arr[0] = 9
arr[1] = 8
arr[2] = 7
arr[3] = 6
arr[4] = 5
arr[5] = 4
arr[6] = 3
arr[7] = 2
arr[8] = 1

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 14976)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 5.1.7 冒泡排序:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	// 将该数组的元素按大小进行排序
	int arr[] = {1, -2, 3, -4, 5, -6 ,7, -8, -9, 10};
	// 定义变量接收数组长度，方便循环遍历数组
	int len = sizeof(arr) / sizeof(arr[0]);
	// 使用嵌套循环，遍历数组
	int i = 0;
	// 定义flag对排序进行优化 省去不必要的判断
	// 思想：如果内循环跑了一周结果一个也没有交换位置，这说明什么？
	// 这说明不需要往后面再走了。再走也是没有意义的！
	int flag = 1;
	for (;i < len;i++)
	{
		// 循环条件 len - i - 1 ，i为动态增长变量，每次执行内循环
		// 长度就会减一，因为交换过了换地方再交换
		int j = 0;
		for (;j < len - i - 1;j++)
		{
			// 判断前一个是否小于后一个满足条件则执行
			if (arr[j] < arr[j + 1])
			{
				// 如果还有需要排序的则将flag设置为0
				flag = 0;
				// 进行排序 交换元素位置
				int temp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = temp;
			}
		}
		// 判断是否需要进行下一周期的运行工作 ！
		if (flag)
		{
			break;
		}
		else
		{
			flag = 1;
		}
	}
	int k = 0;
	for (;k < len;k++)
	{
		printf("arr[%d] = %d\n",k , arr[k]);
	}
	return 0;
}
//--------------------------------运行结果--------------------------------
arr[0] = 10
arr[1] = 7
arr[2] = 5
arr[3] = 3
arr[4] = 1
arr[5] = -2
arr[6] = -4
arr[7] = -6
arr[8] = -8
arr[9] = -9

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 14512)已退出，代码为 0。
按任意键关闭此窗口. . .
```

### 5.2 二维数组:deciduous_tree: 

#### 5.2.1 二维数组的定义和使用:evergreen_tree: 

二维数组定义的一般形式是：

```
类型说明符 数组名[常量表达式1][常量表达式2]
```

其中常量表达式1 表示第一维下标的长度，常量表达式2 表示第二维下标的长度。

`int a[3][4]` 

**第一种格式**：（推荐使用) ，一句话别那么懒，多写一个两个符号累不死人。不仅不累而且还管饱 ！

```c
int arr[2][3] = 
{
	{1, 2, 3},
	{4, 5, 6}
};
```

**第二种格式**：

-  省略了行的长度，列为3 这时程序会以 3 个元素分为 1 列 如下可分为 2 行 3 列

```c
int arr[][3] = 
{
	{1, 2, 3},
	{4, 5, 6}
};
```

**第三种格式**：

-  与 第二种格式同理，也会按现有元素来分

```c
int arr[][3] = {1, 2, 3, 4, 5, 6, 7};
```

<font color='red'>问题</font>：

如果第二种格式，按 3 列的元素来分就不能出现第 4 列元素否则报错如下：

![image-20230923165407567](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309231654023.png)

如果第三种格式，多余出一个元素那么由于分为 3 列 有 2 列 是空余的 由 0 补充如下：

```c
int arr[][3] = {1, 2, 3, 4, 5, 6, 7};
int i = 0;
for (;i < sizeof(arr)/ sizeof(arr[0]);i++)
{
	int j = 0;
	for (;j < (sizeof(arr)/ sizeof(arr[0][0])) / (sizeof(arr) / sizeof(arr[0]));j++)
	{
		printf("%5d", arr[i][j]);
		/*    打印结果：
		*     1    2    3
		*     4    5    6
		*	  7    0    0
		*/
	}
	printf("\n");
}
//-----------------------运行结果-----------------------
    1    2    3
    4    5    6
    7    0    0

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 8392)已退出，代码为 0。
按任意键关闭此窗口. . .
```

-  命令规则同一维数组
-  定义了一个三行四列数组，数组名为a其元素类型为整型，该数组的元素个数为3 X 4个，即：

```c
#include <stdio.h>

int main(void)
{
	int arr[2][3] =
	{
		{1, 2, 3},
		{4, 5, 6}
	};
	// 数组总内存大小：一个元素4字节，共9个 所以 4 * 9 = 24 占用大小
	printf("arr = %d\n", sizeof(arr)); // 24
	// 数组行内存大小：第0行有3个元素，一个元素4字节，共3个 所以 3 * 4 = 12
	printf("arr[0] = %d\n", sizeof(arr[0])); // 12
	// 数组列内存大小：第0行0列是元素1，一个元素4字节，共1个 所以 1 * 4 = 4
	printf("arr[0][0] = %d\n", sizeof(arr[0][0])); // 4

	// 得出二维数组的总长度 6
	printf("二维数组总长度：%d\n", sizeof(arr)/ sizeof(arr[0][0]));
	// 得出二维数组行的长度 2
	printf("二维数组行长度：%d\n", sizeof(arr)/ sizeof(arr[0]));
	// 得出二维数组列的长度 3
	printf("二维数组列长度：%d\n", (sizeof(arr)/ sizeof(arr[0][0]))/ (sizeof(arr)/ sizeof(arr[0])));
	// 二维数组的遍历
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(arr[0]);i++)
	{
		int j = 0;
		for (; j < (sizeof(arr) / sizeof(arr[0][0])) / (sizeof(arr) / sizeof(arr[0])); j++)
		{
			printf("arr[%d][%d] = %d\n",i , j, arr[i][j]);
		}
	}
	return 0;
}
//------------------------------运行结果------------------------------
arr = 24
arr[0] = 12
arr[0][0] = 4
二维数组总长度：6
二维数组行长度：2
二维数组列长度：3
arr[0][0] = 1
arr[0][1] = 2
arr[0][2] = 3
arr[1][0] = 4
arr[1][1] = 5
arr[1][2] = 6

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 11868)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 5.2.2 二维数组的首地址:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	int arr[2][3] =
	{
		{1, 2, 3},
		{4, 5, 6}
	};
	// 因为arr和arr[0] 本身就是一个数组可以直接取到地址但是arr[0][0]是个值
	// 需要使用取地址运算符来取地址
	printf("%p\n", arr); // 0000008C694FF618
	printf("%p\n", arr[0]); // 0000008C694FF618
	printf("%p\n", &arr[0][0]); // 0000008C694FF618
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(arr[0]);i++)
	{
		int  j = 0;
		for (;j < (sizeof(arr)/ sizeof(arr[0][0])) / (sizeof(arr) / sizeof(arr[0]));j++)
		{
			printf("arr[%d][%d] = %p\n",i ,j , &arr[i][j]);
			/*
			* 遍历结果
			* arr[0][0] = 000000D537CFF9A8 《---
			* arr[0][1] = 000000D537CFF9AC
			* arr[0][2] = 000000D537CFF9B0
			* arr[1][0] = 000000D537CFF9B4
			* arr[1][1] = 000000D537CFF9B8
			* arr[1][2] = 000000D537CFF9BC
			*/
		}
	}
	return 0;
}
//-----------------------------运行结果-----------------------------
000000E1016FFBC8
000000E1016FFBC8
000000E1016FFBC8
arr[0][0] = 000000E1016FFBC8
arr[0][1] = 000000E1016FFBCC
arr[0][2] = 000000E1016FFBD0
arr[1][0] = 000000E1016FFBD4
arr[1][1] = 000000E1016FFBD8
arr[1][2] = 000000E1016FFBDC

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 3356)已退出，代码为 0。
按任意键关闭此窗口. . .
```

### 5.3 多维数组:deciduous_tree: 

多维数组的定义与二维数组类似，其语法格式具体如下：

```c
数组类型修饰符 数组名 [n1][n2]...[nn];
```

-  例如

```c
int a[3][4][5]
```

定义了一个三维数组，数组名是a ，数组的长度为3，每个数组的元素又是一个二维数组，这个二维数组的长度是4，并且这个二维数组中的每个元素又是一个一维数组，这个一维数组的长度是5，元素类型是int。

```c
#include <stdio.h>

int main(void)
{
	/**
	*  ---------------------------
	* | 一维数组定义               |
	* | 数据类型 数组名[元素个数]  |
	*  ---------------------------
	* | 二维数组定义               |
	* | 数据类型 数组名[行][列]    |
	*  ---------------------------
	* | 三维数组定义               |
	* | 数据类型 数组名[层][行][列]|
	*  ---------------------------''
	*/
	int arr[2][3][4] =
	{
		{
			{1, 2, 3, 4},
			{5, 6, 7, 8},
			{9, 10, 11, 12}
		},
		{
			{13, 14, 15, 16},
			{17, 18, 19, 20},
			{21, 22, 23, 24}
		}
	};
	// 得出它们所占的内存大小
	printf("%d\n", sizeof(arr)); // 96 层*行*列*sizeof(数据类型) = 96
	printf("%d\n", sizeof(arr[0])); // 48
	printf("%d\n", sizeof(arr[0][0])); // 16
	printf("%d\n", sizeof(arr[0][0][0])); // 4
	// 求出 层，行，列的长度 用于遍历
	printf("层的长度：%d\n", sizeof(arr)/ sizeof(arr[0])); // 2
	printf("行的长度：%d\n", sizeof(arr[0])/ sizeof(arr[0][0])); // 3
	printf("列的长度：%d\n", sizeof(arr[0][0])/ sizeof(arr[0][0][0])); // 4
	// 遍历三维数组
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(arr[0]);i++)
	{
		int j = 0;
		for (;j < sizeof(arr[0])/ sizeof(arr[0][0]);j++)
		{
			int k = 0;
			for (;k < sizeof(arr[0][0]) / sizeof(arr[0][0][0]);k++)
			{
				printf("arr[%d][%d][%d] = %d\t", i, j, k, arr[i][j][k]);
			}
			printf("\n");
		}
	}
	return 0;
}
//----------------------------运行结果----------------------------
96
48
16
4
层的长度：2
行的长度：3
列的长度：4
arr[0][0][0] = 1        arr[0][0][1] = 2        arr[0][0][2] = 3        arr[0][0][3] = 4
arr[0][1][0] = 5        arr[0][1][1] = 6        arr[0][1][2] = 7        arr[0][1][3] = 8
arr[0][2][0] = 9        arr[0][2][1] = 10       arr[0][2][2] = 11       arr[0][2][3] = 12
arr[1][0][0] = 13       arr[1][0][1] = 14       arr[1][0][2] = 15       arr[1][0][3] = 16
arr[1][1][0] = 17       arr[1][1][1] = 18       arr[1][1][2] = 19       arr[1][1][3] = 20
arr[1][2][0] = 21       arr[1][2][1] = 22       arr[1][2][2] = 23       arr[1][2][3] = 24

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 9628)已退出，代码为 0。
按任意键关闭此窗口. . .
```

### 5.4 字符数组与字符串:deciduous_tree: 

#### 5.5.1 字符数组与字符串区别:evergreen_tree: 

-  C语言中没有字符串这种数据类型，可以通过char的数组来替代
-  字符串一定是一个char的数组，但char的数组未必是字符串
-  <font color='red'>数字 0 (和字符 ‘\0’ 等价) 结尾的char数组就是一个字符串</font>，但如果char数组没有以数字0结尾，那么就不是一个字符串，只是普通字符数组，所以<font color='red'>字符串是一种特殊的char的数组</font>。

```c
#include <stdio.h>

int main(void)
{
	/**
	* 总结：字符数组长度设置为n多余的空间补充0
	*		char* str = "hello" 通过sizeof得到的内存大小为8是因为使用了指针，指针地址导致的
	*		指针*为地址，地址的长度和系统有关32位系统sizeof=4，64位系统sizeof=8
	*		字符串结束标志为 \0 数字0等同于\0 但是不等同于 '0'
	*		字符数组和字符串是一样的都是一组字符集合，但是字符串结束标志是\0
	*		char str[] = "hello" 与 char str1[] = {'h', 'e', 'l', 'l', 'o', '\0'}; 本质相同
	*		char str[] = "hello" 通过sizeof获取占用内存大小为6 因为末尾有\0结束符
	*		字符数组通过sizeof获取的内存大小为当前元素的个数
	*/
	// 字符数组 多余的长度补充 0 ，如果使用sizeof的话就会出现乱码
	char ch[] = { 'h', 'e', 'l', 'l', 'o' };
	// 字符串 字符串结束标志为\0  数字0等同于\0  但是不等同于'0'
	char* str = "hello";
	printf("%d\n", sizeof(str)); // 8 为什么是8呢 因为 
	// 指针*为地址，地址的长度和系统有关32位系统sizeof=4，64位系统sizeof=8
	
	// 字符数组和字符串是一样的都是一组字符集合，但是字符串结束标志是\0
	char str1[] = "hello";
	// 上面本质就是下面的格式 ↓
	// char str1[] = {'h', 'e', 'l', 'l', 'o', '\0'};
	printf("%d\n", sizeof(str1)); // 6 str1[]是一个数组没有用到 指针所以sizeof=6
	// char类型1个字节，数组中5个元素每一个元素就是1个字节
	// 1 * 5 = 5 所有ch数组占5个字节
	printf("%d\n", sizeof(ch)); // 5
	int i = 0;
	for (;i < sizeof(ch);i++)
	{
		printf("arr[%d] = %c\t", i, ch[i]);
	}
	return 0;
}
//-------------------------运行结果-------------------------
8
6
5
arr[0] = h      arr[1] = e      arr[2] = l      arr[3] = l      arr[4] = o
E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 908)已退出，代码为 0。
按任意键关闭此窗口. . .
```

>  总结：
>
>  字符数组长度设置为n多余的空间补充0
>
>  char* str = "hello" 通过sizeof得到的内存大小为8是因为使用了指针，指针地址导致的
>
>  指针*为地址，地址的长度和系统有关32位系统sizeof=4，64位系统sizeof=8
>
>  字符串结束标志为 \0 数字0等同于\0 但是不等同于 '0'
>
>  字符数组和字符串是一样的都是一组字符集合，但是字符串结束标志是\0
>
>  char str[] = "hello" 与 char str1[] = {'h', 'e', 'l', 'l', 'o', '\0'}; 本质相同
>
>  char str[] = "hello" 通过sizeof获取占用内存大小为6 因为末尾有\0结束符
>
>  字符数组通过sizeof获取的内存大小为当前元素的个数

#### 5.5.2 字符串的初始化:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	/**
	* C语言没有字符串类型，通过字符数组模拟
	* C语言字符串，以字符 '\0' ，数字0 结尾 如果没有则输出为乱码
	*/
	//不指定长度 没有0结尾符，有多少个元素就有多长 输出就是乱码
	char ch[] = {'h', 'o', 'l', 'l', 'o'};
	printf("%s\n", ch); // 打印乱码
	// 指定长度，后面没有赋值的元素 自动补充0 这样输出就不会乱码了 必须有空余的补0
	char cc[7] = { 'h', 'o', 'l', 'l', 'o' };
	printf("%s\n", cc); // hollo
	/**
	* hollo烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫
	* 烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫
	* 烫烫烫烫烫绦鳺逖
	*/
	//----------------------字符串---------------------------
	char ch1[] = "hello";                                   //|
	printf("%s\n", ch1);                                    //|
	char ch2[] = { 'h', 'o', 'l', 'l', 'o', '\0'};          //|
	printf("%s\n", ch2);                                    //|
	char* str = "hello";                                    //|
	printf("%s\n", str);                                    //|
	char ch3[] = {"hello"};                                 //|
	printf("%s\n", ch3);                                    //|
	//-------------------------------------------------------''
	//--------输出结果：hello--------------------<<<<<<<<<<<>>>
	return 0;
}
//---------------------------运行结果---------------------------
hollo烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫
烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫烫??N_
hollo
hello
hollo
hello
hello

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 17848)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 5.5.3 字符串的输入输出:evergreen_tree: 

由于字符串采用了 ‘\0’ 标志，字符串的输入输出将变得简单方便

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	char ch[100];
	printf("可以带空格：\n >>> ");
	// %[^\n] 表示通过正则表达式 接收非 \n 字符 输入的时候就可以带空格了
	scanf("%[^\n]", ch);
	printf("%s\n", ch);

	char ch1[100];
	printf("空格后面不接受：\n >>>");
	// 默认会以用户输入的空格分隔不接受空格后面的字符串
	scanf("%s", ch1);
	printf("%s\n", ch1);
	return 0;
}
//------------------------运行结果------------------------
可以带空格：
 >>> hello world
hello world
空格后面不接受：
 >>>hello world
hello

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 9036)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 5.5.3.1 gets():palm_tree: 

```c
#include <stdio.h>
char* gets(char* str);
功能：从标准输入读入字符，并保存到指定的内存空间，直到出现换行符或读到文件结尾为止。
参数：
   s: 字符串首地址
返回值：
   成功：读入的字符串
   失败：NULL
```

```c
#include <stdio.h>

int main(void)
{
	char ch[10];
	// 通过键盘输入获取一个字符串
   // gets 接收字符串可以带空格
	gets(ch);
	printf("%s\n", ch);
	return 0;
}
//------------------运行结果------------------
hello world
hello world

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 4816)已退出，代码为 0。
按任意键关闭此窗口. . .
```

gets(str) 与 scanf(“%s”, str) 的区别：

-  gets(str) 允许输入的字符串含有空格
-  scanf(“%s”, str) 不允许含有空格，但是可以使用表达式来完成需求

<font color='red'>注意</font>：

由于scanf() 和 gets() 无法知道字符串s大小，必须遇到换行符或读到文件结尾为止才接受输入，因此容易导致字符数组越界 (缓冲区溢出) 的情况。



##### 5.5.3.2 fgets():palm_tree: 

```c
#include <stdio.h>
char* fgets(char* s, int size, FILE* stream);
功能：从stream指定的文件内读入字符，保存到所指定的内存空间，直到出现换行符，读到文件结尾或是已读了size - 1个字符为止，最后会自动加上字符 '\0' 作为字符串结束
参数：
   s: 字符串
   size：指定最大读取字符串的长度 (size - 1)
   stream：文件指针，如果读键盘输入的字符串，固定为stdin
返回值：
     成功：成功读取的字符串
     读取文件结尾或出错：NULL
```

```c
int main(void)
{
	char ch[10];
	// fgets 可以接收空格
	// fgets 获取字符串少于元素个数会有 \n 大于等于 没有 \n 结尾带 \0
	fgets(ch, sizeof(ch), stdin); // 输入 hello
	printf("%s\n", ch); // 输出 hello\n\0
	return 0;
}
//--------------------运行结果--------------------
hello
hello


E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 1960)已退出，代码为 0。
按任意键关闭此窗口. . .
```

fgets()在读取一个用户通过键盘输入的字符串的时候，同时把用户输入的回车也作为字符串的一部分。通过scanf和gets输入一个字符串的时候，不包含结尾的 ‘\n’ ，<font color='red'>但通过 fgets 结尾多了 ‘\n’ </font>。fgets() 函数是安全的，不存在缓冲区溢出的问题。



##### 5.5.3.3 puts():palm_tree: 

```c
#include <stdio.h>
int puts(const char* s);
功能：标准设备输出s字符串，在输入完成后自动输出一个 '\n'
参数：
   s：字符串首地址
返回值：
   成功：非负数
   失败：-1
```

```c
int main(void)
{
	char ch[] = "hello world";
	// 自带换行
	puts(ch); // hello world\n
	// 打印中途遇到\0结束
	puts("hello\0world"); // hello
	return 0;
}
//--------------------------运行结果--------------------------
hello world
hello

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 4036)已退出，代码为 0。
按任意键关闭此窗口. . .
```



##### 5.5.3.4 fputs():palm_tree: 

```c
#include <stdio.h>
int fputs(const char* str, FILE* stream);
功能：将str所指定的字符串写入到stream指定的文件中，字符串结束符 '\0' 不写入文件。
参数：
   str：字符串
   stream：文件指针，如果把字符串输出到屏幕，固定写为stdout
返回值：
   成功：0
   失败：-1
```

fputs() 是 puts() 的文件操作版本，但fputs() ==不会自动输出一个 ‘\n’==。

```c
printf("hello world");
puts("hello world");
fputs("hello world", stdout);
```

 

##### 5.5.3.5 strlen():palm_tree: 

```c
#include <string.h>
size_t strlen(const char* s);
功能：计算指定字符s的长度，不包含字符串结束符 '\0'
参数：
   s：字符串首地址
返回值：字符串s的长度，size_t为unsigned int类型
```

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
	char ch[] = "hello world";
	printf("数组内存大小：%d\n", sizeof(ch)); // 12 包含了 '\0'
	printf("字符串的长度：%d\n", strlen(ch)); // 11 不包含 '\0'
	return 0;
}
//------------------------运行结果------------------------
数组内存大小：12
字符串的长度：11

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 4120)已退出，代码为 0。
按任意键关闭此窗口. . .
```



#### 5.5.4 字符串拼接:evergreen_tree: 

```c
#define SIZE1 20

int main(void)
{
	char ch[] = "hello";
	char ch1[] = "world";
	char ch2[SIZE1];
	int i = 0;
	int j = 0;
	while (ch[i] != '\0')
	{
		ch2[i] = ch[i];
		i++;
	}
	while (ch1[j] != '\0')
	{
		ch2[i + j] = ch1[j];
		j++;
	}
	ch2[i + j] = '\0';
	printf("arr = %s\n", ch2);
	return 0;
}
//-----------------------------运行结果-----------------------------
arr = helloworld

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 7936)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 5.5.5 键盘输入字符数组字符串:evergreen_tree: 

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#define SIZE 5

int mainqwesd(void)
{
	// 定义字符数组存储字符串
	char ch[SIZE];
	// 可以使用%4s占位符来对用户输入的字符串长度做限制，意思是只要输入的前4个字符串
	scanf("%4s", ch);
	printf("%s", ch);
	return 0;
}
//----------------------------运行结果----------------------------
jiaosndoandiwbyagboiabybg
jiao
E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 17040)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 5.5.6 字符数组赋值数值型:evergreen_tree: 

```c
int main(void)
{
	// 字符数组中使用数值的话会转换为对应的ASCII编码 长度为9 余位补0 避免乱码
	char arr[9] = {97, 98, 99, 100, 110, 111, 112, 123};
	printf("%s\n", arr);
	return 0;
}
//------------------------运行结果------------------------
abcdnop{

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 19308)已退出，代码为 0。
按任意键关闭此窗口. . .
```

## 6 函数:christmas_tree: 

### 6.1 概述:deciduous_tree: 

#### 6.1.1 函数分类:evergreen_tree: 

>  C程序是由函数组成的，我们写的代码都是由主函数 main() 开始执行的。函数是C程序的基本模块，是用于完成特定任务的程序代码单元
>
>  从函数定义的角度看，函数可分为系统函数和用户定义函数两种
>
>  -  **系统函数，即库函数**：
>
>     这是由编译系统提供的，用户不必自己定义这些函数，可以直接使用它们，如我们常用的打印函数 printf()
>
>  -  **用户定义函数**：
>
>     用以解决用户的专门需求

#### 6.1.2 函数的作用:evergreen_tree: 

函数的使用可以省去重复代码的编写，降低代码重复率

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	int b = 15;
	int res = max(a, b);
	printf("调用函数求两值最大值为：%d\n", res);
	return 0;
}

// 求最大值函数
int max(int a, int b)
{
	return a < b ? b : a;
}
//--------------------运行结果--------------------
调用函数求两值最大值为：15

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 3100)已退出，代码为 0。
按任意键关闭此窗口. . .
```

-  函数可以让程序更加模块化，从而有利于程序的阅读，修改和完善。

假如我们编写一个实现以下功能的程序：读入一行数字；对数字进行排序；找到它们的平均值；打印一个柱状图。如果我们把这些操作直接写在main() 里，这样可能会给用户带来感觉代码会有点凌乱。但，假如我们使用函数，这样可以让程序更加清晰，模块化。

```c
#include <stdio.h>
#define SIZE 50

int main(void)
{
	float list[SIZE];
	// 这里只是举例，函数还没有实现
	readlist(list, 50);
	sort(list, 50);
	average(list, 50);
	bargraph(list, 50);
	return 0;
}
```

这里我们可以这么理解，程序就像公司，公司是由部分组成的，这个部门就类似于C程序的函数。默认情况下，公司就是一个大部门(只有一个部门的情况下) ，相当于C程序的main() 函数。如果公司比较小 (程序比较小) ，因为任务少而简单，一个部门即可(main() 函数) 胜任。但是，如果这个公司很大 (大型应用程序)，任务多而杂，如果只是一个部门管理 (相当于没有部门，没有分工)，我们可想而知，公司管理，运营起来会有多混乱， 不是说这样不可以运营，只是这样不完美而已，如果根据公司要求分成一个一个部门 (根据功能封装一个一个函数)，招聘由行政部门负责，研发由技术部门负责等，这样就可以分工明确，结构清晰，方便管理，各部门之间还可以互相协调。

#### 6.1.3 函数的调用：产生随机数:evergreen_tree: 

当调用函数时，需要关心5要素：

-  头文件：包含指定的头文件
-  函数名字：函数名字必须和头文件声明的名字一样
-  功能：需要知道此函数能干嘛后才调用
-  参数：参数类型要匹配
-  返回值：根据需要接收返回值

```c
#include <time.h>
time_t time(time_t* t);
功能：获取当前系统时间
参数：常设置为NULL
返回值：当前系统时间，time_t 相当于long类型，单位为毫秒
```

```c
#include <stdlib.h>
void srand(unsigned int seed);
功能：用来设置rand()产生随机数的种子，不设置种子随机数都是一样的。
参数：如果每次seed相等，rand()产生随机数相等
返回值：无
```

```c
#include <stdlib.h>
int rand(void);
功能：返回一个随机数值
参数：无
返回值：int类随机数
```

代码演示：

```c
#include <stdio.h>
#include <time.h>
#include <stdlib.h>

int main(void)
{
	/*time_t timer = time(NULL);
	srand((size_t)timer);*/
	// 简写
	srand((size_t)time(NULL));
	int i = 0;
	for (;i < 10;i++)
	{
		// 循环10次 得出 1 ~ 100 之间的随机数
		int r = rand() % 100 + 1;
		printf("%d\n", r);
	}
	return 0;
}
//----------------------------运行结果----------------------------
73
11
64
1
6
73
9
7
20
53

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 14376)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 6.1.4 双色球小案例:evergreen_tree: 

```c
#include <stdio.h>
#include <time.h>
#include <stdlib.h>

void sort(int* a, int len)
{
	int flag = 0;
	int i = 0;
	for (; i < len - 1; i++)
	{
		int j = 0;
		for (; j < len - i - 1; j++)
		{
			if (a[j] > a[j + 1])
			{
				flag = 0;
				int temp = a[j + 1];
				a[j + 1] = a[j];
				a[j] = temp;
			}
		}
		if (flag)
		{
			break;
		}
		else
		{
			flag = 1;
		}
	}
}

#define SIZE 6

int main(void)
{
	// 双色球 6个红球 1 - 32 1个篮球 1 - 16 
	srand((size_t)time(NULL));
	int c = 0;
	for (;c < 5;c++)
	{
	int arr[SIZE];
	int value = 0;
	int flag = 0;
	int i = 0;
		printf("-------------------第 %d 组-------------------\n", c + 1);
		for (; i < 6; i++)
		{
			value = rand() % 32 + 1;
			// 去重
			if (i >= 1)
			{
				if (value == arr[i - 1])
				{
					continue;
				}
			}
			arr[flag] = value;
			flag++;
		}
		sort(arr, sizeof(arr)/ sizeof(arr[0]));
		int k = 0;
		for (; k < sizeof(arr) / sizeof(arr[0]); k++)
		{
			printf("%d ", arr[k]);
		}
		printf("+%d\n", rand() % 16 + 1);
		printf("-------------------end-------------------\n");
	}
	return 0;
}
//----------------------------运行结果----------------------------
-------------------第 1 组-------------------
4 5 6 9 23 32 +7
-------------------end-------------------
-------------------第 2 组-------------------
4 6 7 8 25 32 +15
-------------------end-------------------
-------------------第 3 组-------------------
2 3 16 17 28 32 +2
-------------------end-------------------
-------------------第 4 组-------------------
10 11 15 23 29 30 +8
-------------------end-------------------
-------------------第 5 组-------------------
3 27 28 29 30 31 +6
-------------------end-------------------

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 16712)已退出，代码为 0。
按任意键关闭此窗口. . .
```

### 6.2 函数的定义:deciduous_tree: 

#### 6.2.1 函数定义格式:evergreen_tree: 

#### 6.2.1 函数的声明:evergreen_tree: 

如果使用用户自己定义的函数，而该函数与调用它的函数(即主函数)不再同一文件中，或者 <font color='red'>函数定义的位置在主函数之后</font>，则必须在调用此函数之前对被调用的函数作声明使用关键字：extern。

所谓函数声明，就是在函数尚在未定义的情况下，事先将该函数的有关信息通知编译系统，相当于告诉编译器，函数在后面定义，以便使编译能正常进行。

<font color='red'>注意</font>：一个函数只能被定义一次，但可以声明多次。

程序依次往下执行，函数声明需要写在main函数的上面，直接在上面写函数可以直接定义，如果下面写函数需要在main函数上面先声明函数

```c
#include <stdio.h>

// 在main函数的前面可以直接定义函数并调用
void out()
{
	printf("hello!");
}

// 声明函数 建议使用extern 不要懒 不要懒 不要懒 重要事情说三遍
extern void out1();
// 声明函数 可以只写一个样式，定义的时候写全
extern void out2(int, int);

int main(void)
{
	// 调用函数
	out1();
	// 调用函数
	out();
   // 调用函数
   out2(1, 2);
	return 0;
}

// 定义函数 ，需要先上main函数前声明后定义才能调用使用,否则报错
void out1()
{
	printf("world!");
}
void out2(int a, int b)
{
   printf("%d + %d = %d", a, b, a + b);
}
//---------------------运行结果---------------------
world!hello!
E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 4952)已退出，代码为 0。
按任意键关闭此窗口. . .
```

**函数定义的一般形式**：

```c
返回值类型 函数名(参数列表)
{
   数据定义部分;
   执行语句部分;
   return 返回值;
}
```

![image-20230924113332383](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241133075.png)

```c
#include <stdio.h>

/**
* 返回值类型 函数名(参数列表)
* {
*	代码体(复合代码);
*   return 返回值;
* }
*/
// 定义加法函数
int add(int a, int b)
{
	return a + b;
}
int main(void)
{
	int a = 10;
	int b = 10;
	// 调用加法函数，得到返回的结果
	int sum = add(a, b);
	printf("%d\n", sum);
	return 0;
}
//------------------------运行结果------------------------
20

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 9796)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 6.2.2 函数名字，形参，函数体，返回值:evergreen_tree: 

##### 6.2.2.1 函数名:palm_tree: 

理论上是可以随意起名字，最好起的名字 见名知意 ，应该让用户看到这个函数名字就知道这个函数的功能。注意，函数名的后面有个小括号() ，代表这个为函数，不是普通的变量名

##### 6.2.2.2 形参列表:palm_tree: 

在定义函数时指定的形参，<font color='red'>在未出现函数调用时，它们并不占用内存中的存储单元</font>，因此称它们是形式参数或虚拟参数，简称形参，表示它们并不是实际存在的数据，所以，形参里的变量不能赋值。

```c
void max(int a = 10, int b = 20) // error 形参不能赋值
{
   
}
```

在定义函数时指定的形参，必须是，类型 + 变量的形式：

```c
// 1: success 类型 + 变量
void max(int a, int b)
{
   
}
// 2: error 只有类型，没有变量
void max(int, int)
{
   
}
// 3: error 只有变量，没有类型
int a, int b
void max(a, b)
{
   
}
```

在定义函数时指定的形参，可有可无，根据函数的需要来设计，<font color='red'>如果没有形参，圆括号内容为空，或写一个void关键字</font>：

```c
// 没有形参，圆括号内容为空
void max()
{
   
}
// 没有形参，圆括号内容为void关键字
void max(void)
{
   
}
```

##### 6.2.2.3 函数体:palm_tree: 

花括号 { } 里的内容即为函数体的内容，这里为<font color='red'>函数功能实现的过程</font>，这和以前的写代码没太大区别，以前我们把代码写在main() 函数里，现在只是把这些写到别的函数里。

##### 6.2.2.4 返回值:palm_tree: 

函数的返回值是通过函数中的return语句获得的，return后面的值也可以是一个表达式

1 尽量保证return语句中表达式的值和函数返回类型是同一类型

```c
int max() // 函数的返回值 为int类型
{
   int a = 10;
   return a; // 返回值a为int类型，函数返回类型也是int类型，函数返回值类型和返回值的类型要匹配
}
```

2 如果函数返回的类型和return语句中表达式的值不一致，则以函数返回类型为准，即<font color='red'>函数返回类型决定返回值的类型</font>。对数值型数据，可以自动进行转换。

```c
double max() // 函数的返回值为double类型
{
   int a = 10;
   return a; // 返回值a为int类型，它会转换为double类型再返回
}
```

<font color='red'>注意</font>：

如果函数返回的类型和return语句中表达式的值不一致，而它又无法自动进行类型转换，程序则会报错。

3 return 语句的另一个作用为中断 return 所在的执行函数，类似于 break 中断循环，switch 语句一样。

```c
int max()
{
   return 1; // 执行到，函数已经被中断，所以下面的return 2无法被执行到
   return 2; // 没有执行
}
```

4 如果函数带返回值，return 后面必须跟着一个值，如果函数没有返回值，函数名字的前面必须写一个 void 关键字，这时候，我们写代码时也可以通过 return 中断函数 (也可以不同)，只是这时，return后面不带内容 (分号 “ ; ” 除外)。

```c 
void max() // 最好要有void关键字
{
   return; // 中断函数，这个可有可无
}
```

### 6.3 函数的调用:deciduous_tree: 

<font color='red'>定义函数后，我们需要调用此函数才能执行到这个函数里的代码段</font>。这和main()函数不一样，main() 为编译器设定好自动调用的主函数，无需认为调用，我们都是在main() 函数里调用别的函数，<font color='red'>一个C程序里有且只有一个main()函数</font>。

```c
#include <stdio.h>

void print_test()
{
   printf("this is for test\n");
}

int main()
{
   print_test(); // print_test函数的调用
   return 0;
}
```

#### 6.3.1 函数执行流程:evergreen_tree: 

1.  进入main() 函数
2.  调用print_test() 函数
    1.  它会在main()函数的前面寻找一个名字叫 “print_test” 的函数定义
    2.  如果找到，接着检查函数的参数，这里调用函数时没有传参，函数定义也没有形参，参数类型匹配。
    3.  开始执行 print_test() 这一段代码，等待 print_test() 函数的执行
3.  print_test() 函数执行完 (这里打印一句话) ，main() 才会继续往下执行，执行到 return 0，程序执行完毕

#### 6.3.2 函数的形参和实参:evergreen_tree: 

-  形参出现在函数定义中，在整个函数体内部都可以使用，离开该函数则不能使用。
-  实参出现在主调函数中，进入被调函数后，实参也不能使用。
-  实参变量对形参变量的数据传递是 “值传递”， 即单向传递，<font color='red'>只由实参传给形参，而不能由形参传回来给实参</font>.
-  在调用函数时，编译系统临时给形参分配存储单元。调用结束后，形参单元被释放
-  实参单元与形参单元是不同的单元。调用结束后，形参单元被释放，函数调用结束返回主调函数后则不能使用改形参变量。实参单元仍保留并维持原值。<font color='red'>因此，在执行一个被调用函数时，形参的值如果发生改变，并不会改变主调函数中实参的值</font>。

##### 值传递

```c
#include <stdio.h>

void swap(int a, int b)
{
	printf("交换前：a = %d\n", a);
	printf("交换前：b = %d\n", b);
	int temp = a;
	a = b;
	b = temp;
	printf("交换后：a = %d\n", a);
	printf("交换后：b = %d\n", b);
}

int main(void)
{
	int a = 10;
	int b = 20;
	printf("交换前：a = %d\n", a);
	printf("交换前：b = %d\n", b);
	printf("--------------swap--------------\n");
	swap(a, b);
	printf("--------------swap--------------\n");
	printf("交换后：a = %d\n", a);
	printf("交换后：b = %d\n", b);
	return 0;
}
//-----------------------------运行结果-----------------------------
交换前：a = 10
交换前：b = 20
--------------swap--------------
交换前：a = 10
交换前：b = 20
交换后：a = 20
交换后：b = 10
--------------swap--------------
交换后：a = 10
交换后：b = 20

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 16400)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 地址传递

```c
#include <stdio.h>
// 函数的参数列表中接受的是一个arr数组是一个地址常量并不是变量的值
void maoPaoSort(int arr[], int len)
{
	int flag = 1;	
	int i = 0;
	for (;i < len - 1;i++)
	{
		int j = 0;
		for (;j < len - 1 - i;j++)
		{
			if (arr[j] > arr[j + 1])
			{
				flag = 0;
				int temp = arr[j + 1];
				arr[j + 1] = arr[j];
				arr[j] = temp;
			}
		}
		if (flag)
		{
			break;
		}
		else
		{
			flag = 1;
		}
	}
}

int main(void)
{
	int arr[] = {2, 4, 3, 1, 8, -2, 0, -9, 7, 5, 6};
	// 调用函数传入实参 进行排序然后遍历这个main函数的数组
	maoPaoSort(arr, sizeof(arr)/ sizeof(arr[0]));
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(arr[0]);i++)
	{
		printf("arr[%d] = %d\n", i, arr[i]);
	}
	return 0;
}
//-------------------------运行结果-------------------------
arr[0] = -9
arr[1] = -2
arr[2] = 0
arr[3] = 1
arr[4] = 2
arr[5] = 3
arr[6] = 4
arr[7] = 5
arr[8] = 6
arr[9] = 7
arr[10] = 8

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 10564)已退出，代码为 0。
按任意键关闭此窗口. . .
```



##### 6.3.2.1 字符串比较:palm_tree: 

```c
#include <stdio.h>

int my_strcompare(char arr[], char arr1[])
{
	int i = 0;
	// 判断两个字符串的字符是否相同
	while (arr[i] == arr1[i])
	{
		// 判断是否到字符串的结尾
		if (arr[i] == '\0')
		{
			return 1;
		}
		i++;
	}
	return 0;
}

int main(void)
{
	char ch[] = "hello";
	char ch1[] = "hello";
	int flag = my_strcompare(ch, ch1);
	if (flag)
	{
		printf("yes");
	}
	else
	{
		printf("no");
	}
	return 0;
}
//--------------------------------运行结果--------------------------------
yes
E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 16332)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 6.3.3 无参函数调用:evergreen_tree: 

如果是调用无参函数，则不能加上 “实参” ，但括号不能省略

```c
// 函数的定义
void test()
{
   printf("hello");
}

int main()
{
   // 函数的调用
   test(); // success ，圆括号 () 不能省略
   // test(250); // error ，函数定义时没有参数
   return 0;
}
//-------------------运行结果-------------------
hello
```

#### 6.3.4 有参函数调用:evergreen_tree: 

1 如果实参列表包含多个实例，则各参数间用逗号隔开。

```c
void test(int a, int b)
{
   printf("%d\n%d\n", a, b);
}

int main()
{
   int p = 10, q = 20;
   test(p, q); // 函数的调用
   return 0;
}
//----------------------运行结果----------------------
10
20
```

2 实参与形参的个数应相等，类型应匹配 (相同或赋值兼容)。实参与形参按顺序对应，一对一的传递数据。

3 实参可以是常量，变量或表达式，<font color='red'>无论实参是何种类型的量，在进行函数调用时，它们都必须具有确定的值，以便把这些值传递给形参</font>，所以，这里的变量是在圆括号( )外面定义好，赋好值的变量。

```c
// 函数的定义
void test(int a, int b)
{w
   
}

int main()
{
   // 函数的调用
   int p = 10, q = 20;
   test(p, q); // success
   test(11, 30 - 10); // success
   test(int a, int b); // error，不应该在圆括号里面定义变量
}
```

#### 6.3.5 函数的返回值:evergreen_tree: 

1 如果函数定义没有返回值，函数调用时不能写 void 关键字，调用函数时也不能接收函数的返回值

```c
#include <stdio.h>

void print_test()
{
	printf("hello");
}

int main(void)
{
	// void print_test(); // error
	print_test();
	return 0;
}
//---------------运行结果---------------
hello
E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 6792)已退出，代码为 0。
按任意键关闭此窗口. . .
```

2 **函数的返回值**就是当这个函数被调用执行结束之后向函数调用者返回的一个结果。  注意，与参数表不同，一个函数可以有多个输入参数，但只能有一个返回结果。  函数需要用return语句来定义其返回值，return语句后的表达式将作为函数的返回值，而这个值必须要与定义函数时的返回值类型一致。如果返回值类型为数组则需要将其转化为指针的返回值类型也就是在返回值类型后加*

```c
#include <stdio.h>

extern char* add(char*, char*);
extern int* concat(int, int, int);
extern int add1(int, int);

int main(void)
{
	// 字符串返回值类型
	char ch[] = "hello";
	char ch1[] = "world !";
	char* p = add(ch, ch1);
	printf("p = %s\n", p);
	// int类型数组返回值类型
	int a = 10;
	int b = 20;
	int c = 30;
	int* p1 = concat(a, b, c);
	int i = 0;
	for (;i < 3;i++)
	{
		printf("p1 = %d\n", *(p1 + i));
	}
	// int类型返回值
	int a1 = 10;
	int b1 = 20;
	int res = add1(a1, b1);
	printf("res = %d\n", res);
	return 0;
}
char* add(char* a,char* b)
{
	char ch[100] = "";
	int i = 0;
	while (ch[i] = *a++)i++;
	while (ch[i] = *b++)i++;
	return ch;
}

int* concat(int a, int b, int c)
{
	int arr[100] = { a, b, c };
	return arr;
}

int add1(int a, int b)
{
	return a + b;
}
//-------------------------------运行结果-------------------------------
p = helloworld !
p1 = 10
p1 = 20
p1 = 30
res = 30

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 12572)已退出，代码为 0。
按任意键关闭此窗口. . .
```



### 6.4 main函数与exit函数:deciduous_tree: 

在main函数中调用exit和return结果是一样的，但在子函数中调用return只是代表子函数终止了，然后返回主函数继续执行，在子函数中调用exit，那么整个程序终止！

```c
#include <stdio.h>

void test()
{
	printf("hello world-----test\n");
	return; // 终止了子函数的执行返回到主函数中
	printf("hello world-----test\n");
}

void error()
{
	printf("hello world-----error\n");
	exit(0); // 终止整个程序运行
	printf("hello world-----error\n");
}

int main(void)
{
	printf("hello world-----main\n");
	test();
	printf("hello world----- test完毕 - main\n");
	error(); // 在error函数中执行了exit(0) 整个程序终止运行
	printf("hello world----- test - error完毕 - main\n");
	printf("hello world----- main 完毕\n");
	return 0;
}
//----------------------运行结果----------------------
hello world-----main
hello world-----test
hello world----- test完毕 - main
hello world-----error

E:\C\Demo\Project3\Project4\x64\Debug\Project4.exe (进程 3820)已退出，代码为 0。
按任意键关闭此窗口. . .
```

### 6.5 多文件(分文件)编程:deciduous_tree: 

#### 6.5.1 分文件编程:evergreen_tree: 

-  把函数声明放在头文件 xxx.h 中，在主函数中包含相应头文件
-  在头文件对应的xxx.c中实现xxx.h声明的函数
-  头文件中 #pragma once 表示 防止头文件重复包含 
-  在头文件中可以定义全局变量，这个变量的作用域范围是整个源程序但是使用是需要引入对应的头文件的

规范：

-  一个功能文件 hello.c 对应一个 头文件 hello.h 两个命名一致

代码演示：

02fun.h 头文件 声明某个功能模块的函数

```c
#pragma once // 防止头文件重复包含

// 全局变量的定义
char ch[] = "刘桑";
// 函数的声明
extern int max(int, int);

```

02fun.c 函数文件 实现具体的功能

```c
int max(int a, int b)
{
	return a > b ? a : b;
}
```

01main.c 主函数文件

```c
#include <stdio.h>
#include "02fun.h"

int main(void)
{
	int res = max(1, 2);
	printf("%s 觉得, 最大值为： %d\n", ch, res);
	return 0;
}
//--------------------运行结果--------------------
刘桑 觉得, 最大值为： 2

E:\C\Demo\Project3\Project7\x64\Debug\Project7.exe (进程 6792)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 6.5.2 防止头文件重复包含:evergreen_tree: 

当一个项目比较大时，往往都是分文件，这时候有可能不小心把同一个头文件include 多次，或者头文件嵌套包含，这时预处理时会报错。

<font color='red'>注意</font>：头文件有#pragma once 会自动进行处理，运行程序并不会报错，想报错需要将其注释掉

02fun.h 中包含 03fun.h

```c
// #pragma once // 防止头文件重复包含
#include "03.fun.h"

// 全局变量的定义
char ch[] = "刘桑";
// 函数的声明
extern int max(int, int);

```

03fun.h 中包含 02fun.h 

```c
// #pragma once
#include "02fun.h"
// 此时编译器已经给出了报错提示了但是不用管
char ch1[] = "张三";
```

01main.c 主函数文件中使用其中某个头文件

```c
#include <stdio.h>
#include "02fun.h"

int main(void)
{
	int res = max(1, 2);
	printf("%s 觉得, 最大值为： %d\n", ch, res);
	return 0;
}
```

编译上面的例子，会出现如下报错提示：

![Snipaste_2023-09-24_17-55-07](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309241755080.png)

为了避免同一个文件被include多次，C/C++中有两种方式，一种是 #ifndef 方式，一种是 #pragma once方式

我们看下#ifndef方式：

```c
// #pragma once // 防止头文件重复包含
// 将内容全部包裹起来
// 名字中_H表示头文件
#ifndef __02_UFO_H_NB // 宏定义名字，不要跟其它名字冲突
#define __02_UFO_H_NB
#include "03.fun.h"

// 全局变量的定义
char ch[] = "刘桑";
// 函数的声明
extern int max(int, int);
#endif
```

这种方式用的是最多的，因为 #pragma once中能用于windows中其它操作系统中就没用了

#### 6.5.3 gcc编译:evergreen_tree: 

我们使用gcc编译时需要将包含的其它的头文件和其它的.c文件 需要在命令行中加上这些文件，否则报错

02fun.h 头文件

```c
// #pragma once // 防止头文件重复包含
// 将内容全部包裹起来
// 名字中_H表示头文件
#ifndef __02_UFO_H_NB // 宏定义名字，不要跟其它名字冲突
#define __02_UFO_H_NB

// 全局变量的定义
char ch[] = "刘桑";
// 函数的声明
extern int max(int, int);
#endif

```

02fun.c 函数文件

```c
int max(int a, int b)
{
	return a > b ? a : b;
}
```

01main.c 主函数文件

```c
#include <stdio.h>
#include "02fun.h"

int main(void)
{
	int res = max(1, 2);
	printf("%s 觉得, 最大值为： %d\n", ch, res);
	return 0;
}
```

只编译01main.c 或者 01main.c 与 02fun.h 的报错情况

```c
E:\C\Demo\Project3\Project7\Project7>gcc -o 01main.exe 01main.c
D:/ScoopApps/apps/mingw/13.1.0-rt_v11-rev1/bin/../lib/gcc/x86_64-w64-mingw32/13.1.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\Users\ADMINI~1\AppData\Local\Temp\ccYBH7mS.o:01main.c:(.text+0x18): undefined reference to `max'
collect2.exe: error: ld returned 1 exit status

E:\C\Demo\Project3\Project7\Project7>
```

```c
E:\C\Demo\Project3\Project7\Project7>gcc -o 01main.exe 01main.c 02fun.h
D:/ScoopApps/apps/mingw/13.1.0-rt_v11-rev1/bin/../lib/gcc/x86_64-w64-mingw32/13.1.0/../../../../x86_64-w64-mingw32/bin/ld.exe: C:\Users\ADMINI~1\AppData\Local\Temp\ccA1r5NV.o:01main.c:(.text+0x18): undefined reference to `max'
collect2.exe: error: ld returned 1 exit status

E:\C\Demo\Project3\Project7\Project7>
```

但是编译 01main.c 与 02fun.c 时并不会报错，但是不建议这么干还是都带上连着头文件

```c
E:\C\Demo\Project3\Project7\Project7>gcc -o 01main.exe 01main.c 02fun.c

E:\C\Demo\Project3\Project7\Project7>
```

使用gcc编译分文件编程的正确方式

```c
E:\C\Demo\Project3\Project7\Project7>gcc -o 01main.exe 01main.c 02fun.c 02fun.h

E:\C\Demo\Project3\Project7\Project7>
```
