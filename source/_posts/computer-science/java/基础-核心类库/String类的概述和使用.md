---
title: String类的概述和使用
categories:
    - [计算机学科,java,基础-核心类库]
    - java
    - 基础
---

# String类的概述和使用:christmas_tree: 

###### [查看绝世秘籍](#秘籍) :green_book: 


[TOC]

##### 1.String类的概念(重点):deciduous_tree: 

*  java.lang.String类用于描述字符串,java程序中所有的字符串字面值都可以使用该类的对象加以描述如: "abc"

*  该类由**final** 关键字修饰,表示该类不能被继承

*  从JDK1.9开始该类的底层不适用char[ ]来存储数据,而是改成byte[ ]加上编码标记,从而节约了一些空间

*  该类描述的字符串内容是个**常量**不可更改,因此可以被共享使用因为修饰了**static** 

   如:

   ​		`String  str1 = "abc";` - 其中 "abc" 这个字符串是个常量不可改变

   ​		 `str1 = "123";`				  - 将 "123" 字符串的地址赋值给变量str1

   ​													   - 改变str1的指向并没有改变指向的内容

![image-20230104122739584](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051122005.png)

##### 1.2常量池的概念(原理):deciduous_tree: 

由于String类型描述的字符串内容是不可改变,因此java虚拟机将首次出现的字符串放入常量池中,若后续代码中出现了相同的字符串内容则直接使用池中已有的字符串对象而无需申请内存及创建对象,从而提高了性能

##### 1.3常用的构造方法(练熟,记住):deciduous_tree: 

| 方法声明                                    | 功能介绍                                                     |
| ------------------------------------------- | ------------------------------------------------------------ |
| String(  )                                  | 使用无参方式构造对象得到空字符序列                           |
| String(byte[ ] bytes,int offset,int length) | 使用bytes数组中下标从offset位置开始的length个字节来构造对象  |
| String(byte[ ] bytes)                       | 使用bytes数组中的所有内容构造对象                            |
| String(char[ ] value,int offset,int count)  | 使用value数组中下标从offset位置开始的count个字符来构造对象   |
| String(char[ ] value)                       | 使用value数组中的所有内容构造对象                            |
| String(String original)                     | 根据参数指定的字符串内容来构造对象,新创建对象为参数对象的副本 |

-  将字符串类型的字符串转换为整数类型

   ```java
   String ar = new String("12345");
   	//存储变量
   	int ib = 0;
   for(int i = 0;i < ar.length();i ++){
       //每次执行让前面的值乘10加上个位数就实现了12345的整数转换
       ib = ib * 10 + (ar.charAt(i) - '0');
   }
   System.out.println(ib);
   ```

   >  思路: 循环将ib*10,将个位数变为十位数因为要容纳下一个数的个数加起来从而实现12345整数

   -   .:green_book: **<span id="秘籍">绝世秘籍</span>**: 推荐使用`""+变量`来将这个数据类型转换为字符串类型.

##### 1.4常用的成员方法(练熟,记住):deciduous_tree: 

| 方法声明                | 功能介绍                                 |
| ----------------------- | ---------------------------------------- |
| String toString(  )     | 返回字符串本身                           |
| byte[ ] getBytes(  )    | 将当前字符串内容转换为byte数组并返回     |
| char[ ] toCharArray(  ) | 用于将当前字符串内容转换为char数组并返回 |
| char charAt(int index)  | 方法charAt用于返回字符串指定位置的字符   |
| int length(  )          | 返回字符串字符序列的长度                 |
| boolean isEmpty(  )     | 判断字符串是否为空                       |

```java
//a与b的地址是在同一个位置中的所以结果为true
String a = "abc";
String b = "abc";
String s = "abc";
//使用+将两个字符串进行拼接,常量优化机制会与abc存储到和abc相同的位置中
String sss = "a"+"bc";
//字符串有常量优化机制而变量没有,这个s是一个变量所以进行比较时结果为false
String s1 = s+"bc";
System.out.println(a == b);//true
System.out.println(s == sss);//true
System.out.println(s == s1);//false
```

