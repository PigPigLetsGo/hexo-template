---
title: C语言-下
categories:
    - [计算机学科,C]
tags:
    - 计算机学科
    - C
    - 基础
---

## 7 指针:christmas_tree: 

### 7.1 概述:deciduous_tree: 

#### 7.1.1 内存:evergreen_tree: 

**内存含义**：

-  **存储器**：计算机的组成中，用来存储==程序==和==数据==，==辅助CPU进行运算处理==的重要部分
-  **内存**：==内部存储器==，暂存程序 / 数据——==断电丢失== SRAM，DRAM，DDR，DDR2，DDR3
-  **外存**：==外部存储器==，长时间保存程序 / 数据——==断点不丢失== ROM ，ERROM，FLASH (NAND，NOR)，硬盘，光盘。

**内存是沟通CPU与硬盘的桥梁**：

-  暂存放，CPU中的运算数据
-  暂存放，与硬盘等外部存储器交换的数据

#### 7.1.2 物理存储器和存储地址空间:evergreen_tree: 

**有关内存的两个概念**：==物理存储器==和==存储地址空间==.

**物理存储器**：实际存在的具体存储器芯片

-  主板上装插的内存条
-  显示卡上的显示RAM芯片
-  各种适配卡上的RAM芯片和ROM芯片

**存储地址空间**：对存储器编码的范围。我们在软件上常说的内存是指这一层含义

-  ==编码==：对每个物理存储单元 (一个字节) 分配一个号码
-  ==寻址==：可以根据分配的号码找到对应的存储单元，完成数据的读写

#### 7.1.3 内存地址:evergreen_tree: 

-  将内存抽象成一个很大的一维字符数组
-  编码就是对内存的每一个字节分配一个32位或64位的编号 (与32位或者64位处理器相关)
-  这个内存编号我们称之为内存地址。

内存中的每一个数据都会分配一个地址：

-  char：占一个字节分配一个地址
-  int：占四个字节分配四个地址
-  float，struct，函数，数组等 

编写一段代码：

```c
#include <stdio.h>

int main(void)
{
   // 使用十六进制的数据 查看内存的时候不会被转换
	int a = 0xaabbccdd;
	printf("%p\n", &a);
	getchar();
	return 0;
}
```

在getchar()位置打断点然后进行debug，从终端复制一下a变量的地址，到调试的内存中进行查询

![image-20230924201641000](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309242016365.png)

当我们对这个内存地址进行一下修改访问000000F0470FF7B5呢

![image-20230924201731912](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309242017862.png)

访问000000F0470FF7B6呢

![image-20230924201803904](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309242018216.png)

终端输出的是a变量内存地址的首地址，一个int类型4个字节 8 个比特。首地址与其它的三个内存地址是连续的一个内存空间

![image-20230924205949831](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309242059565.png)

#### 7.1.4 指针和指针变量:evergreen_tree: 

-  <font color='red'>内存区的每一个字节都有一个编号，这就是 “地址”</font>.
-  如果在程序中定义了一个变量，在对程序进行编译或运行时，系统就会给这个变量分配内存单元，并确定它的内存地址(编号)
-  指针的实质就是内存 “地址” ，指针就是地址，地址就是指针
-  <font color='red'>指针是内存单元的编号，指针变量是存放地址的变量</font>.
-  通常我们叙述时会把指针变量简称为指针，实际它们含义并不一样

![image-20230925111922033](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309251119753.png)

### 7.2 指针的基础知识:deciduous_tree:  

#### 7.2.1 指针变量的定义和使用:evergreen_tree: 

-  指针也是一种==数据类型==，指针变量也是一种==变量==.
-  指针变量指向谁，就把谁的地址赋值给指针变量
-  ‘*’ 操作符操作的是指针变量指向的内存空间

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	// 定义指针变量存储变量地址  指针类型 -> 数据类型*
	int* p;
	// 取出a对应的地址赋值给 指针变量p
	p = &a;
	printf("%p\n", &a);
	printf("%p\n", p);
	printf("----------------\n");
	printf("%d\n", a);
	printf("%d\n", *p);
	return 0;
}
//----------------------运行结果----------------------
000000A2548FFCB4
000000A2548FFCB4
----------------
10
10

E:\C\Demo\Project3\指针\x64\Debug\指针.exe (进程 18928)已退出，代码为 0。
按任意键关闭此窗口. . .
```

![image-20230924212109809](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309242121778.png)

#### 7.2.2 通过指针间接修改变量的值:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	// 定义指针变量存储变量地址  指针类型 -> 数据类型*
	int* p;
	// 取出a对应的地址赋值给 指针变量p
	p = &a;
	// 通过指针间接改变 变量的值
	*p = 100;
	printf("%d\n", a);
	printf("%d\n", *p);
	return 0;
}
//---------------------运行结果---------------------
100
100

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 15900)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.2.3 指针大小:evergreen_tree: 

-  <font color='red'>使用sizeof() 测量指针的大小，得到的总是：4 或 8</font>.
-  sizeof()测的是指针变量指向存储地址的大小
-  在32位平台，所有的指针(地址) 都是32位(4字节)
-  在64位平台， 所有的指针(地址) 都是64位(8字节)

```c
int main(void)
{
	// 指针类型的内存空间根据操作系统位的不同而不同
	printf("int %d\n", sizeof(int*)); // x86: 4 | x64: 8
	printf("char %d\n", sizeof(char*)); // x86: 4| x64: 8
	printf("short %d\n", sizeof(short*)); // x86: 4| x64: 8
	printf("long %d\n", sizeof(long*)); // x86: 4| x64: 8
	printf("long long %d\n", sizeof(long long*)); // x86: 4| x64: 8
	printf("float %d\n", sizeof(float*)); // x86: 4| x64: 8
	printf("double %d\n", sizeof(double*)); // x86: 4| x64: 8
	return 0;
}
//---------------------运行结果---------------------
int 4
char 4
short 4
long 4
long long 4
float 4
double 4

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 17352)已退出，代码为 0。
按任意键关闭此窗口. . .
```

![image-20230924215613591](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309242156170.png)

#### 7.2.4 强制类型转换:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	// & 是取地址运算符 是升维度的。普通变量加&变一级指针。
	// * 是取值运算符 是降维度的。如果是一级指针前面加*就是普通变量，
	// 如果是二级指针前面加*就是一级指针，二级指针前面两*就是普通变量。
	int p = &a;
	// 强制类型转换
	*(int*)p = a;
	return 0;
}
```

#### 7.2.5 指针类型与变量类型:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	char ch = 'a';
	int* p = &ch;
	printf("--------------------内存地址--------------------\n");
	printf("%p\n", ch);
	printf("%p\n", p);
	printf("------------------------------------------------\n");
	//*p = 123;
	//printf("%d\n", ch);   error：程序报错

	printf("%d\n", ch); // 97
	printf("%d\n", *p); // -858993567
	return 0;
}
//-------------------------运行结果-------------------------
--------------------内存地址--------------------
00000061
0113FCD7
------------------------------------------------
97
-858993567

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 12412)已退出，代码为 0。
按任意键关闭此窗口. . .
```

![image-20230925085653260](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309250856513.png)

#### 7.2.6 野指针和空指针:evergreen_tree: 

指针变量也是变量，是变量就可以任意赋值，不要越界即可 (32位为4字节，64位为8字节)，但是，<font color='red'>任意数值赋值给指针变量没有意义，因为这样的指针就成了野指针</font>，此指针指向的区域是未知 (操作系统不允许此指针指向的内存区域)。所以，<font color='red'>野指针不会直接引起错误，操作野指针的内存区域才会出问题</font>。

```c
#include <stdio.h>

int main(void)
{
	// 不建议将一个变量的值或者值赋值给指针否则就是野指针
	// 野指针 -> 指针变量指向一个未知的内存空间
	// 未知内存空间 并不是这个地址，这个地址的内存空间是未知的
	int* p = 255;
	// 操作野指针对应的内存空间可能报错(也不会报错)
	printf("p = %d\n", *p);
	return 0;
}
//-----------------------运行结果-----------------------
啥也没有
```

查看内存情况debug

![image-20230925090607279](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309250906543.png)

![image-20230925090633936](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309250906103.png)

这块地址是不允许访问的

但是，野指针和有效指针变量保存的都是数值，为了标志此指针变量没有指向任何变量 (空闲可用) ，C语言中，可以把NULL 赋值给此指针，这样就标志此指针为空指针，没有任何指针。

```c
#include <stdio.h>

int main(void)
{
	// #define NULL ((void *)0)
	// 空指针是指内存地址编号为 0 的空间
	int* p = NULL;
	*p = 100;
	printf("%d\n", *p);
	return 0;
}
```

NULL 是一个值为0的宏常量：

```c
#define NULL ((void *)0)
```

#### 7.2.7 万能指针 void*:evergreen_tree: 

void* 指针可以指向任意变量的内存空间：

```c
#include <stdio.h>

int main(void)
{
	int a = 100;
	// int* p = &a;
	// 万能指针可以接收任意类型的变量内存地址
	void* v = &a;
	// 在通过万能指针修改变量的值时，需要找到变量对应的指针类型
	*(int*)v = 200;
	printf("a = %d\n", a);
	// 万能指针在取地址变量时也需要强制类型转换为对应的指针类型
	printf("*(int*)v = %d\n", *(int*)v);
	printf("v = %d\n", v);
	// 万能指针也是64位占8字节84位占4字节，但是指针类型是不确定的
	printf("sizeof(v)万能指针占内存大小：%d\n", sizeof(v));
	return 0;
}
//---------------------运行结果---------------------
a = 200
*(int*)v = 200
v = 5897636
sizeof(v)万能指针占内存大小：4

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 13120)已退出，代码为 0。
按任意键关闭此窗口. . .
```

将其void* 类型指针赋值给其它任何类型的指针不需要强制类型转换

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	void* p = &a;
	int* p1 = p;
	printf("%d\n", *p1);
	return 0;
}
//---------------------运行结果---------------------
10

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 6992)已退出，代码为 0。
按任意键关闭此窗口. . .
```



#### 7.2.8 const修饰的指针变量:evergreen_tree: 

在编辑程序时，指针作为函数参数，如果不想修改指针对应内存空间的值，需要使用const修饰指针数据类型。

基本语法：

```c
const int* p = &a;
//或者
int* const p = &a;
```

##### const 修饰的指针 到底是约束了指针自己还是内存空间呢？:tanabata_tree: 

##### 7.2.8.1 常量指针:palm_tree: 

`const int* p = &a;` 

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	// 常量 指针
	const int* p = &a;
	// 将常量指针存储的地址改为了20.
	// p = 20;
   // 修改常量指针中存储的a变量地址的值
	*p = 20; // ERROR ， 表达式必须是可修改的左值， 常量不可被修改
	printf("变量a的值：%d\n", a); // 10
	printf("常量指针存储的值：%d\n", *p); // 10 如果 p = 20; 修改地址值 则为野指针 值不允许访问
	printf("常量指针内存地址：%p\n", p); // 0053F9D4 如果 p = 20; 修改地址值 则为野指针 值不允许访问
	return 0;
}
```

##### 7.2.8.2 指针常量:palm_tree: 

`int* const p = &a;` 

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	// 常量 指针
	int* const p = &a;
	// 将常量指针存储的地址改为了20.
	p = 20; // ERROR ， 表达式必须是可修改的左值， 常量不可被修改
   // 修改常量指针中存储的a变量地址的值
	// *p = 20;
	printf("变量a的值：%d\n", a); // 20
	printf("常量指针存储的值：%d\n", *p); // 20
	printf("常量指针内存地址：%p\n", p); // 0053F9D4
	return 0;
}
```

![image-20230925101929685](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309251019976.png)

##### 7.2.8.3 常量指针常量:palm_tree: 

