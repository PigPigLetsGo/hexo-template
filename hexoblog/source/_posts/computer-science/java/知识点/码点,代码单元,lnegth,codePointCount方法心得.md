---
title: 码点,代码单元,lnegth,codePointCount
categories:
    - [计算机学科,java,知识点]
tags:
    - API的区别
    - 知识点
---

# 码点,代码单元,lnegth,codePointCount

[TOC]

码点,代码单元,length(),codePointCount()

下面是我在阅读<<Java核心技术卷Ⅰ>>中的两段代码

### length()方法

![image-20220831143809050](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081434624.png)

`Returns the length of this string. The length is equal to the number of Unicode code units in the string.`

-  返回此字符串的长度,长度等于字符串中的Unicode==代码单元==数

### codePointCount()方法

![image-20220831143859921](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081434404.png)

`Returns the number of Unicode code points in the specified text range of this String.`

-  返回此字符串指定文本范围内的Unicode==码点==数

### length()方法和codePointCount()方法的区别

从返回值可以看出: length()方法返回的是代码单元,codePointCount()方法返回的是码点,而代码单元和码点究竟是什么呢?它们有什么区别呢?

length()|codePointCount()
-|-
返回此字符串的长度,长度等于字符串中的Unicode==代码单元==数|返回此字符串指定文本范围内的Unicode==码点==数

-  从下面这个例子的结果来看,码点和代码单元似乎没有区别

```java
public class Test {
    public static void main(String []args) {
		String greeting = "Hello";
		int n = greeting.length();
		int cpCount = greeting.codePointCount(0, greeting.length());
       	System.out.println(n);
		System.out.println(cpCount);
    }
}
```

打印结果:   `5` 

​				  `5` 

-  但是从这个例子就可以看出码点和代码单元的不同了,下面我们就来聊一下码点和代码单元

```java
String greeting = "Hello";
int n = greeting.length();//5
int cpCount = greeting.codePointCount(0, greeting.length());
System.out.println(n);//5
System.out.println(cpCount);//5
System.out.println("=-------------=");
String str = "😂";
int r = str.length();
System.out.println(r);//2
int x = str.codePointCount(0,str.length());
System.out.println(x);//1
```

打印结果: ![image-20220831144457496](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081434986.png)

### 码点

码点就是你实际看到的每一个字符,比如:a,1,%,😂等都算一个码点

>  码点(Code Point)
>
>  码点是指与一个编码表中的某个字符对应的值,在Unicode标准中,码点采用了十六进制书写,并加上前缀U+,例如U+0041就是拉丁字母A的码点,Unicode的码点可以分成17个代码平面,第一个代码平面成为基本多语言平面,包括码点从U+0000到U+FFFF的经典Unicode代码,其余的16个平面的码点为从U+10000到U+10FFFF,包括辅助字符
>
>  ----------  <<Java核心技术卷Ⅰ>>P33

### 代码单元

而代码单元就不一定是你实际看到的每一个字符,有可能你实际看到的字符是包含一个代码单元,也有可能包含两个代码单元,这是因为:

java的字符串由char类型序列组成,而char类型原本是用来表示单个字符的,但是由于Unicode编码的机制,16位的char类型已经无法满足描述所有的Unicode字符的需要了,那么有些Unicode字符就需要两个char值表示,则可对应上下文中的高亮字体:==一个代码单元时一个字符的编码==

>  代码单元(Code Unit)
>
>  UTF-16编码采用不同长度的编码表示所有Unicode编码,在基本多语言平面中,每个字符用1位表示,称为代码单元,辅助字符编码为一对连续的代码单元,采用这种编码对表示的各个值落入基本多语言平面中未用的2048个值范围内,通常称为替代区域,这样设计十分巧妙,我们可以从中迅速知道==一个代码单元时一个字符的编码==,还是辅助字符的第一或第二部分
>
>  ----------  <<Java核心技术卷Ⅰ>>P33

### 方法:

方法声明|功能介绍
-|-
int offsetCodePoints(int index,int codePointoffset)|返回此String中的索引,该索引从给定的index偏移codePointoffset码点,由index和codePointoffset给出的文本范围内的未配对代理计为每个代码点
int codePointAt(int index)|返回指定索引处的字符(Unicode代码点),索引引用char值(Unicode代码单位),范围从0到lenth() - 1
**StringBuilder方法**|
String appendCodePoint(int cp)|追加一个码点,并将其转化为一个或者两个代码单元并返回this

```java
		String atest = "abcABC😀a";
        if("abcABC😀a".equals(atest)) {
            int cp1 = atest.offsetByCodePoints(0,0);
            int cp4 = atest.offsetByCodePoints(0, 3);
            int cp5 = atest.offsetByCodePoints(0, 6);
            int cp7 = atest.offsetByCodePoints(0, 7);
            System.out.println("cp1 is "+cp1+"  cp4 is "+ cp4+"  cp5 is "+cp5+"  cp7 is "+cp7);
            int cp1unicode = atest.codePointAt(cp1);
            int cp4unicode = atest.codePointAt(cp4);
            int cp5unicode = atest.codePointAt(cp5);
            int cp7unicode = atest.codePointAt(cp7);
            System.out.println("a: "+cp1unicode+"  A: "+cp4unicode+"  😀: "+cp5unicode+"  a: "+cp7unicode);
        }
```

打印结果:![image-20220904101620196](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081434856.png)

#### offsetByCodePoints

`public int offsetByCodePoints(int index,int codePointoffset)`

这里的index就是你指定的任意第i个码点,假如你想知道距离第i个码点x个码点,(x可正可负)是相对于第0个码点第几个码点,则可以用offsetByCodePoints(i,x)得到的你想要的值,比方说

```java
String astring = "abcdABCD";
int cpcount = astring.offsetByCodePoints(7,-4)//从0开始也就是d
//对应ACSII码值也就是100
```

cpcount的值应该是3(即表示d距离a 3个码点,也就是距离第0个位置3个位置)

#### codePointAt

`public int codePointAt(int index)`

还是以上面那个例子,d距离a,有3个位置,这个3通过offsetByCodePoints()得到,并且就可以看成是一个索引值,通过它你就能找到对应位置上是d

```java
        String astring = "abcdABCD";
        int cpcount = astring.offsetByCodePoints(7,-4);//从0开始也就是d
//对应ACSII码值也就是100
        int dddunicode = astring.codePointAt(cpcount);
        System.out.println(dddunicode);
```

dddunicode = 100;这其实是ASCII码值

#### appendCodePoint

`String appendCodePoint(int cp)`

追加一个码点,并将其转化为一个或者两个代码单元并返回this

```java
		String str = "hello";
        StringBuilder strbud = new StringBuilder();
        strbud.append(str);
        System.out.println(strbud.appendCodePoint(97));
```

打印结果: ![image-20220904103852797](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081434104.png)