>  >  StringEman

```java
//1.请问下面的字符串中会创建几个对象,会存放在什么地方?
String srt = "abc"; // 1个对象,会存放在常量池中
String srt1 = new String("abc");// 2个一个常量池一个堆
String srt2 = new String("abc");
```



*  案例题目: 判断字符串 "上海自来水来自海上" 是否为回文并打印,所以回文是指一个字符串序列无论是从左向右还是从右向左读都是相同的句子

   ```java
    public static void main(String[]args) {
           String s = "上海自来水来自海上";
           int len = s.length() - 1;
           for(int i = 0;i < s.length() / 2;i ++){
               if(s.charAt(i) != s.charAt(len--)){
                   System.out.println(s+"-->不是回文");
                   return;
               }
           }
           System.out.println(s+"-->是回文");
    }
   ```

   

方法声明|功能介绍
---|---
int compareTo(String anotherString)|用于比较调用对象和参数对象的大小关系
int compareTolgnoreCase(String str)|不考虑大小写,也就是 'a' 和 'A' 是相等的关系

-  案例题目:编程实现字符串之间大小的比较并打印

   

   ```java
   String sr = "helloworld";
   System.out.println(sr.compareTo("abcde"));// 97 - 104 => 7
   System.out.println(sr.compareTo("healo"));// 97 - 108 => 11
   System.out.println(sr.compareTo("aelio"));// 97 - 104 => 7
   System.out.println(sr.compareTo("hello"));// 5
   System.out.println(sr.compareTo("helloworldworld"));// -5
   System.out.println(sr.compareToIgnoreCase("HELLOWORLD"));// 0
   ```

   

方法声明|功能介绍
---|---
String concat(String srt)|用于实现字符串的拼接
boolean contains(CharSequence s)|用于判断当前字符串是否包含参数指定的内容
String toLowerCase(  )|将字符串转换为小写形式
String toUpperCase(  )|将字符串转换为大写形式
String trim(  )|返回去掉前后空格的字符串(只能去掉半角空格)
String strip(  )|返回去掉全角/半角 前后空格的字符串
boolean startsWith(String prefix)|判断字符串是否以参数字符串开头
boolean startWith(String prefix,int toffset)|从指定位置开始是否以参数字符串开头
boolean endsWith(String suffix)|判断字符串是否以参数字符串结尾


-  :warning:需要注意的坑**:空格也算**😅[^这TM也行?]	

```java
String sr = " helloworld ";
boolean f = sr.startsWith("hell");
System.out.println(f);//false
f = sr.startsWith(" ");
System.out.println(f);//true
f = sr.endsWith("ld");
System.out.println(f);//false
f = sr.endsWith("ld ");
System.out.println(f);//true
```



方法声明|功能介绍
---|---
boolean equals(Object obj)|用于比较字符串内容是否相等并返回
int hashCode(  )|获取调用对象的哈希码值
boolean equalsIgnoreCase(String anotherString)|用于比较字符串是否相等并返回,不考虑大小写如: 'A' 和 'a' 是相等

-  使用上面方法实现 注册登入 系统模拟



```java
package month8fuxiD5;

import java.util.Scanner;

public class Userd {
    String zhang = "user";
    String mima = "123";
    public static void main(String[]args){
        System.out.println("请输入账号密码登入平台,如果您还没有该平台账号可以进行申请");
        System.out.println("登入按1,申请按2");
        Scanner sc = new Scanner(System.in);
        int s = sc.nextInt();
        Userd a = new Userd();
        switch(s){
            case 1:
                a.deng(a.zhang,a.mima);
                break;
            case 2:
                a.zhu();
                break;
            default:
                System.out.println("错误!");
                break;
        }
    }
    public void zhu(){
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入账号");
        zhang = sc.next();
        System.out.println("请输入密码");
        mima = sc.next();
        System.out.println("注册完成,您可以开始登入了");
        deng(zhang,mima);
    }
    public void deng(String zhang,String mima){
        Scanner sc = new Scanner(System.in);
        for(int i = 3;i > 0;i --) {
            System.out.print("账号:");
            String z = sc.next();
            System.out.print("密码:");
            String m = sc.next();
            //如果有些程序是运行用户输入的账号或密码有大小写的可以使用equalsIgnoreCase(String s)来进行判断输入的是否正确
            if(z.equals(zhang)&m.equals(mima)){
                System.out.println("登入成功!");
                return;
            }
            if(i == 1){
                System.out.println("抱歉,由于您多次错误,该账号由安全设定被冻结请联系客服方!");
            }else {
                System.out.println("账号或密码错误请重试,您还有" + (i - 1) + "次机会!");
            }
        }
    }
}
```