`const int* const p = &a;` 指针类型和变量都被const修饰了就不能直接使用p或者*p来进行修改了否则报错。

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	// 常量 指针
	// 修饰指针类型，修饰指针变量
	const int* const p = &a;
	// 将常量指针存储的地址改为了20.
	p = 20; // ERROR ， 表达式必须是可修改的左值， 常量不可被修改
	// 修改常量指针中存储的a变量地址的值
	*p = 20; // ERROR ， 表达式必须是可修改的左值， 常量不可被修改
	return 0;
}
```

##### 7.2.8.4 通过二级指针修改常量指针常量:palm_tree: 

那还能不能修改呢？可以的！普通的常量修饰的变量我们可以使用一级指针进行修改，常量修饰的一级指针我们可以使用二级指针进行修改：

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	int b = 20;
	// 常量 指针
	// 修饰指针类型，修饰指针变量
	const int* const p = &a;
	// 将常量指针存储的地址改为了20.
	// p = 20; // ERROR ， 表达式必须是可修改的左值， 常量不可被修改
	// 修改常量指针中存储的a变量地址的值
	// *p = 20; // ERROR ， 表达式必须是可修改的左值， 常量不可被修改
	// 将一级指针p的地址赋值给二级指针
	int** p1 = &p;
	// 修改二级指针存储的值
	**p1 = 123;
	printf("%p\n", **p1); // 0000007B p1 变量存储的内存地址
	printf("%p\n", &p); // 00BCFC78 p 变量本身的内存地址
	printf("%p\n", p); // 00BCFC84 p 变量中存储的内存地址
	printf("%d\n", *p); // 123  a 变量的值
	printf("%d\n", a); // 123 a 变量的值
	printf("%d\n", b); // 20 b 变量的值
	// *p1 是二级指针的一级指针 *p1 = *p 修改 指向地址为变量 b 的内存地址
	*p1 = &b;
	printf("*p1 = &b：%d\n", *p); // 20  b 变量的值
	printf("*p1 = &b：%d\n", a); // 123 a 变量的值
	// 已知 *p1 存储的是 &b 的内存地址那么 一个*代表一个降级维度
	// 所以 **p1 就是 b 的变量值 对其进行修改 为 123
	**p1 = 123;
	// 上面将*p1 赋值为了 b 变量的值为 20 然后又 通过 **p1 修改为123
	printf("*p1 = &b：-> **p1 = 123：%d\n", *p); // 123  b 变量的值
	// a 变量的值在上面早已被修改
	printf("*p1 = &b：-> **p1 = 123：%d\n", a); // 123 a 变量的值
	return 0;
}
//--------------------------运行结果--------------------------
0000007B
00B3FC38
00B3FC50
123
123
20
*p1 = &b：20
*p1 = &b：123
*p1 = &b：-> **p1 = 123：123
*p1 = &b：-> **p1 = 123：123

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 19116)已退出，代码为 0。
按任意键关闭此窗口. . .
```



#### 7.2.9 指针修改常量的值:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	// 常量类型a = 10;
	const int a = 10;
	// a = 100; // ERROR 常量数据类型不能进行修改否则报错。
	// 可以通过指针来间接性修改常量的值
	int* p = &a;
	*p = 100;
	printf("%d\n", a); // 100
	return 0;
}
//--------------------------运行结果--------------------------
100

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 15728)已退出，代码为 0。
按任意键关闭此窗口. . .
```

### 7.3 指针和数组:deciduous_tree: 

#### 7.3.1 数组名:evergreen_tree: 

数组名字是数组的首元素地址，但它是一个常量：

```c
int arr[] = {1, 2, 3, 4};
printf("a = %p\n", a);
printf("a[0]内存地址 = %p\n", &a[0]);
a = 10;// error 数组名只是常量地址，不能修改
```

#### 7.3.2 指针操作数组元素:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	// 必须是相同类型集合，字符串和int不是同一个类型
	// 字符类型对应ASCII值为：97, 98, 99 是同一个类型 的
	int arr[] = {1231, 2, 3, 4, 5, 6, 7, 8, 'a', 'b', 'c'};
	// 数组名 是一个常量 不允许赋值
	// 数组名 是数组首元素地址
	// arr = 100; ERROR
	// 数组名 本事就是常量地址 不需要加 & 取地址运算符
	int* p = arr;
	*p = 123;
	// arr 是一个数组 44 个字节大小
	printf("%d\n", sizeof(arr)); // 44
	// p 是一个指针变量 4 个字节大小
	printf("%d\n", sizeof(p)); // 4
	printf("arr = %p\n", arr); // 00D3F7F0
	printf("p = %p\n", p); // 00D3F7F0
	printf("*p = %d\n", *p); // 123
	// 已知 *p 可以直接取出 数组首地址的元素1231 那么怎么取下一个索引的值呢
	// 答：*(p + 1) 给指针变量加上一个向后的偏移量就可以获取到下一个索引的值了
	printf("*(p + 1) = %d\n", *(p + 1)); // 2
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(arr[0]);i++)
	{
		// p是变量，arr是常量 所以循环是p可以使用++进行修改+1而arr不行
		printf("arr[%d] = %d\t", i, *p++);
	}
	printf("\n");
	// 指针变量减去arr数组名首地址 循环中p++指针到了数组的最后了
	// 所有指针类型相减结果都是int类型的。
	int step = p - arr;
	printf("p - arr = %d\n", step); // 11
	return 0;
}
//----------------------------运行结果----------------------------
44
4
arr = 00DAF9D0
p = 00DAF9D0
*p = 123
*(p + 1) = 2
arr[0] = 123    arr[1] = 2      arr[2] = 3      arr[3] = 4      arr[4] = 5      arr[5] = 6      arr[6] = 7      arr[7] = 8      arr[8] = 97     arr[9] = 98     arr[10] = 99
p - arr = 11

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 6372)已退出，代码为 0。
按任意键关闭此窗口. . .
```

![image-20230925133208960](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309251332309.png)

#### 7.3.3 数组当参数传入退化为指针:evergreen_tree: 

```c
#include <stdio.h>

extern void sort(int);

int main(void)
{
	int arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
	sort(arr);
	return 0;
}
 // 数组作为参数会退化为指针 丢失数组的精度
void sort(int arr[])
{
   // 指针是变量 4 个字节 arr[0] 获取数组中索引为0的元素 int 类型 4个字节
	// 4 / 4 = 1
	printf("sizeof(arr)/ sizeof(arr[0])：%d\n", sizeof(arr)/ sizeof(arr[0])); // 1
	printf("sizeof(arr)：%d\n", sizeof(arr)); // 4
}
//-----------------------运行结果-----------------------
sizeof(arr)/ sizeof(arr[0])：1
sizeof(arr)：4

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 7912)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 7.3.3.1 指针方式编写冒泡排序:palm_tree: 

```c
#include <stdio.h>

extern void sort1(int);

int main(void)
{
	int arr[] = {-1, 2, 3, 1, -2, -3, 5, 4, 6, 8, 9, 7};
	sort1(arr, sizeof(arr)/ sizeof(arr[0]));
	int* p = arr;
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(arr[0]);i++)
	{
		printf("arr[%d] = %d\t", i, *(p + i));
	}
	return 0;
}
 // 编写冒泡排序的算法