方法声明|功能介绍
---|---
int indexOf(int ch)|用于返回当前字符串中参数ch指定的字符第一次出现的下标
int indexOf(int ch,int fromlndex)|用于从fromlndex位置开始查找ch指定的字符
int indexOf(String str)|在字符串中检索str返回其第一次出现的位置,若找不到返回-1
int indexOf(String str,int fromlndex)|表示从字符串的fromlndex位置开始检索str第一次出现的位置
int lastIndexOf(int ch)|用于返回参数ch指定的字符最后一次出现的下标
int lastIndexOf(int ch,int fromlndex)|用于从fromlndex位置开始查找ch指定字符出现的下标
int lastIndexOf(String str)|返回str指定字符串最后一次出现的下标
int lastIndexOf(String str,int fromlndex)|用于从fromlndex位置开始反向搜索的第一处出现的下标

*  案例题目:

   ​				编写通用的代码可以查询字符串 "Good Good Study,Day Day Up!" 中所有 "Day" 出现的索引位置并打印出来

```java
String ar = "Good Good Study,Day Day Up!";
int x = ar.indexOf('g'); // -1 负一表示查找失败
System.out.println(x);
int x1 = ar.indexOf("Good");// 0
System.out.println(x1);
int x2 = ar.indexOf("Good",1);// 5 跳过字符串中第一个字母获取后面的要查找的匹配字符串
System.out.println(x2);
int x3 = ar.indexOf('D');// 16
System.out.println(x3);
int x4 = ar.indexOf("Day");//16 以字符串的第一个字母为下标返回
System.out.println(x4);
System.out.println("-------查找所有Day下标位置-------");
//查找所有Day下标位置
int pos = 0;
while((pos = ar.indexOf("Day",pos)) != -1){
    System.out.println(pos);
    pos += "Day".length();
}
```
打印结果:`-1
			0
			5
			16
			16
-------查找所有Day下标位置-------
			16
			20`



![image-20220829211406982](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081413501.png)

-  案例题目:

 提示用户从键盘输入一个字符串和一个字符,输出该字符(不含)后面的所有子字符串

```java
String ar = "Good Good Study,Day Day Up!";
int pos = ar.lastIndexOf("Good",5);
System.out.println(pos);
pos = ar.lastIndexOf("Good",4);
System.out.println(pos);
pos = ar.lastIndexOf("Good",6);
System.out.println(pos);
pos = ar.lastIndexOf("Day",20);
System.out.println(pos);
pos = ar.lastIndexOf("Day",18);
System.out.println(pos);
pos = ar.lastIndexOf("Day",15);
System.out.println(pos);
```



打印结果:`5
	    0
		5
		20
		16
		-1`



方法声明|功能介绍
---|---
String substring(int beginindex,int endlndex)|返回字符串中下标beginIndex(包括)开始到endindex(不包括)结束的子字符串
String substring(int beginIndex)|返回字符串中从下标beginIndex(包括)开始到字符串结尾的子字符串

-  案例题目:

​	  		 提示用户从键盘输入一个字符串和一个字符,输出该字符(不含)后面的所有子字符串

思路: 获取字符串的子字符串在字符串中第一次出现的位置,通过``substring``来将这个起始位置开始输出后面的字符串