void sort1(int* arr, int len)
{
	int flag = 1;
	int i = 0;
	for (;i < len - 1;i++)
	{
		int j = 0;
		for (;j < len - i - 1;j++)
		{
			if (*(arr + j) > *(arr + j + 1))
			{
				flag = 0;
				int temp = *(arr + j + 1);
				*(arr + j + 1) = *(arr + j);
				*(arr + j) = temp;
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
//---------------------运行结果---------------------
arr[0] = -3     arr[1] = -2     arr[2] = -1     arr[3] = 1      arr[4] = 2      arr[5] = 3      arr[6] = 4      arr[7] = 5      arr[8] = 6      arr[9] = 7      arr[10] = 8     arr[11] = 9
E:\C\Demo\Project3\指针\Debug\指针.exe (进程 20376)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.3.4 指针加减运算:evergreen_tree: 

##### 7.3.4.1 加法运算:palm_tree: 

-  <font color='red'>指针计算不是简单的整型相加</font>.
-  如果是一个int* ，+1 的结果是增加一个 int(4个字节) 的大小
-  如果是一个char* ，+1 的结果是增加一个 char(一个字节) 的大小

```c
#include <stdio.h>

int main(void)
{
	int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	int* p = &arr[-3];
	printf("%p\n", p); //   00CFFBBC
	printf("%p\n", arr); // 00CFFBC8
	// p 跟 arr 相差了 3 步 一个 4 个字节
	//     将指针加3步
	//      00CFFBBC
	p++; // 00CFFBC2
	p++; // 00CFFBC4
	p++; // 00CFFBC8
	// 此时的指针p 与 arr 地址就相同了
	printf("%p\n", p); //   00CFFBC8
	printf("%p\n", arr); // 00CFFBC8
	return 0;
}
//-------------------运行结果-------------------
003CFE0C
003CFE18
003CFE18
003CFE18

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 16040)已退出，代码为 0。
按任意键关闭此窗口. . .
```



##### 7.3.4.2 减法运算:palm_tree: 

-  <font color='red'>指针计算不是简单的整型相减</font>.
-  如果是一个int* ，-1 的结果是减少一个 int(4个字节) 的大小
-  如果是一个char* ，-1 的结果是减少一个 char(一个字节) 的大小

```c
int main(void)
{
	int arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};
	int* p = &arr[3];
	printf("%p\n", p); // 0097FE98
	printf("%p\n", arr); // 0097FE8C
	// 内存地址相差是 12 / sizeof(int) 4个字节 = 偏移量
	int step = p - arr;
	printf("step = %d\n", step); // 12 / 4 = 3
	printf("arr[-1] = %d\n", arr[-1]); // 下标越界
	// 指针操作数组时下标允许是负数
	printf("p[-2] = %d\n", p[-2]); // 2

	p--; // 0097FE94 指针的加减运算和指针的类型有关 int 类型 4 个字节
	p--; // 0097FE90
	p--; // 0097FE8C
	printf("%p\n", p); // 0097FE8C
	printf("%p\n", arr); // 0097FE8C
	// 上面p-- x3 就将偏移量去除了
	int stepp = p - arr;
	printf("偏移量消除后step = %d\n", stepp); // 0
	
	printf("------------------------------\n");

	int arr1[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	char* p1 = arr;
	p1 = &arr1[3];
	printf("%p\n", p1); // 0097FE98
	printf("%p\n", arr1); // 0097FE8C
	// 指针内存地址相差12 / sizeof(char)1个字节 = 偏移量
	int step1 = p1 - arr1;
	printf("%d\n", step1); // 12 / 1 = 12
	p1--; // 0097FE97 指针的加减运算和指针的类型有关 char 类型 1 个字节
	p1--; // 0097FE96
	p1--; // 0097FE95
	printf("%p\n", p1); // 0097FE95
	printf("%p\n", arr1); // 0097FE8C
	// 指针内存地址相差12 - 3 = 9 / sizeof(char)1个字节 = 9
	int stepp1 = p1 - arr1;
	printf("偏移量消除后step = %d\n", stepp1); // 9
	return 0;
}
//---------------------------运行结果---------------------------
010FFA90
010FFA84
step = 3
arr[-1] = -858993460
p[-2] = 2
010FFA84
010FFA84
偏移量消除后step = 0
------------------------------
010FFA40
010FFA34
12
010FFA3D
010FFA34
偏移量消除后step = 9

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 4800)已退出，代码为 0。
按任意键关闭此窗口. . .
```



#### 7.3.4.2 字符串的拷贝逐步简化版本:evergreen_tree: 

```c
#include <stdio.h>

extern void my_strcopy(char, char);
// 简化书写版
extern void my_strcopy2(char, char);
// 简化书写版升级!
extern void my_strcopy3(char, char);

int main(void)
{
	// 字符串拷贝操作
	//partone------------------------------------
	char ch[] = "hello world !";
	char dest[100];
	my_strcopy(dest, ch);
	printf("my_strcopy：%s\n", dest);
	//parttwo------------------------------------
	char ch1[] = "hello world !";
	char dest1[100];
	my_strcopy2(dest1, ch1);
	printf("my_strcopy2：%s\n", dest);
	//partthree------------------------------------
	char ch2[] = "hello world !";
	char dest2[100];
	my_strcopy3(dest2, ch2);
	printf("my_strcopy3：%s\n", dest);
	return 0;
}

void my_strcopy(char* dest, char* ch)
{
	int i = 0;
	// 如果字符串末尾\0 也等同于 0 while循环停止
	while (*(ch + i))
	{
		*(dest + i) = *(ch + i);
		i++;
	}
	*(dest + i) = 0;
}
// 简化书写
void my_strcopy2(char* dest, char* ch)
{
	while (*ch)
	{
		*dest = *ch;
		dest++;
		ch++;
	}
	*dest = 0;
}
// 终极简化版本
void my_strcopy3(char* dest, char* ch)
{
	while (*(dest++) = *(ch++));
	/* 程序的执行流程 ->>表达式拆分解析<<- (分析运算符的优先级) * ， = > ++
	* 第一步： 先*ch *dest取出值 
	* 第二步： 执行 = 将*ch的值赋值给 *dest
	* ->第三步： 进行while的条件判断，先赋值后判断 ，字符串的最后赋值0判断结束循环
	* 第四步： 执行 dest++ ，ch++
	*/
}
//----------------------------运行结果----------------------------
my_strcopy：hello world !
my_strcopy2：hello world !
my_strcopy3：hello world !

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 18628)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.3.5 指针和运算符的操作:evergreen_tree: 

指针不能使用：加，乘，除，取余 可以使用 减，和其它的逻辑运算符和比较运算符

```c
#include <stdio.h>

// 指针和运算符的操作
int main(void)
{
	int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
	int* p = &arr[3];
	// 两个指针相加就会报错 一定是一个 野指针
	// p = p + arr;  error C2110: “+”: 不能添加两个指针
	// p = p * arr; error C2297 : “ * ” : 无效，因为右操作数的类型为“int * ”
	// p = p / arr; error C2296 : “ / ” : 无效，因为左操作数的类型为“int * ”
	// p = p % arr; error C2296 : “ % ” : 无效，因为左操作数的类型为“int * ”
	if (p > arr)
	{
		printf("真\n");
	}
	else
	{
		printf("假\n");
	}
	int* p1 = 100;
	int res = p1 - arr;
	// 野指针减去arr可以但是没有意义，证明指针可以使用减法
	printf("%d\n", res); // -3620364
	return 0;
}
//-------------------------------运行结果-------------------------------
真
-1310548

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 19912)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.3.6 指针数组:evergreen_tree: 

指针数组，它是数组，数组的每个元素都是指针类型

![image-20230925170606645](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309251706258.png)

```c
#include <stdio.h>

int main(void)
{
	// 定义数组  数据类型  数据名[元素个数] = {值1，值2，...}
	// 定义指针数组
	int a = 10;
	int b = 20;
	int c = 30;
	int* arr[] = {&a, &b, &c};
	printf("%d\n", *arr[0]); // 10
	printf("%d\n", sizeof(arr)); // 12
	printf("%d\n", sizeof(*arr)); // 4
	printf("%d\n", sizeof(*arr[0])); // 4
	// arr[0] 获取的是指针类型，84位都是4字节 64位都是8字节
	printf("%d\n", sizeof(arr[0])); // 4
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(*arr[0]);i++)
	{
		printf("arr[%d] = %d\n", i, *arr[i]);
	}
	return 0;
}
//-----------------------------运行结果-----------------------------
10
12
4
4
4
arr[0] = 10
arr[1] = 20
arr[2] = 30

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 19344)已退出，代码为 0。
按任意键关闭此窗口. . .
```

另一种格式

![image-20230925175142856](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309251751388.png)

```c
#include <stdio.h>

int main(void)
{
	// 指针数组里面元素存储的是指针
	int a[] = { 1, 2, 3 };
	int b[] = { 4, 5, 6 };
	int c[] = { 7, 8, 9 };
	// 指针数组对应与二级指针
	int* arr[] = { a, b, c };
	int** p = arr;
	// arr是指针数组的首地址
	printf("%p\n", arr); // 0053F80C
	printf("%p\n", &arr[0]); // 0053F80C
	printf("%p\n", arr[0]); // 0053F848
	printf("%p\n", a); // 0053F848
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(*arr[0]); i++)
	{
		int j = 0;
		for (;j < sizeof(arr)/ sizeof(*arr[0]);j++)
		{
			// printf("arr[%d] = %d\n", i, *arr[i] + j);
			printf("arr[%d] = %d\n", i, *(*(arr + i) + j));
		}
	}
	return 0;
}
//-------------------------------运行结果-------------------------------
00BCF8F4
00BCF8F4
00BCF930
00BCF930
arr[0] = 1
arr[0] = 2
arr[0] = 3
arr[1] = 4
arr[1] = 5
arr[1] = 6
arr[2] = 7
arr[2] = 8
arr[2] = 9

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 10292)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.3.7 指针数组和二级指针建立关系:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	int a[] = { 1, 2, 3 };
	int b[] = { 4, 5, 6 };
	int c[] = { 7, 8, 9,10 };
	int* arr[] = { a, b, c };
	// 指针数组和二级指针建立关系
	int** p = arr;
	printf("%p\n", arr); // 006BFE74
	printf("%p\n", arr[0]); // 006BFEB0
	printf("%d\n", arr[0][0]); // 1
	printf("%p\n", p); // 006BFE74
	printf("%p\n", *p); // 006BFEB0
	printf("%d\n", **p); // 1
	printf("---------------\n"); // 1
	// 一级指针加偏移量 相当于跳过了一个一元素
	// 二级指针加偏移量 相当于跳过了一个一维数组大小
	printf("%d\n", *(*p) + 1); // 2
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(arr[0]); i++)
	{
		int j = 0;
		for (;j < sizeof(arr)/ sizeof(arr[0][0]); j++)
		{
			printf("arr[%d] = %d\t", i, *(*(p + i) + j));
		}
		printf("\n");
	}
	return 0;
}
//-------------------------运行结果-------------------------
006FF9FC
006FFA3C
1
006FF9FC
006FFA3C
1
---------------
2
arr[0] = 1      arr[0] = 2      arr[0] = 3
arr[1] = 4      arr[1] = 5      arr[1] = 6
arr[2] = 7      arr[2] = 8      arr[2] = 9

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 8740)已退出，代码为 0。
按任意键关闭此窗口. . .
```



### 7.4 多级指针:deciduous_tree: 

-  C语言允许有多级指针存在，在实际的程序中一级指针最常用，其次是二级指针。
-  二级指针就是指向一个一级指针变量地址的指针。

![image-20230925203527968](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309252035539.png)

```c
#include <stdio.h>

int main(void)
{
	int a = 10;
	int b = 20;
	int* p = &a;
	int** pp = &p; 
	int*** ppp = &pp;
	/**
	* *p = a
	* **pp = *p = a
	* ***ppp = **pp = *p = a
	*/
	*pp = &b; // 等价于*p = &b
	**pp = 100; // 这并不是野指针，等价于 *p = 100 也等价于 b = 100
	// *pp = 100; // ERROR 属于野指针，改变了 *p 的地址 造成了未知内存
	printf("%d\n", *p); // 100
	**ppp = &a; // 等价于 *p = &a
	***ppp = 200; // 这并不是野指针，等价于 *p = 200 也等价于 a = 200
	printf("%d\n", ***ppp); // 200
	printf("%p\n", *ppp); // 输出 **pp 的内存地址：010FFE80
	return 0;
}
//--------------------------运行结果--------------------------
100
200
007EF92C

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 1056)已退出，代码为 0。
按任意键关闭此窗口. . .
```

###  7.5 指针和函数:deciduous_tree: 

#### 7.5.1 函数形参改变实参的值:evergreen_tree: 

 ```c
 #include <stdio.h>
 
 extern void swap(int, int);
 extern void swap1(int, int);
 
 int main(void)
 {
 	int a = 10;
 	int b = 20;
 	// 值传递 形参不影响实现的值
 	swap(a, b);
 	printf("a = %d, b = %d\n", a, b);
 
 	printf("------------------\n");
 
 	int a1 = 10;
 	int b1 = 20;
 	// 地址传递 形参的改变会影响到实参的值
 	swap1(&a1, &b1);
 	printf("a1 = %d, b1 = %d\n", a1, b1);
 	return 0;
 }
 
 void swap(int a, int b)
 {
 	int temp = a;
 	a = b;
 	b = temp;
 }
 // 指针作为函数参数
 void swap1(int* a, int* b)
 {
 	int temp = *a;
 	*a = *b;
 	*b = temp;
 }
 //---------------------------运行结果---------------------------
 a = 10, b = 20
 ------------------
 a1 = 20, b1 = 10
 
 E:\C\Demo\Project3\指针\Debug\指针.exe (进程 16436)已退出，代码为 0。
 按任意键关闭此窗口. . .
 ```

![image-20230925211143681](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309252111081.png)

#### 7.5.2 字符串拼接指针版:evergreen_tree: 

```c
#include <stdio.h>

extern void  my_strcat(char*, char*);

int main(void)
{
	char ch[100] = "hello";
	char ch1[] = "world !!!";
	my_strcat(ch, ch1);
	printf("%s\n", ch);
	return 0;
}

void my_strcat(char* ch1, char* ch2)
{
	// 循环判断是否达到字符串的结尾 进行ch1++ 访问到最后
	while (*ch1)ch1++;
	// 这里比较特殊看似简单其实执行了四个步骤
	while (*ch1++ = *ch2++);
	/** 分析判断表达式的执行步骤 (>>优先级<<)
	*   第一步：先取出 ch1 与 ch2 的值
	*   第二步：将ch2 的值 赋值 给 ch1
	*   第三步：判断是否满足条件这里判断的是其中一个是否为 0
	*   第四步：各自进行++操作，因为这是后++，在第一次赋值时没有进行++直接
	*   赋值，第二次进行++，所以说最后一个多走一步将其赋值为 \0 这样防止了乱码
	*/
}
//-----------------------运行结果-----------------------
helloworld !!!

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 10048)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.5.3 字符串去除空格:evergreen_tree: 

```c
#include <stdio.h>

extern char* remove_str_space(char*);
extern void remove_str_space1(char*);

int main(void)
{
	char ch[] = "      h   e   ll   o wr o  l   d ! ! ";
	char* p = remove_str_space(ch);
	printf("p = %s\n", p);
	// 地址传递修改原ch的值
	remove_str_space1(ch);
	printf("ch = %s\n", ch);
	return 0;
}

char* remove_str_space(char* str)
{
	char str_new[100] = "";
	printf("str_new[0] 内存地址： %p\n", str_new[0]);
	char* p = str_new;
	while (*str)
	{
		if (*str <= 122 && *str >= 97 || *str <= 90 && *str >= 65 || *str == 33)
		{
			*p++ = *str;
		}
		str++;
	}
	printf("*p 内存地址：%p\n", *p);

	return str_new;
}

void remove_str_space1(char* str)
{
	char* first = str;
	char* last = str;
	while (*last)
	{
		if (*last != ' ')
		{
			*first = *last;
			first++;
		}
		last++;
	}
	*first = '\0';
}
//-----------------------------运行结果-----------------------------
str_new[0] 内存地址： 00000000
*p 内存地址：00000000
p = hellowrold!!
ch = hellowrold!!

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 6388)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.5.4 指针作为函数的返回值:evergreen_tree: 

```c
#include <stdio.h>

extern char* search(char*, char);

int main(void)
{
	char ch[] = "hello world";
	printf("ch[3] = %p\n", ch[3]);
	char* p = search(ch, 'l');
	// printf("%p\n", *p); //ERROR 如果返回值为NULL则取值就会报错 程序无法往下正常执行
	// 判断p是否为NULL
	if (!p == NULL)
	{
		//判断找到的字符的地址是否是我们想要的
		if (*p == ch[3])
		{
			printf("p = %p\n", *p);
		}
		else
		{
			printf("这是一个不符合规则的地址");
		}
	}
	else
	{
		printf("没有找到\n");
	}
	return 0;
}
// 查找字符数组中某个字符地址函数
char* search(char* str, char ch)
{
	while (*str)
	{
		if (*str++ == ch)
		{
			return &*str;
		}
	}
	return NULL;
}
//---------------------------运行结果---------------------------
ch[3] = 0000006C
p = 0000006C

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 18712)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.5.5 字符串查找字符串:evergreen_tree: 

```c
#include <stdio.h>

extern char* my_strsearch(char*, char*);

int main()
{
	char ch[] = "helle worldllo";
	char dest[] = "llo";
	char* p = my_strsearch(ch, dest);
	printf("%s\n", p);
	return 0;
}

char* my_strsearch(char* src, char* dest)
{
	char* fsrc = src; // 遍历原字符串的指针
	char* rsrc = src; // 记录每次相同字符串的首地址
	char* tdest = dest; // 记录dest的指针，不满足时回滚
	while (*fsrc)
	{
		// 记录每次fsrc的指针地址
		rsrc = fsrc;
		// 这里的判断中不能缺少 单独判断 *fsrc或 *tdest 否则就会越界返回错误信息
		while (*fsrc == *tdest && *fsrc && *tdest)
		{
			// 每次相同指针后移判断下一个
			fsrc++;
			tdest++;
		}
		// 判断*tdest指针是否走到了最后了
		if (*tdest == '\0')
		{
			// 返回rsrc指针位置往后的字符串
			return rsrc;
		}
		// 指针回滚
		tdest = dest; 
		fsrc = rsrc;
		// 指针回滚后，fsrc指针前移一步因为当前判断过了
		fsrc++;
	}
	return NULL;
}
//------------------------------------运行结果------------------------------------
llo

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 12520)已退出，代码为 0。
按任意键关闭此窗口. . .
```

### 7.6 指针和字符串:deciduous_tree: 

#### 7.7.1 字符指针:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	char ch[] = "hello world"; // 栈区字符串
	// "hello world"存放于 数据区常量区 数据是不可修改的
	char* p = "hello world"; // 数据区产量区字符串
	// 相同的字符串 "hello world" 指针 指向用一个内存地址，一个内存可以可以被多个指针指向
	char* p1 = "hello world"; // 数据区产量区字符串
	// printf("hello world");数据区
	printf("%p\n", p); // 00BBAC64
	printf("%p\n", p1); // 00BBAC64
	ch[2] = 'm';
	// p[2] = 'm'; ERROR
	// *(p + 2) = 'm'; ERROR
	printf("%s\n", ch);
	printf("%s\n", p);
	return 0;
}
//-----------------------------运行结果-----------------------------
0008AC64
0008AC64
hemlo world
hello world

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 22316)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.7.2 指针和字符串:evergreen_tree: 

```c
int main(void)
{
	// 字符串数组
	// 指针数组
	char ch[] = "hello";
	char ch1[] = "world";
	char ch2[] = "des";
	// 字符数组，可以修改
	char* arr[] = { ch, ch1, ch2 };
	// 常量字符串，不可以修改 称为 字符串数组
	char* arr1[] = { "hello", "world", "des" };

	int len = sizeof(arr) / sizeof(arr[0]);
	int flag = 1;
	int i = 0;
	for (; i < len; i++)
	{
		int j = 0;
		for (; j < len - 1 - i; j++)
		{
			// 根据数组中每一个字符串的首字母进行比较判断
			if (arr1[j][0] > arr1[j + 1][0])
			{
				// flag判断是否停止工作
				flag = 0;
				// 交换位置
				int temp = arr1[j + 1];
				arr1[j + 1] = arr1[j];
				arr1[j] = temp;
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
	int i1 = 0;
	for (;i1 < sizeof(arr1)/ sizeof(arr1[0]);i1++)
	{
		printf("arr1[%d] = %s\n", i1, arr1[i1]);
		/**
		* des
		* hello
		* world
		*/
	}

	int i2 = 0;
	for (; i2 < sizeof(arr1) / sizeof(arr1[0]); i2++)
	{
		printf("arr1 -> p = %p\n", arr1[i2]);
		/** 它们之间的地址是不连续的
		* 001DBB44
		* 001DBC34
		* 001DBD00
		*/
	}
	return 0;
}
//-------------------------运行结果-------------------------
arr1[0] = des
arr1[1] = hello
arr1[2] = world
arr1 -> p = 00A4BD08
arr1 -> p = 00A4BC34
arr1 -> p = 00A4BD00

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 17428)已退出，代码为 0。
按任意键关闭此窗口. . .
```

![image-20230926143122095](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309261431345.png)

#### 7.7.3 有效字符串:evergreen_tree: 

```c
#include <stdio.h>

extern int my_strlen(char*);

int main(void)
{
	char ch[] = "hello   world";
	int len = my_strlen(ch);
	printf("字符串的有效长度：%d\n", len);
	return 0;
}

int my_strlen(char* ch)
{
	int i = 0;
	char* temp = ch;
	while (*temp) if (*temp == ' ') { i++; temp++; }else temp++;;
	return temp - ch - i;
}
//-----------------------------------运行结果-----------------------------------
字符串的有效长度：10

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 21028)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.7.4 const修饰指针变量:evergreen_tree: 

```c
#include <stdio.h>

int main(void)
{
	char ch[] = "hello";
	char ch1[] = "world";
	// 指向常量的指针
	// 可以修改指针变量的值，不可以修改指针变量指向存储空间的值
	const char* p = ch;
	// *p = 'm'; // ERROR
	// *(p + 1) = 'm'; // ERROR
	// p[1] = 'm'; // ERROR
	p = ch1; // success
	printf("p = %s\n", p);

	char* const p1 = ch;
	*p1 = 'a';
	*(p1 + 1) = 'b';
	p1[2] = 'c';
	// p1 = ch1; // ERROR
	printf("p1 = %s\n", p1);
	return 0;
}
//------------------------------运行结果------------------------------
p = world
p1 = abclo

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 19260)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 7.7.4.1 通过多级指针修改 常量指针常量:palm_tree: 

```c
#include <stdio.h>

int main(void)
{
	char ch[] = "hello";
	char ch1[] = "world";
	// 常量指针
	// 指向常量的指针
	// 可以修改指针变量的值，不可以修改指针变量指向存储空间的值
	const char* p = ch;
	// *p = 'm'; // ERROR
	// *(p + 1) = 'm'; // ERROR
	// p[1] = 'm'; // ERROR
	p = ch1; // success
	printf("p = %s\n", p);

	// 指针常量
	// 可以修改指针变量指向内存空间的值
	// 不可以修改指针变量的值
	char* const p1 = ch;
	*p1 = 'a';
	*(p1 + 1) = 'b';
	p1[2] = 'c';
	// p1 = ch1; // ERROR
	printf("p1 = %s\n", p1);

	// 常量指针常量
	const char* const p2 = ch1;
	// *p2 = 'a'; // ERROR
	// *(p2 + 1) = 'b'; // ERROR
	// p2[2] = 'c'; // ERROR
	// p2 = ch1; // ERROR

	// 通过二级指针修改 一级的常量指针常量 或者 修改变量的值
	const char** const pp = &p2;
	// **pp = 'a';
	// *(*pp + 1) = 'b';
	//*pp[2] = 'c';
	//printf("pp = %s\n", *pp);
	
	// 通过三级指针修改 二级的常量指针常量 或者 修改变量的值
	char*** ppp = &pp;
	***ppp = 'a';
	*(*(*ppp) + 1) = 'b';
	// const char* const p2 = ch1; 这种方式通过一级指针修改的
	// 但是一级指针为const所以这个方式不可用，但*(*(*ppp) + 1) = 'b';
	// 可以达到相同效果
	**ppp[2] = 'c';
	printf("%s\n", **ppp);
	return 0;
}
//-------------------------------运行结果-------------------------------
p = world
p1 = abclo

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 16580)已退出，代码为 -1073741819。
按任意键关闭此窗口. .
```

#### 7.7.5 指针数组作为main函数的形参:evergreen_tree: 

```c
int main(int argc, char* argv[]){};
```

-  main 函数是操作系统调用的，第一个参数标明 argc 数组的成员数量，argv 数组的每个成员都是char * 类型
-  argv 是命令行参数的字符串数组
-  argc 代表命令行参数的数量，程序名字本身算一个参数

```c
#include <stdio.h>

// >> gcc -o hello.exe hello.c 传递的参数的个数是4
// 分别是 ("gcc" "-o" "hello.exe" "hello.c")
// argc 表示传递参数的个数
// argv 分别存储的是{ "gcc", "-o", "hello.exe", "hello.c" }
int main(int argc, char* argv[])
{
	int i = 0;
	for (;i < argc;i++)
	{
		printf("%s\n", argv[i]);
	}
	return 0;
}
```

打开cmd进行编译.c文件 

<font color='red'>提示</font>：如果出现了如下的错误提示，则需要使用 -std=c99来进行编译就不会报错了。

![image-20230926160000442](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309261600849.png)

![image-20230926160101299](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309261601622.png)

编译完成！

进行如下操作：执行.exe可执行程序 后面跟上 三句话 ，如果空格分隔会被解析为下一个参数，最终输出三个参数的语句。

![image-20230926160350649](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309261603881.png)

可以对其进行一些判断限制

```c
#include <stdio.h>

// >> gcc -o hello.exe hello.c 传递的参数的个数是4
// 分别是 ("gcc" "-o" "hello.exe" "hello.c")
// argc 表示传递参数的个数
// argv 分别存储的是{ "gcc", "-o", "hello.exe", "hello.c" }
int main(int argc, char* argv[])
{
	if (argc < 3)
	{
		printf("缺少参数：至少3个以上 !");
		return -1;
	}
	int i = 0;
	for (;i < argc;i++)
	{
		printf("%s\n", argv[i]);
	}
	return 0;
}
```

执行可执行程序的路径到这个文件 也算是一个参数

![image-20230926161020670](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309261610831.png)

#### 7.7.6 项目开发常用字符串应用模型:evergreen_tree: 

##### 1 strstr中的while和do-while模型:palm_tree: 

利用strstr标准库函数找出一个字符串中substr出现的个数。

###### a) while模型:tanabata_tree: 

```c
#include <stdio.h>

extern char* my_strSearchstr(char*, char*);

int main(void)
{
	char str[] = "11abcd111122abcd333abcd3322abcd3333322qqq";
	char ch1[] = "abcd";
	char* p = my_strSearchstr(str, ch1);
	int count = 0;
	while (p)
	{
		count++;
		p += strlen(ch1);
		p = my_strSearchstr(p, ch1);
		printf("判断第%d次，p = %s\n", count, p);
	}
	printf("出现的次数：%d\n", count);
	return 0;
}
// 通过dest查找src匹配的字符串
char* my_strSearchstr(char* src, char* dest)
{
	char* fsrc = src;
	char* tsrc = src;
	char* fdest = dest;
	while (*fsrc)
	{
		tsrc = fsrc;
		while (*fsrc == *fdest && *fsrc && *fdest)
		{
			fsrc++;
			fdest++;
		}
		if (*fdest == '\0')
		{
			return tsrc;
		}
		// 指针回滚
		fsrc = tsrc;
		fdest = dest;
		fsrc++;
	}
	return NULL;
}
//----------------------------运行结果----------------------------
判断第1次，p = abcd333abcd3322abcd3333322qqq
判断第2次，p = abcd3322abcd3333322qqq
判断第3次，p = abcd3333322qqq
判断第4次，p = (null)
出现的次数：4

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 19836)已退出，代码为 0。
按任意键关闭此窗口. . .
```

###### b) do-while模型:tanabata_tree: 

```c
#include <stdio.h>

extern char* strSearchstrtwo(char*, char*);

int main(void)
{
	char str[] = "11abcd111122abcd333abcd3322abcd3333322qqq";
	char ch1[] = "abcd";
	char* p = strSearchstrtwo(str, ch1);
	int count = 0;
	do
	{
		// 由于do-while会无条件先指向，防止错误用if来判断一下
		if (p)
		{
			count++;
			p += strlen(ch1);
			p = strSearchstrtwo(p, ch1);
			printf("判断第%d次：%s\n", count, p);
		}
	} while (p);
	printf("出现的次数：%d\n", count);
	return 0;
}

char* strSearchstrtwo(char* src, char* dest)
{
	char* fsrc = src;
	char* tsrc = src;
	char* fdest = dest;
	while (*fsrc)
	{
		tsrc = fsrc;
		while (*fsrc == *fdest && *fsrc && *fdest)
		{
			fsrc++;
			fdest++;
		}
		if (*fdest == '\0')
		{
			return tsrc;
		}
		fsrc = tsrc;
		fdest = dest;
		fsrc++;
	}
	return NULL;
}
//-------------------------------运行结果-------------------------------
判断第1次：abcd333abcd3322abcd3333322qqq
判断第2次：abcd3322abcd3333322qqq
判断第3次：abcd3333322qqq
判断第4次：(null)
出现的次数：4

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 9556)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 2 两头堵模型:palm_tree: 

```c
#include  <stdio.h>

extern int getCount(char*);

int mcoacoicon(void)
{
	char ch[] = " he l l   o   w orld  ! ";
	int count = getCount(ch);
	printf("个数为：%d\n", count);
	return 0;
}

int getCount(char* src)
{
	int i = 0;
	char* temp = src;
	while (*temp)if (*temp == ' ') { i++; temp++; }else temp++;
	return temp - src - i;
}
//--------------------------运行结果--------------------------
个数为：11

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 3856)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 3 计算每个字符出现的次数:palm_tree: 

```c
#include <stdio.h>