```java
Scanner sc = new Scanner(System.in);
System.out.println("请输入第一个字符串");
String sr = sc.next();
System.out.println("请输入子字符串");
String sr1 = sc.next();
int x = sr.indexOf(sr1);
String tt = sr.substring(x + 1);
System.out.println("字符串后的子字符串为: " + tt);
```

#### 1.5 正则(规则)表达式的概念(了解):deciduous_tree: 

>  正则表达式本质就是一个 "规则字符串" 可以用于对字符串数据的格式进行验证,以及匹配,查找,替换等操作.该字符串通常使用^运算符作为开头标志,使用$运算符作为结尾标志,当然也可以省略.

1.5.1正则表达式的规则(了解)

正则表达式|说明
-|-
[abc]|可以出现a,b,c中任意一个字符
[^abc]|可以出现任何字符,除了a,bc的任意字符
[a-z]|可以出现a,b,c...,z中的任意一个字符
[a-zA-Z0-9]|可以出现a~z,A~Z,0~9中任意一个字符

正则表达式|说明
-|-
.|任意一个字符(通常不包括换行符)
\d|任意一个数字字符,相当于[0-9]
\D|任意一个非数字字符
\s|空白字符,相当于[\t\n\x0B\f\r]
\S|非空白字符
\w|任意一个单词字符,相当于[a-zA-Z_0-9]
\W|任意一个非单词字符

正则表达式|说明
-|-
X?|表示X可以出现一次或多次或一次也没有,也就是0~1次
X*|表示X可以出现零次或多次,也就是0~n次
X+|表示X可以出现一次或多次,也就是1~n次
X{n}|表示X可以出现恰好n次(多了也不行,少了也不行 )
X{n,}|表示X可以出现至少n次,也就是>=n次
X{n,m}|表示X可以出现至少n次,但是不超过m次,也就是>=n并且<=m次

##### 1.5.1正则表达式相关的方法(熟悉):evergreen_tree: 

| 方法名称                      | 方法说明                                                 |
| ----------------------------- | -------------------------------------------------------- |
| boolean matches(String regex) | 判断当前正在调用的字符串是否匹配参数指定的正则表达式规则 |