int main(void)
{
	char ch[] = "h hl lo abb worl  dni chou s hachouniz ad izaichouyigeshishi";
	 
	int ch1[26] = { 0 };
	int i = 0;
	int space = 0;
	while (ch[i])
	{
		if (ch[i] != ' ')
		{
			ch1[ch[i] - 'a']++;
		}
		else if(ch[i] == ' ')
		{
			space++;
		}
		i++;
	}
	int j = 0;
	int space_out = 0;
	for (; j < 26; j++)
	{
		if (ch1[j] != 0)
		{
			if (space > 0 && space_out == 0)
			{
				printf("空格出现次数：%d\n", space);
				space_out++;
			}
			else
			{
				printf("字母: %c 出现次数：%d\n", j + 'a', ch1[j]);
			}
		}
	}
	return 0;
}
//------------------------------运行结果------------------------------
空格出现次数：11
字母: b 出现次数：2
字母: c 出现次数：3
字母: d 出现次数：2
字母: e 出现次数：1
字母: g 出现次数：1
字母: h 出现次数：8
字母: i 出现次数：7
字母: l 出现次数：3
字母: n 出现次数：2
字母: o 出现次数：5
字母: r 出现次数：1
字母: s 出现次数：3
字母: u 出现次数：3
字母: w 出现次数：1
字母: y 出现次数：1
字母: z 出现次数：2

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 16252)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 4 字符串反转模型(逆置):palm_tree: 

![image-20230926201705364](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309262017621.png)

```c 
#include <stdio.h>

extern void inver(char*);
extern void inver1(char*);

int main(void)
{
	char ch[] = "hello world";
	inver1(ch);
	printf("%s\n", ch);
	return 0;
}

void inver(char* src)
{
	int i = 0;
	int j = strlen(src) - 1;
	while (i < j / 2)
	{
		char ch = src[i];
		src[i] = src[j];
		src[j] = ch;
		i++;
		j--;
	}
}

void inver1(char* src)
{
	char* fir = src;
	char* len = src + strlen(src) - 1;
	printf("%p\n", fir); // 004FF80C
	printf("%p\n", len); // 004FF816
	// 循环遍历内存地址从h到d
	while (fir < len)
	{
		char ch = *len;
		*len = *fir;
		*fir = ch;
		fir++;
		len--;
	}
}
//-----------------------------------运行结果-----------------------------------
0133F92C
0133F936
dlrow olleh

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 17920)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 5 回文字符串:palm_tree: 

```c
#include <stdio.h>

// 回文字符串
extern int isSymm(char*);

int main(void)
{
	char ch[] = "abcbae";
	int flag = isSymm(ch);
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
// 回文字符串
int isSymm(char* src)
{
	// 字符串的头地址
	char* first = src;
	// 字符串的尾地址
	char* last = src + strlen(src) - 1;
	// 循环遍历内存地址从头到尾
	while (first < last)
	{
		if (*first != *last)
		{
			return 0;
		}
		first++;
		last--;
	}
	return 1;
}
//-----------------------------运行结果-----------------------------
no
E:\C\Demo\Project3\指针\Debug\指针.exe (进程 9120)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 7.7.7 字符串处理函数:evergreen_tree: 

##### 1 strcpy():palm_tree: 

```c
#include <string.h>
char* strcpy(char* dest, const char* src);
功能：把src所指向的字符串复制到dest所指向的空间中，'\0' 也会拷贝过去
参数：
   dest：目的字符串首地址
   src：源字符首地址
返回值：
   成功：返回dest字符串的首地址
   失败：NULL
```

<font color='red'>注意</font>：如果参数dest所指的内存空间不够大，可能会造成缓冲溢出的错误情况。

```c
#include <stdio.h>

extern void my_strcpy(char*, char*);

int main()
{
	char ch[] = "hello world";
	char str[100];
	// 将ch的字符串拷贝到str中，但是要保证str有足够的空间
	strcpy(str, ch);
   my_strcpy(str, ch);
	printf("strcpy = %s\n", str);
   printf("my_strcpy = %s\n", str);
	return 0;
}

void my_strcpy(char* src, char* dest)
{
	while (*src++ = *dest++);
}
//-------------------------运行结果-------------------------
strcpy = hello world
my_strcpy = hello world
   
E:\C\Demo\Project3\指针\Debug\指针.exe (进程 4020)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 2 strncpy():palm_tree: 

```c
#include <string.h>
char* strncpy(char* dest, const char* src, size_t n);
功能：把src指向字符串的前n个字符复制到dest所指向的空间中，是否拷贝结束符看指定的长度是否包含 '\0'
参数：
   dest：目的字符串首地址
   src：源字符首地址
   n：指定需要拷贝字符串个数
返回值：
   成功：返回dest字符串的首地址
   失败：NULL
```

<font color='red'>注意</font>：如果参数dest所指的内存空间不够大，可能会造成缓冲溢出的错误情况。

```c
#include <stdio.h>

extern void my_strncpy(char*, char*, int);
extern void my_strncpy1(char*, char*, int);

int main()
{
	char ch[] = "helloworld";
	char str[100] = "";
	// strncpy(str, ch, 5);
	my_strncpy(str, ch, 5);
	printf("my_strncpy = %s\n", str);
	return 0;
}

void my_strncpy(char* src, char* dest, int v)
{   
	while ((*src++ = *dest++) && --v);
	// 运行结果my_strncpy =  hello
}

void my_strncpy1(char* src, char* dest, int v)
{
	// char ch[] = "helloworld";
	// --v：hell    先-- 后 && 最后 *dest
	// v--：hello   先 v 后 && 然后 *dest 最后 --
	while (v-- && *dest)
	{
		*src = *dest;
		src++;
		dest++;
	}
}
//---------------------------运行结果---------------------------
my_strncpy = hello

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 17552)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 3 strcat():palm_tree: 

```c
#include <string.h>
char* strcat(char* dest, const char* src);
功能：将src字符串连接到dest的尾部，'\0' 也会追加过去
参数：
   dest：目的字符串首地址
   src：源字符串首地址
返回值：
   成功：返回dest字符串的首地址
  	失败：NULL
```

<font color='red'>注意</font>：需要保证被追加的字符串初始长度有追加字符串的长度空间大小否则报错

```c
#include <stdio.h>

extern void my_strcatp(char*, char*);

int main(void)
{
	// 需要保证该字符串有足够的空间来追加其它的字符串长度
	char ch[100] = "hello";
	char src[] = "world";
	// strcat(ch, src);
	my_strcatp(ch, src);
	printf("%s\n", ch);
	return 0;
}
// 追加字符串的函数
void my_strcatp(char* src, char* ch)
{
	while (*src)src++;
	while (*src++ = *ch++);
}
//-------------------------------运行结果-------------------------------
helloworld

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 22036)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 4 strncat():palm_tree: 

```c
#include <string.h>
char* strncat(char* dest, const char* src, size_t n);
功能：将src字符串前n个字符连接到dest的尾部，'\0' 也会追加过去
参数：
   dest：目的字符串首地址
   src：源字符首地址
   n：指定需要追加字符串个数
返回值：
   成功：返回dest字符串的首地址
   失败：NULL
```

<font color='red'>注意</font>：需要保证被追加的字符串初始长度有追加字符串的长度空间大小否则报错，可以进行限制追加字符串的长度

```c
#include <stdio.h>

extern void my_strncatp(char*, char*, int);

int main(void)
{
	// 该字符串初始长度要有足够的追加字符串的空间否则报错
	char ch[100] = "hello";
	char src[] = "world";
	// strncat(ch, src, 3);
	my_strncatp(ch, src, 3);
	printf("%s\n", ch);
	return 0;
}
// 字符串追加限制长度 的函数
void my_strncatp(char* src, char* ch, int v)
{
	while (*src)src++;
	while ((*src++ = *ch++) && --v);
}
//-------------------------------运行结果-------------------------------
hellowor

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 15568)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 5 strcmp():palm_tree: 

```c
#include <string.h>
int strcmp(const char* s1, const char* s2);
功能：比较s1和s2的大小，比较的是字符ASCII码大小。
参数：
   s1：字符串1首地址
   s2：字符串2首地址
返回值：
   相等：0
   大于：>0 在不用操作系统strcmp结果会不同，返回ASCII差值
   小于：<0
```

```c
#include <stdio.h>

extern int my_strcmp(char*, char*);

int main(void)
{
	char ch[] = "hello world";
	char ch1[] = "hello worldl\0 123";
	int flag1 = strcmp(ch, ch1);
	printf("flag1 = %d\n", flag1);
	int flag = my_strcmp(ch, ch1);
	printf("flag = %d\n", flag);
	if (!flag) printf("相同\n"); else printf("不相同\n");
	if (flag1 == flag)
	{
		printf("函数判断正确\n");
	}
	else
	{
		printf("函数判断失误\n");
	}
	return 0;
}

int my_strcmp(char* src, char* ch)
{
	if (*src == '\0' && *ch != '\0' || *src != '\0' && *ch == '\0')
		return *src > *ch ? 1 : -1;
	while (*src && *ch)
	{
		if (*src != *ch)
		{
			return *src > *ch ? 1 : -1;
			ch++;
			src++;
		}
		else if (strlen(src) != strlen(ch))
		{
			return strlen(src) > strlen(ch) ? 1 : -1;
		}
		ch++;
		src++;
	}
	return 0;
}
//------------------------------运行结果------------------------------
flag1 = -1
flag = -1
不相同
函数判断正确
E:\C\Demo\Project3\指针\Debug\指针.exe (进程 8136)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 6 strncmp():palm_tree: 

```c
#include <string.h>
int strncmp(const char** s1, const char* s2, size_t n);
功能：比较s1 和s2 前n个字符的大小，比较的是字符ASCII码大小
参数：
   s1：字符串1首地址
   s2：字符串2首地址
   n：指定比较字符串的数量
返回值：
   相等：0
   大于：>0 在不用操作系统strcmp结果会不同，返回ASCII差值
   小于：<0
```

```c
#include <stdio.h>

extern int my_my_strcmp1(char*, char*, int);

int main(void)
{
	char ch[] = "hello world";
	char ch1[] = "hallo world\0 123";
	int flag1 = strncmp(ch, ch1, 4);
	printf("flag1 = %d\n", flag1);
	int flag = my_strcmp1(ch, ch1, 4);
	printf("flag = %d\n", flag);
	if (!flag) printf("相同\n"); else printf("不相同\n");
	if (flag1 == flag)
	{
		printf("函数判断正确\n");
	}
	else
	{
		printf("函数判断失误\n");
	}
	return 0;
}

int my_strcmp1(char* src, char* ch, int v)
{
   // 判断是否其中一个字符串是空串
	if (*src == '\0' && *ch != '\0' || *src != '\0' && *ch == '\0')
	{
      // 进行比较返回对应的值
		return *src > *ch ? 1 : -1;
	}
   // 判断用户是否输入了v的值和字符串的长度是否大于等于v的值
	if (v && strlen(src) >= v && strlen(ch) >= v)
	{
      // 进行循环判断
		while(*src && *ch && v--)
		{
			if (*src != *ch)
			{
				return *src > *ch ? 1 : -1;
			}
			src++;
			ch++;
		}
	}
   // 如果用户没输入v的值 或者 输入了v的值 但是不大于等于字符串的长度，那么可以看做是 判断一个不相同的字符串进行如下的判断流程
	else
	{
		while (*src && *ch)
		{
			if (*src != *ch)
			{
				return 1;
			}
			else if (strlen(src) != strlen(ch))
			{
				return strlen(src) > strlen(ch) ? 1 : -1;
			}
			src++;
			ch++;
		}
	}
	// 字符串相同返回0
	return 0;
}
//-----------------------------------运行结果-----------------------------------
flag1 = 1
flag = 1
不相同
函数判断正确

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 21484)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 7 sprintf():palm_tree: 

```c
#include <string.h>
int sprintf(char* src, const char* format, ...);
功能：根据参数format字符串来转换并格式化数据，然后将结果输出到str指定的空间中，直到出现字符串结束符 '\0' 为止
参数：
   str：字符串首地址
   format：字符串格式，用法和printf()一样
返回值：
   成功：实际格式化的字符个数
   失败：-1
```

```c
#include <stdio.h>

int main(void)
{
	char ch[100];
	sprintf(ch, "hello world");
	printf("%s\n", ch);
	// 将后面的a，b，3分别赋值给前面的%s+%s=%d然后赋值到字符数组中
	sprintf(ch, "%s + %s = %d", "a", "b", 3);
	printf("%s\n", ch);
	return 0;
}
//-----------------------运行结果-----------------------
hello world
a + b = 3

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 17552)已退出，代码为 0。
按任意键关闭此窗口. . .
```



##### 8 sscanf():palm_tree: 

```c
#include <string.h>
int sscanf(const char* str, const char* format, ...);
功能：从str指定的字符串读取数据，并根据参数format字符串来转换并格式化数据。
参数：
   str：指定的字符串首地址
   format：字符串格式，用法和scanf()一样
返回值：
   成功：参数数目，成功转换的值的个数
   失败：-1
```

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	char ch[] = "1+2=3";
	int a, b, c;
	// 将ch中取出对应的值通过a,b,c的地址将值赋值
	sscanf(ch, "%d+%d=%d", &a, &b, &c);
	printf("%d\n", a);
	printf("%d\n", b);
	printf("=\n");
	printf("%d\n", c);

	char ch1[] = "helloworld";
	char ca[100];
	char cb[100];
	// %3s%s第一个读取3个第二个全部读取完
	sscanf(ch1, "%3s%s", &ca, &cb);
	printf("%s\n", ca);
	printf("%s\n", cb);
	return 0;
}
//---------------------------运行结果---------------------------
1
2
=
3
hel
loworld

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 15424)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 9 strchr():palm_tree: 

```c
#include <string.h>
char* strchr(const char* s, int c);
功能：在字符串s中查找字母c出现的位置
参数：
   s：字符串首地址
   c：匹配字母 (字符)
返回值：
   成功：返回第一次出现的c地址
   失败：NULL
```

```c
#include <stdio.h>

extern char* my_strchr(char*, char);

int main(void)
{
	char ch[] = "hello world";
	char c = 'l';
	// char* index = strchr(ch, c);
	char* index = my_strchr(ch, c);
	printf("%s\n", index);
	return 0;	
}
// 实现strchr功能 函数
char* my_strchr(char* ch, char c)
{
	while (*ch)
	{
		if (*ch == c) return ch;
		ch++;
	}
	return NULL;
}
//-----------------------------------运行结果-----------------------------------
llo world

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 7820)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 10 strstr():palm_tree: 

```c
#include <string.h>
char* strstr(const char* haystack, const char* needle);
功能：在字符串haystack中查找字符串needle出现的位置
参数：
   haystack：源字符串首地址
   needle：匹配字符串首地址
返回值：
   成功：返回第一次出现的needle地址
   失败：NULL
```

```c
#include <stdio.h>

int main(void)
{
	char ch[] = "hello world";
	char ch1[] = "llo";
	// 获取ch1字符串中的位置以及后面的内容
	char* index = strstr(ch, ch1);
	printf("%s\n", index);
	return 0;
}
//-------------------------------运行结果-------------------------------
llo world

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 6288)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 11 strtok():palm_tree: 

```c
#include <string.h>
char* strtok(char* str, const char* delim);
功能：来将字符串分割成一个个分段，当strtok()在参数s的字符串中发现参数delim中包含的分隔字符时，则会将该字符改为0字符，当连续出现多个时只替换第一个为0
参数：
   str：指向预分割字符串中包含的所有字符
返回值：
   成功：分割后字符串首地址
   失败：NULL
```

-  在第一次调用时：strtok()必须给予参数s字符串
-  往后的调用则将参数s设置成NULL，每次调用成功则返回指向被分割出片段的指针

```c
#include <stdio.h>

int main(void)
{
	// 字符串截取会破坏源字符串，strtok会用\0来替换标志位 "."
	char ch[] = "www.dkx.net"; // www\0dkx.net\0
	char* new_str = strtok(ch, ".");
	printf("%s\n", new_str); // www
	new_str = strtok(NULL, ".");
	printf("%s\n", new_str); // dkx
	new_str = strtok(NULL, ".");
	printf("%s\n", new_str); // net
	return 0;
}
//--------------------------运行结果--------------------------
www
dkx
net

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 5868)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 12 atoi():palm_tree: 

类似于javaScript中的parseInt

```c
#include <stdlib.h>
int atoi(const char* nptr);
功能：atoi()会扫描nptr字符串，跳过前面的空格字符，直到遇到数字或正负号才开始做转换，而遇到非数组或字符串结束符 '\0' 才结束转换，并将结果返回返回值
参数：
   nptr：待转换的字符串
返回值：
   成功转换后整数
```

类似的函数有：

-  atof()：把一个小数形式的字符串转化为一个浮点数
-  atol()：将一个字符串转化为long类型

```c
#include <stdio.h>

int main(void)
{
	char ch[] = "-123-456";
	// 只能识别十进制整数
	int i = atoi(ch);
	printf("%d\n", i);
	return 0;
}
//-----------------------------运行结果-----------------------------
-123

E:\C\Demo\Project3\指针\Debug\指针.exe (进程 12524)已退出，代码为 0。
按任意键关闭此窗口. . .
```

## 8 内存管理:christmas_tree: 

### 8.1 作用域:deciduous_tree: 

C语言变量的作用域分为：

-  代码块作用域 (代码块是 {} 之间一段代码)
-  函数作用域
-  文件作用域

#### 8.1.1 局部变量:evergreen_tree: 

局部变量也叫 auto 自动变量 (auto可写可不写) ，一般情况下代码块 {} 内部定义的变量都是自动变量，它有如下特点：

-  在一个函数内定义，只在函数范围内有效
-  在复合语句中定义，只在复合语句中有效
-  <font color='red'>随着函数调用的结束或复合语句的结束局部变量的生命周期也结束</font>
-  如果没有赋初始值，内容为随机

```c
#include <stdio.h>

int vmvavin(void)
{
	// 定义变量 局部变量
	// 在函数内部定义的变量，称为局部变量，可以使用auto进行修饰
	// 也可以加auto
	// 作用域：在函数内部
	// 生命周期：从创建到函数结束
	auto int a = 10;
	// 或者
	int b = 20;
	return 0;
}
```

#### 8.1.2 静态 (static) 局部变量:evergreen_tree: 

-  static局部变量的作用域也是在定义的函数内有效。
-  static局部变量的声明周期和程序运行周期一样，同时static局部变量的值<font color='red'>只初始化一次，但可以赋值多次</font>。
-  static局部变量若未赋值以初始值，则由系统自动赋值：数值型变量自动赋初始值0，字符型变量赋空字符。

```c
#include  <stdio.h>

void fun02()
{
	// 静态变量只会初始化一次，可以多次赋值
	// 在数据区进行存储
	// 作用域：函数内部范围
	// 生命周期：从程序创建到程序销毁
	static int b = 10;
	b++;
	printf("b = %d\n", b); // 输出的是数值的增长
	int a = 10;
	a++;
	printf("a = %d\n", a); // 输出的都是11
}

int main(void)
{
	// 静态局部变量
	// static int b = 10;
	int i = 0;
	for (;i < 10;i++)
	{
		fun02();
	}
	// printf("b = %d\n", b);
	return 0;
}
//------------------------------运行结果------------------------------
b = 11
a = 11
b = 12
a = 11
b = 13
a = 11
b = 14
a = 11
b = 15
a = 11
b = 16
a = 11
b = 17
a = 11
b = 18
a = 11
b = 19
a = 11
b = 20
a = 11

E:\C\Demo\Project3\内存管理\x64\Debug\内存管理.exe (进程 14252)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 8.1.3 全局变量:evergreen_tree: 

-  在函数外定义，可被本文件及其它文件中的函数所共用，若其它文件中的函数调用此变量，须用extern声明
-  全局变量的生命周期和程序运行周期一样
-  <font color='red'>不同文件的全局变量不可重名</font>.

02全局变量.c

```c
#include <stdio.h>

// 全局变量存储在数据区
// 全局变量，定义在函数外部的变量
// 作用域：整个项目中所有文件，如果 其它某个文件需要使用
// 该全局变量需要在它的文件中进行声明extern int a; 即可
// 生命周期：从程序创建到程序销毁
int a = 10;

void fun()
{
	a = 100;
	printf("fun = %d\n", a);
}

int main(void)
{
	// 局部变量
	int a = 123;
	printf("main - a = %p\n", &a);
	{
		// 局部变量
		// 作用域：代码块范围内外部访问不到
		int a = 456;
		printf("main - {one} - a = %p\n", &a);
		printf("main -{one} = %d\n", a);
	}
	{
		// a 变量没有进行重新定义按就近原则 找最近的存在的a变量
		// 也就是main函数中的a变量然后进行重新赋值
		a = 789;
		printf("main - {two} - a = %p\n", &a);
		printf("main -{two} = %d\n", a);
	}
	// 数据在操作时会采用就近原则
	printf("main = %d\n", a);
	fun();
	fun01();
	return 0;
}
//------------------------------运行结果------------------------------
main - a = 000000090C79F8E4
main - {one} - a = 000000090C79F904
main -{one} = 456
main - {two} - a = 000000090C79F8E4
main -{two} = 789
main = 789
fun = 100
fun01 = 1000

E:\C\Demo\Project3\内存管理\x64\Debug\内存管理.exe (进程 10020)已退出，代码为 0。
按任意键关闭此窗口. . .
```

02test.c

```c
#include <stdio.h>

// int a = 110; 多个全局变量命名不能重复

// 声明int a变量
extern int a;

void fun01()
{
	a = 1000;
	printf("fun01 = %d\n", a);
}
```



![image-20230929122402239](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309291224448.png)

#### 8.1.4 静态(static)全局变量:evergreen_tree: 

-  static 局部变量的作用域也是在定义的函数内有效
-  static局部变量的声明周期和程序运行周期一样，同时static局部变量的值<font color='red'>只初始化一次，但可以赋值多次</font>。
-  static局部变量若未赋予初始值，则由系统自动赋值：数值型变量自动赋值初始值0，字符型变量赋空字符

静态全局变量.c

```c
#include <stdio.h>

// 静态全局变量
// 作用域；可以在文本文件中使用，不可以在其它文件中使用
// 生命周期：从程序创建到程序销毁
static int d = 10;

void fun05()
{
	d = 20;
	printf("%d\n", d);
}

int main(void)
{
	printf("%d\n", d);
	fun05();
   // 03test.c测试时打开
	// fun06();
	return 0;
}
//--------------------------------运行结果--------------------------------
10
20

E:\C\Demo\Project3\内存管理\x64\Debug\内存管理.exe (进程 6724)已退出，代码为 0。
按任意键关闭此窗口. . .
```

03test.c

```c
#include <stdio.h>
// 找不到外部符号 d
extern int d;

void fun06()
{
	printf("%d\n", d);
}
```

![image-20230929184139331](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309291841379.png)

#### 8.1.5 extern全局变量声明:evergreen_tree: 

extern int a; 声明一个变量，这个全局变量在别的文件中已经定义了，这里只是声明，而不是定义。

#### 8.1.6 全局函数和静态函数:evergreen_tree: 

在C语言中函数默认都是全局的，使用关键字static可以将函数声明为静态，函数定义为static就意味着这个函数只能在定义这个函数的文件中使用，在其它文件中不能调用，即使在其它文件中声明这个函数都没用。

对于不同文件中的static函数名字可以相同。

![image-20230929232417298](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309292324673.png)

全局函数和静态函数.c

```c
#include <stdio.h>

// 函数可以自己调用自己 称为 递归
extern void sort(int*, int);
// 声明静态函数
extern void fun07();

int main(void)
{
	int arr[] = { 9, 1, 5, 6, 8, 2, 7, 10, 4, 3 };
	// 全局函数的名称是作用域中唯一的，不允许出现重名
	// 作用域：在整个项目中所有文件中使用
	sort(arr, sizeof(arr)/ sizeof(arr[0]));
	int i = 0;
	for (;i < sizeof(arr)/ sizeof(arr[0]); i++)
	{
		printf("%d\n", arr[i]);
	}
	// 这个静态函数作用域仅限于它当前文件中，运行会报错
	// 在当前文件中可以定义与其它文件重名的静态函数，那么它执行的就是当前文件的静态函数
	fun07();
	return 0;
}

// 静态函数  可以和全局函数重名
// 作用域：当前文件中
// 声明周期：从程序创建到程序销毁 
static void fun07()
{
	printf("hello world\n");
}
//------------------------------运行结果------------------------------
1
2
3
4
5
6
7
8
9
10
hello world

E:\C\Demo\Project3\内存管理\x64\Debug\内存管理.exe (进程 3108)已退出，代码为 0。
按任意键关闭此窗口. . .
```