-  案例题目:

   使用正则表达式描述一下银行卡密码的规则: 要求是由6位数字组成

   ```java
   Scanner sc = new Scanner(System.in);
           //字符串规则:正则表达式由开头^由结尾$组成,中间为设定的规则:数字0-9之间大括号中6表示
           //只能是长度为6的否则为不是该规则的格式
   		//正则表达式只能对格式进行验证, 无法对数据内容的正确性进行检查,内容的正确性检查需要后台数据库查询
           String reg = "^[0-9]{6}$";
   	  //String reg = "[0-9]{6}";      ^$可以省略不写
        //String reg = "^\\d{6}$";  \d也可以表示0-9的数字字符,需要使用两个\因为一个为转义字符
           System.out.println("请输入银行卡密码");
           int i;
           for(i = 3;i > 0;){
               String d = sc.next();
               //使用matches方法对两个字符串进行判断规则是否相同
               if(d.matches(reg)){
                   System.out.println("格式正确");
                   break;
               }if(i == 1){
                   System.out.println("账户冻结请找客户联系方式: 112_139_151");
                   System.exit(0);
               }else{
                   i --;
                   System.out.println("格式错误!还有"+i+"次输入机会!");
               }
           }
   ```
   
   
   
   使用正则表达式描述一下QQ号码的规则: 要求是由非0开头的5~15位数组成
   
   ```java
   Scanner sc = new Scanner(System.in);
           //字符串规则:正则表达式由开头^由结尾$组成,中间为设定的规则:数字0-9之间大括号中6表示
           //只能是长度为6的否则为不是该规则的格式
           String reg = "^[1-9]\\d{4,14}$";
           System.out.println("请输入QQ号密码");
           int i;
           for(i = 3;i > 0;){
               String d = sc.next();
               //使用matches方法对两个字符串进行判断规则是否相同
               if(d.matches(reg)){
                   System.out.println("格式正确");
                   break;
               }if(i == 1){
                   System.out.println("账户冻结请找客户联系方式: 112_139_151");
                   System.exit(0);
               }else{
                   i --;
                   System.out.println("格式错误!还有"+i+"次输入机会!");
               }
           }
   ```
   
   
   
   使用正则表达式描述一下手机号码的规则: 要求是由1开头,第二位数是3,4,5,7,8中的一位,总共11位
   
   ```java
   Scanner sc = new Scanner(System.in);
           //使用正则表达式描述一下手机号码的规则: 要求是由1开头,第二位数是3,4,5,7,8中的一位,总共11位
           String reg = "^1[3,4,5,7,8]\\d{9}$";
           System.out.println("请输入手机号码");
           int i;
           for(i = 3;i > 0;){
               String d = sc.next();
               if(d.matches(reg)){
                   System.out.println("格式正确");
                   break;
               }if(i == 1){
                   System.out.println("账户冻结请找客户联系方式: 112_139_151");
                   System.exit(0);
               }else{
                   i --;
                   System.out.println("格式错误!还有"+i+"次输入机会!");
               }
           }
   ```
   
   
   
   描述身份证号码的规则: 总共18位,6位数字代表地区,4位数字代表年,2位数字代码月,2位数字代表日期,3位数字代表个人,最后一位可能数字也能是X
   
   ```java
   Scanner sc = new Scanner(System.in);
           //描述身份证号码的规则: 总共18位,6位数字代表地区
           //4位数字代表年,2位数字代码月,2位数字代表日期,3位数字代表个人,最后一位可能数字也能是X
           //小括号代表一组
           String reg = "^(\\d{6})(\\d{4})(\\d{2})(\\d{2})(\\d{3})([0-9|X])$";
           System.out.println("请输入身份证号:");
           int i;
           for(i = 3;i > 0;){
               String d = sc.next();
               //使用matches方法对两个字符串进行判断规则是否相同
               if(d.matches(reg)){
                   System.out.println("格式正确");
                   break;
               }if(i == 1){
                   System.out.println("账户冻结请找客户联系方式: 112_139_151");
                   System.exit(0);
               }else{
                   i --;
                   System.out.println("格式错误!还有"+i+"次输入机会!");
               }
           }
   ```
   
   

| 方法名称                                             | 方法说明                                                     |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| String[] split(String regex)                         | 参数regex为正则表达式,以regex所表示的字符串为分隔符,将字符串拆分成字符串数组 |
| String replace(char oldChar,char newChar)            | 使用参数newChar替换此字符串中出现的所有参数oldChar           |
| String replaceFirst(String regex,String replacement) | 替换此字符串匹配给定的正则表达式的第一个子字符串             |
| String replaceAll(String regex,String replacement)   | 将字符串中匹配正则表达式regex的字符串替换成replacement       |

```java
String ar = "123a456b789c10";
//\\d表示0-9的数字字符,+表示可以出现1或多次
String t = ar.replaceFirst("\\d+","#");
System.out.println(t);
//\\d表示0-9数字
String t1 = t.replaceAll("\\d","X");
System.out.println(t1);
```

## 尽可能的少定义全局变量:mag: 

编程语言中有一条不成文的铁律——尽可能的少定义全局变量

大致有两个原因，一难以控制，二占用内存

1.  全局变量不好控制，可以在任何地方进行读写，意味着可能会被不相干的程序改写
2.  全局变量占用内存的生命周期长，一般局部变量(定义在函数中的变量)，在函数调用完成后与之对应的执行环境会被推出执行栈，回收机制会每隔一段时间进行一次回收操作，释放不需要被占用的内存，执行环境出栈就是在告诉回收机制，"这些变量我不需要了,可以回收了"，而全局变量因为随时可以被任何程序在任何地方读写，所以回收机制很难统计何时需要释放全局变量所占用的内存，也就导致全局变量一般是在全局执行环境被销毁时才会释放