04test.c

```c
void sort(int* src, int len)
{
	int flag = 1;
	int i = 0;
	for (;i < len;i++)
	{
		int j = 0;
		for (;j < len-i-1;j++)
		{
			if (*(src + j) > *(src + j + 1))
			{
				flag = 0;
				int temp = *(src + j + 1);
				*(src + j + 1) = *(src + j);
				*(src + j) = temp;
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
```

5test.c

```c
#include <stdio.h>

// 静态函数
// 作用域：当前文件中
static void fun07()
{
	printf("hello world\n");
}
```



<font color='red'>注意</font>：如果不在，全局函数和静态函数中去声明sort函数则跳转不到函数定义，如果声明则可以，如下图演示，建议声明因为有意义

![test](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309292004720.gif)

<font color='red'>注意</font>：

-  运行在不同的函数中使用相同的变量名，它们代表不同的对象，分配不同的单元，互不干扰。
-  同一源文件中，允许全局变量和局部变量同名，在局部变量的作用域内，全局变量不起作用。
-  所有的函数默认都是全局的，意味着所有的函数都不能重名，但如果是static函数，那么作用域是文件级的，所以不同的文件static函数名是可以相同的。

#### 8.1.7 总结:evergreen_tree: 

| 类型            | 作用域     | 生命周期       |
| --------------- | ---------- | -------------- |
| auto 变量       | 一对 {} 内 | 当前函数       |
| static 局部变量 | 一对 {} 内 | 整个程序运行期 |
| extern 变量     | 整个程序   | 整个程序运行期 |
| static 全局变量 | 当前文件   | 整个程序运行期 |
| extern 函数     | 整个程序   | 整个程序运行期 |
| static 函数     | 当前文件   | 整个程序运行期 |
| register 变量   | 一对 {} 内 | 当前函数       |
| 全局变量        | 整个程序   | 整个程序运行期 |



### 8.2 内存布局:deciduous_tree: 

#### 8.2.1 内存分区:evergreen_tree: 

C代码经过<font color='red'>预处理</font>，<font color='red'>编译</font>，<font color='red'>汇编</font>，<font color='red'>链接</font> 4 步后生成一个可执行程序。

在Windows下，程序是一个普通的可执行文件，以下例出一个二进制可执行文件的基本情况：

```c
#include <stdio.h>

// 安全的常量定义格式 ， 存储为数据区常量区
const int f = 5;
// 未初始化的全局变量
int a;
// 初始化的全局变量
int a1 = 10;
// 未初始化的静态全局变量
static b;
// 初始化的静态全局变量
static b1 = 20;
int main(void)
{
	// 未初始化的局部变量
	int c;
	// 初始化的局部变量
	int c1 = 30;
	// 未初始化的静态局部变量
	static int d;
	// 初始化的静态局部变量
	static int d1 = 40;
	// 字符串常量
	char* p = "hello world";
	// int类型数组
	int arr[] = { 1, 2, 3, 4 };
	// 指针
	int* pp = arr;
	printf("全局常量变量：%p\n", &f);
	printf("未初始化的全局变量：%p\n", &a); 
	printf("初始化的全局变量：%p\n", &a1);
	printf("未初始化的静态全局变量：%p\n", &b);
	printf("初始化的静态全局变量：%p\n", &b1);
	printf("未初始化的局部变量：%p\n", &c);
	printf("初始化的局部变量：%p\n", &c1);
	printf("未初始化的静态局部变量：%p\n", &d);
	printf("初始化的静态局部变量：%p\n", &d1);
	printf("字符串常量：%p\n", p);
	printf("int类型数组：%p\n", arr);
	printf("指针：%p\n", pp);
	printf("指针地址：%p\n", &pp);
	return 0;
}
```

![image-20230930122824027](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309301228333.png)

![image-20230930125556574](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309301255299.png)

![image-20230930130654361](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309301306912.png)

通过上图可以得知，在没有运行程序前，也就是说<font color='red'>程序没有加载到内存前</font>，可执行程序内部已经分好3段信息，分别为<font color='red'>代码区(text)</font>，<font color='red'>数据区(data)和未初始化数据区(bss) </font>3个部分 (有些人直接把data和bss合起来叫做静态区或全局区)

-  代码区

存放CPU执行的机器指令。通常代码区是可共享的(即另外的执行程序可以调用它)，使其可共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可。<font color='red'>代码区通常是只读的</font>，使其只读的原因是防止程序意外的修改了它的指令。另外，代码区还规划了局部变量的相关信息

-  全局初始化数据区/静态数据区(data段)

该区包含了在程序中明确被初始化的全局变量，已经初始化的静态变量(包括全局静态变量和局部静态变量) 和常量数据 (如字符串常量)。



-  未初始化数据区(bss)

存入的是全局未初始化变量和未初始化静态变量。未初始化数据区的数据在程序开始执行之前被内核初始化为0或者空(NULL)

程序在加载到内存前，<font color='red'>代码区和全局区(data和bss)的大小就是固定的</font>，程序运行期间 不能改变。然后，运行可执行程序，系统把程序加载到内存，<font color='red'>除了根据可执行程序的信息分出代码区(text)，数据区(data)和未初始化数据区(bss)之外，还额外增加了栈区，堆区</font>。

![image-20230930144648128](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309301446888.png)

<center>内存模型图</center>

![image-20230930145240721](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309301452105.png)

-  代码区(text segment)

记载的是可执行文件代码段，所有的可执行代码都加载到代码区，这块内存是不可以在运行期间修改的

-  未初始化数据区(BSS)

加载的是可执行文件BSS段，位置可以分开也可以紧靠数据段，存储于数据段的数据(全局未初始化，静态未初始化数据)的生存周期为整个程序运行过程

-  全局初始化数据区 / 静态数据区 (data segment)

记载的是可执行文件数据段，存储于数据段(全局初始化，静态初始化数据，文字常量(只读))的数据生存周期为整个程序运行过程

-  栈区(stack)

栈是一种先进后出的内存结构，由编译器自动分配释放，存放函数的参数值，返回值，局部变量等。在程序运行过程中实时加载释放，因此，局部变量的生存周期为申请到释放该段栈空间

```c
#include <stdio.h>

extern void swap(int, int);

int main(void)
{
	int a = 10;
	int b = 20;
	printf("a = %p\n", &a);
	printf("b = %p\n", &b);
	swap(a, b);
	return 0;
}

void swap(int a, int b)
{
	int temp = a;
	a = b;
	b = temp;
	printf("a = %p\n", &a);
	printf("b = %p\n", &b);
}
```



<center>栈区存储模型</center>

![image-20230930151554523](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309301515899.png)

-  栈区(heap)

堆是一个大容器，它的容量要远远大于栈，但没有栈那样先进后出的顺序。用于动态内存分配。堆在内存中位于BSS区和栈区之间。一般由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收。

```c
#include <stdio.h>

int main(void)
{
	// 栈区大小 1 兆
	// 820000 * 4 = 328,000 个字节
	// 328,000 / 1024 = 320.3125 kb
	// 320.3125 / 1024 ≈ 3 兆
	// 3 兆 / 3 ≈ 1 兆
	// 1 兆情况下也可能会挂掉(报错) 因为栈区要加载一些函数信息
	// 但是这个约等于1兆 也比1兆大了点
	// int arr[820000 / 3] = { 0 };
	// 3 兆 / 4 ≈ 0.7 兆  小于 1 兆 debug执行的话不会挂掉(报错)
	// int arr[820000 / 4] = { 0 };
	// 开辟堆空间存储数据
	// 开辟一个4个字节大小的空间
	int* p = (int*) malloc(sizeof(int));
	printf("%p\n", p);
	// 使用堆空间
	*p = 123;
	printf("%d\n", *p);
	// 玩用完后 要 释放堆空间
	free(p);
	// p 释放掉还有值就是野指针了
	printf("%p\n", p);
	*p = 456;
	printf("%d\n", *p);
	// 避免野指针的出现每次使用完后将该指针置为空(NULL)
	p = NULL; // 如果程序执行完本身就结束了该语句可以不写
	return 0;
}
//------------------------------运行结果------------------------------
00C01A70
123
00C01A70
456

E:\C\Demo\Project3\内存管理\Debug\内存管理.exe (进程 16856)已退出，代码为 0。
按任意键关闭此窗口. . .
```

```c
#include <stdio.h>

int main(void)
{
	// 4 * 8200000 = 32,800,000 kb * 100 = 3,280,000,000 gb
	// int* p = (int*)malloc(sizeof(int) * 8200000 * 100); // 开辟内存大小为3G大的空间
	// printf("%p\n", p); // 00000000 指向内存编号为00000000的地址
	int* p = (int*)malloc(sizeof(int) * 8200000 * 100 / 3); //开辟1G空间
	printf("%p\n", p);// 02EFD040
	// 建议如果开辟的空间比较大做一下判断
	if (!p)
	{
		printf("内存爆炸了 !!!");
		return -1;
	}
	return 0;
}
//---------------------------运行结果---------------------------
031E1040

E:\C\Demo\Project3\内存管理\Debug\内存管理.exe (进程 5360)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 8.2.2 开辟的堆空间与释放堆空间的位置关系:evergreen_tree: 

```c
#include <stdio.h>
#define MAX 10

extern void sort01(int*, int);

int main(void)
{
	srand((size_t) time(NULL));
	int* p = (int*) malloc (sizeof(int) * MAX);
	printf("开辟堆空间的内存地址：%p\n", p);
	int i = 0;
	for (;i < MAX;i++)
	{
		p[i] = rand() % 100;
		printf("%d\t", p[i]);
	}
	sort01(p, MAX);
	printf("\n--------排序后--------\n");
	int j = 0;
	for (;j < MAX;j++)
	{
		printf("%d\t", *p);
		p++;
	}
	printf("\n堆空间指针后移10位：%p\n", p);
	// 如果将堆空间的内存地址在该到原来的位置再去free释放时就不会报错了
	// 因为这样开辟的时候和释放的时候都是同一个位置的内存空间地址
	p -= 10;
	printf("堆空间指针前移10位：%p\n", p);
	/**
	* 这里会报错
	* 因为，指针开辟的位置和释放的位置发生了改变
	* 在排序的时候指针++偏移量已经不在原来的位置了
	* 再回来释放这个堆空间就报错了。
	*/
	free(p);
	return 0;
}

void sort01(int* src, int len)
{
	int flag = 1;
	int i = 0;
	for (;i < len - 1;i++)
	{
		int j = 0;
		for (;j < len - i - 1;j++)
		{
			if (*(src + j) > *(src + j + 1))
			{
				flag = 0;
				int temp = *(src + j + 1);
				*(src + j + 1) = *(src + j);
				*(src + j) = temp;
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
//-------------------------------运行结果-------------------------------
开辟堆空间的内存地址：012F6848
67      88      48      44      83      10      85      81      34      84
--------排序后--------
10      34      44      48      67      81      83      84      85      88
堆空间指针后移10位：012F6870
堆空间指针前移10位：012F6848

E:\C\Demo\Project3\内存管理\Debug\内存管理.exe (进程 3996)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 8.2.3 内存操作函数:evergreen_tree: 

##### 1 memset():palm_tree: 

```c
#include <string.h>
void* memset(void* s, int c, size_t n);
功能：将s的内存区域的前n个字节以参数c填入
参数：
   s：需要操作内存s的首地址
   c：填充的字符，c虽然参数为int，但必须是unsigned char，范围为 0 ~ 255
   n：指定需要设置的大小
返回值：s的首地址
```

```c
#include <stdio.h>

int main(void)
{
	// 开辟40个字节空间，存储10个整型变量，内存地址是连续的
	int* p = (int*)malloc(sizeof(int)*10);
	// memset 重置内存空间的值
	// 将p堆空间的40个字节大小重置为1
	memset(p, 1, sizeof(4)*10);// 16843009
	/**
	* 为什么输出的是16843009呢？
	* 1个字节8个比特位 0000 0000
	* 4个字节32个比特位：0000 0000 0000 0000 0000 0000 0000 0000
	* 重置为1对应的二进制空间为：0001 0001 0001 0001 0001 0001 0001 0001
	* 对应的十六进制为：01 01 01 01 4个字节空间的值
	* 再转换为十进制显示：一个int类型空间为：16843009 循环10次
	* 开辟：4字节*10 = 40字节 
	* 循环：1:4 2:8 3:12 4:16 5:20 6:24 7:28 
	* 8:32 9:34 10:38 每循环4个字节十进制值为：16843009
	*/
	int i = 0;
	for (;i < 10;i++)
	{
		printf("%d\t", p[i]);
	}
	free(p);
	return 0;
}
//--------------------------------运行结果--------------------------------
16843009        16843009        16843009        16843009        16843009        16843009        16843009        168430016843009 16843009
E:\C\Demo\Project3\内存管理\Debug\内存管理.exe (进程 17428)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 2 memcpy():palm_tree: 

```c
#include <string.h>
void* memcpy(void* dest, const void* src, size_t n);
功能：拷贝src所指的内存内容的前n个字节到dest所指的内存地址上
参数：
   dest：目的内存首地址
   src：源内存首地址，注意：dest和src所指的内存空间不可重叠，可能会导致程序报错
   n：需要拷贝的字节数
返回值：dest的首地址
```

```c
#include <stdio.h>

int main(void)
{
	int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
	int* p = (int*)malloc(sizeof(int) * 10);
	memcpy(p, arr, sizeof(int) * 10);
	int i = 0;
	for (;i < 10;i++)
	{
		printf("%d\n", p[i]);
	}
	free(p);
	printf("------------------------\n");
	char ch[] = "hello\0 world";
	char ch1[100];
	// 字符串拷贝遇到\0停止
	strcpy(ch1, ch);
	int i1 = 0;
	for (; i1 < 13; i1++)
	{
		printf("%c", ch1[i1]);
	}
	printf("\n------------------------\n");
	// 内存拷贝 拷贝的内容和字节有关
	memcpy(ch1, ch, 13);
	int i2 = 0;
	for (; i2 < 13; i2++)
	{
		printf("%c", ch1[i2]);
	}
	return 0;
}
//------------------------运行结果------------------------
1
2
3
4
5
6
7
8
9
10
------------------------
hello烫烫烫
------------------------
hello world
E:\C\Demo\Project3\内存管理\Debug\内存管理.exe (进程 17232)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 3 memmove():palm_tree: 

memmove()功能用法和memcpy()一样，区别在于：dest和src所指的内存空间重叠时，memmove()仍然能处理，不过执行效率比memcoy()低些

```c
#include <stdio.h>

int main()
{
	int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
	// 如果拷贝的目标和源发生重叠，可能报错
	// memcpy(&arr[5], &arr[3], 20);
	memmove(&arr[5], &arr[3], 20);
	int i = 0;
	for (;i < 10;i++)
	{
		printf("%d\t", arr[i]);
	}
	return 0;
}
//--------------------------运行结果-------------------------
1       2       3       4       5       4       5       6       7       8
E:\C\Demo\Project3\内存管理\Debug\内存管理.exe (进程 17712)已退出，代码为 0。
按任意键关闭此窗口. . .
```

##### 4 memcmp():palm_tree: 

```c
#include <string.h>
int memcmp(const void* s1, const void* s2, size_t n);
功能：比较s1和s2所指向内存区域的前n个字节
参数：
   s1：内存首地址1
   s2：内存首地址2
   n：需比较的前n个字节
返回值：
   相等：=0
   大于：>0
   小于：<0
```

```c
#include <stdio.h>

int main(void)
{
	int arr[] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
	int arr1[] = { 1, 2, 3, 4, 5 };
	// 比较arr与arr1内存字节大小与内容无关4*5=20字节大小
	int value = memcmp(arr, arr1, 20);
	printf("%d\n", value);
	char ch[] = "hello\0 world";
	char ch1[] = "hello\0 world";
	int value1 = memcmp(ch, ch1, 20);
	printf("%d\n", value);
	return 0;
}
//-------------------------------运行结果-------------------------------
0
0

E:\C\Demo\Project3\内存管理\Debug\内存管理.exe (进程 8720)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 8.2.4 内存常见问题:evergreen_tree: 

##### 1 数组越界:palm_tree: 

![image-20231001121752613](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310011217092.png)

```c
#include <stdio.h>

int main(void)
{
	// 数组下标越界
	// char ch[11] = "hello world";
	char* p = (char*)malloc(sizeof(char) * 11);
	// 开辟了11字节大小的空间往里面拷贝了12个长度的内容
	// 释放时就会出现问题，就是多处一个空间是否要释放
	// 释放导致多释放一段空间，不释放就会浪费空间
	strcpy(p, "hello world");
	printf("%s\n", p);
	free(p);
	return 0;
}
//---------------------运行结果---------------------
hello world
```

![image-20231001121428137](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310011214421.png)

<font color='red'>注意</font>：报错内存需要重新处理就会卡顿主

![image-20231001122214498](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310011222521.png)

##### 2 野指针:palm_tree: 

```c
#include <stdio.h>

int main(void)
{
	// 野指针
	int* p = (int*)malloc(0);
	printf("%p\n", p);
	*p = 100;
	printf("%d\n", p);
	free(p);
	return 0;
}
```

与上面同样的报错

##### 3 堆空间多次释放:palm_tree: 

```c
#include <stdio.h>

int main(void)
{
	int* p = (int*)malloc(sizeof(int) * 10);
	free(p);
	// free(p); ERROR 堆空间不允许多次释放
	// 空指针允许多次释放
	p = NULL;
	free(p);
	return 0;
}
```

##### 4 函数传递:palm_tree: 

两个同级别的指针就是值传递

```c
#include <stdio.h>

void fun08(int* p)
{
	p = (int*)malloc(sizeof(int) * 10);
}

int main(void)
{
	int* p = NULL;
	fun08(p);
	int i = 0;
	for (;i < 10;i++)
	{
		p[i] = i;
	}
	free(p);
	return 0;
}
```

![image-20231001125126240](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310011251783.png)

引用传递

```c
#include <stdio.h>

void fun08(int* p)
{
	p = (int*)malloc(sizeof(int) * 10);
	printf("形参：%p\n", p);
}

void fun09(int** p)
{
	*p = (int*)malloc(sizeof(int) * 10);
	printf("形参：%p\n", *p);
}

int main(void)
{
	int* p = NULL;
	fun08(p);
	fun09(&p);
	printf("实参：%p\n", p);
	free(p);
	return 0;
}
//------------------------运行结果------------------------
形参：01306380
形参：01306900
实参：01306900

E:\C\Demo\Project3\内存管理\Debug\内存管理.exe (进程 8452)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 8.2.5 二级指针对应的堆空间:evergreen_tree: 

![image-20231002010109336](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310020101991.png)

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void)
{
	// int arr[5][3];
	// 开辟二级指针对应的堆空间
	int** p = (int**)malloc(sizeof(int) * 5);
	int i = 0;
	for (;i < 5;i++)
	{
		// 开辟一级指针对应的堆空间
		p[i] = (int*)malloc(sizeof(int) * 3);
	}
	int i1 = 0;
	for (;i1 < 5;i1++)
	{
		int j = 0;
		for (;j < 3;j++)
		{
			scanf("%d", &p[i1][j]);
		}
	}
	int i2 = 0;
	for (;i2 < 5;i2++)
	{
		int j = 0;
		for (;j < 3;j++)
		{
			// printf("%d", p[i2][j]);
			printf("%d ", *(*(p + i2) + j));
		}
		printf("\n");
	}
	int i3 = 0;
	for (;i3 < 5;i3++)
	{
		// 释放内层
		free(p[i3]);
	}
	// 释放外层
	free(p);
	return 0;
}
//---------------------------运行结果---------------------------
1 2 3
4 5 6
7 8 9
10 11 12
13 14 15
1 2 3
4 5 6
7 8 9
10 11 12
13 14 15

E:\C\Demo\Project3\内存管理\Debug\内存管理.exe (进程 12148)已退出，代码为 0。
按任意键关闭此窗口. . .
```

## 9 复合类型(自定义类型):christmas_tree: 

### 9.1 结构体:deciduous_tree: 

#### 9.1.1 概述:evergreen_tree: 

数组：描述一组具有相同类型数据的有序集合，用于处理大量相同类型的数据运算。

有时我们需要将不同类型的数据组合成一个有机的整体，如：一个学生有学号/姓名/性别/年龄/地址等属性。显然单独定义以上变量比较繁琐，数据不便于管理。

C语言中给出了另一种构造数据类型——结构体

![image-20231002093932637](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310020939274.png)

#### 9.1.2 结构体变量的定义和初始化:evergreen_tree: 

定义结构体变量的方式：

-  先声明结构体类型再定义变量名
-  直接定义结构体类型变量 (无类型名)

![image-20231002094204433](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310020942621.png)

#### 9.1.3 结构体成员的使用:evergreen_tree: 

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

// struct 结构体名
// {
// 姓名
// 年龄
// }结构体变量1，结构体变量2，结构体变量3; 这里定义的是全局变量
struct student
{
	// 多余的1个是\0 ，20个字符的名字考虑到多个民族
	char name[21];
	int age;
	int score;
	char address[51];
	//定义结构体同时为结构体赋值
}/*stu = {"李四", 20, 100, "北京华平区08号路"}*/;
int main(void)
{
	// 创建结构体变量
	// 结构体类型 结构体变量
	struct student stu;
	// stu.name = "张三";
	// 字符串需要使用拷贝进行赋值
	// strcpy(stu.name, "张三");
	// stu.age = 18;
	// stu.score = 100;
	// stu.address = "上海昌平区23号路";
	// strcpy(stu.address, "上海昌平区23号路");
	// 有参构造赋值 需要将上面注释掉否则报错重定义
	// struct student stu = { "李四", 20, 100, "北京华平区08号路" };
	scanf("%s%d%d%s", stu.name, &stu.age, &stu.score, stu.address);
	printf("姓名：%s\n", stu.name);
	printf("年龄：%d\n", stu.age);
	printf("分数：%d\n", stu.score);
	printf("地址：%s\n", stu.address);
	return 0;
}
//------------------------------运行结果------------------------------
张三 18 100 北京朝阳区
姓名：张三
年龄：18
分数：100
地址：北京朝阳区

E:\C\Demo\Project3\day09\x64\Debug\day09.exe (进程 18160)已退出，代码为 0。
按任意键关闭此窗口. . .
```

#### 9.1.4 结构体数组:evergreen_tree: 

```c
#include <stdio.h>

struct student
{
	char name[21];
	int age;
	char sex;
	int score;
	char addr[51];
};

int main(void)
{
	struct student stu[3] =
	{
		{ "张三", 18, 'M', 90, "北京" },
		{ "李四", 20, 'M', 99, "上海" },
		{ "刘桑", 19, 'M', 100, "邯郸" }
	};
	// sizeof() 计算数据类型在内存中占的字节大小
	printf("结构体数组大小：%d\n", sizeof(stu)); // 264
	// 结构体中：21 + 51 + 1 + 4 + 4 = 81 为什么与输出的不对应呢？
	// 原因：结构体成员需要偏移对齐
	printf("结构体元素大小：%d\n", sizeof(stu[0])); // 88
	printf("结构体元素个数：%d\n", sizeof(stu)/ sizeof(stu[0])); // 3
	printf("=======学生成绩表=======\n");
	int i = 0;
	for (;i < sizeof(stu) / sizeof(stu[0]);i++)
	{
		printf("-----------------------\n");
		printf("姓名：%s\n", stu[i].name);
		printf("年龄：%d\n", stu[i].age);
		printf("性别：%s\n", stu[i].sex == 'M' ? "男" : "女");
		printf("分数：%d\n", stu[i].score);
		printf("地址：%s\n", stu[i].addr);
		printf("-----------------------\n");
	}
	return 0;
}
//------------------------运行结果------------------------
结构体数组大小：264
结构体元素大小：88
结构体元素个数：3
=======学生成绩表=======
-----------------------
姓名：张三
年龄：18
性别：男
分数：90
地址：北京
-----------------------
-----------------------
姓名：李四
年龄：20
性别：男
分数：99
地址：上海
-----------------------
-----------------------
姓名：刘桑
年龄：19
性别：男
分数：100
地址：邯郸
-----------------------

E:\C\Demo\Project3\day09\x64\Debug\day09.exe (进程 11680)已退出，代码为 0。
按任意键关闭此窗口. . .
```
