---
title: JVM概述
categories:
    - [计算机学科,java,JVM]
tags:
    - jvm
    - java
---

# JVM内存结构

## 什么是JVM

**Java Virtual Machine**：Java虚拟机，用来保证Java语言跨平台

Java虚拟机可以看做是一台抽象的计算机，如同真实的计算机那样，它有自己的指令集以及各种运行内存区域

Java虚拟机与Java语言并没有必然的联系，它只与特定的二进制文件格式 (class文件格式) 所关联

Java虚拟机就是一个==字节码翻译器==，它将字节码文件翻译成各个系统对应的机器码，确保字节码文件能在各个系统正确运行

## 为什么要学习JVM

我们知道一个java应用程序上线后，肯定会时不时的出现一些问题，除去网络/系统本身的问题很多时候Java应用程序也会出现问题都是Java的虚拟机内存出现的问题，要么是内存溢出了，要么是GC停乏 导致响应慢。

如何解决这些问题呢？

首先必须学会看懂日志。那么就必须能看懂==GC日志==，这是Java虚拟机中的一部分。看懂了GC日志就要明白什么是 新生代，老年代，元空间 (永久代)等。这些就是java虚拟机的==内存模型==，懂了内存模型就要知道java虚拟机是如何==垃圾回收==的。它们使用的垃圾回收的算法是怎么样的，它们有何优缺点。接下来就是各种垃圾回收的特性。

这一切就是有必然的联系的。

## JVM体系结构

![在这里插入图片描述](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311171635830.jpeg)

如上图所示，JVM分为三个主要的子系统。

-  Class Loader SubSystem ：类加载器 子系统
-  Runtime Data Areas ：运行时数据区 子系统
-  Execution Engine ：执行引擎 子系统

### Class Loader SubSystem

Java的动态类加载功能由类加载器子系统处理

它是在运行时首次引用类的时候，加载，连接，并初始化类文件。

![image-20231117194324173](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311171943816.png)

1、在加载这个地方，它是通过==引导类加载器==，==扩展类加载器==，==应用程序类加载器==。这三个类加载器帮助完成加载。

2、加载完毕后进行连接操作。而在连接这里又分为三个步骤：验证，准备，解析。

==验证阶段==：字节码验证程序将验证生成的字节码是否正确，如果验证失败将收到，验证错误。

==准备阶段==：为所有静态变量分配内存，并分配默认值。

==解析阶段==：用方法去对原始引用代替所有符号内存引用

3、初始化 (类加载的最后阶段)。

此处所有的静态变量将被分配原始值并且执行静态代码块

## Runtime Data Areas

**主要组成部分**：方法区 (Method Area)，堆区 (Heap Area)，虚拟机栈 (Stack Area)，PC寄存器 (PC Registers)，本地方法栈 (Native Method Stack)。

---

**方法区**：存储所有类级别的数据，每个java虚拟机只有一个方法区所以它是共享的资源

**堆区**：所有对象及其对应的实例变量和数组都将存储在此处，而每个虚拟机堆区也只有一个也是共享资源的

>  <font color='red'>注意</font>：由于，方法区和堆区都是线程==共享==的所以==不是线程安全==的。

**虚拟机栈**：对于每个线程将会创建一个单独的运行时栈，而对于每个方法调用我们在栈内存会创建一个条目称之为 ==栈帧== 所有的==局部变量==都会在栈内存创建由于它==不是共享的资源==所以它==是线程安全==的

**PC寄存器**：每个线程都有它自己的PC寄存器，所以它也是线程安全的，作用是保存当前正在执行的指令。一旦指令执行，PC寄存器将更新到下一条指令

**本地方法栈**：保存本机的方法信息，而对于每个线程它将单独创建本地方法栈所以它也是线程安全的 

## Execution Engine

**解释器** (Interpreter)

-  作用：读取字节码对其进行解释并逐一执行，解释器解释字节码的速度较快，但是它执行速度较慢
-  致命缺点：当一个方法被多次调用时，它每次都需要解释，所以这时就出现了如下JIT Compiler

**即时编译器** (JIT Compiler)

-  作用：解决 解释器的 缺点的。当它发现重复代码时，将采用即时编译器进行编译整个字节码并将其更改为本地代码，此本地代码将直接用于重复的方法调用从而提高系统性能
-  它都做了什么呢？
   -  中间代码生成 (intermediate code generation)：生成中间代码
   -  代码优化器 (CodeOptimizer)：优化 中间代码生成器 所生成的代码
   -  目标代码生成器 (TargeCodeGenerator)：生成机器码
   -  分析器 (Profile)：查看这个方法是否被多次调用

**垃圾回收器** (GarbageCollction)

-  作用：收集或者删除未引用的对象，可以调用system.gc来触发垃圾回收，但是不能保证执行垃圾收集器仅收集new关键字创建的对象，因此如果没有通过new创建的对象则可以使用finalize方法来执行清理

**本地方法接口** (Native Method Interface (JNI))

-  作用：它与本地方法库进行交互 (Native Method Litxary) 并提供执行引擎所需要的本机库

**本地方法库** (Native Method Litxary)

-  作用：提供本地库，配合本地方法接口进行交互

## JVM内存结构

Java SE 8版本的虚拟机规范地址：https://docs.oracle.com/javase/specs/jvms/se8/html/index.html

<center>Jvm内存体系结构图</center> 

![image-20231118083917765](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311180839936.png)

**程序计数器**：

冯若依曼，计算机体积结构的主要内容之一，就是程序预存储计算机自动执行。处理器要执行的程序都是以二进制的代码序列方式存储在计算机的存储器中。

处理器将这些代码逐条的取到处理器中再编译执行以完成整个程序的执行，为了保证程序能够连续的执行下去CPU必须去用某些手段来确定下一条取值指定的地址，程序计数器正是起到这种作用。又被称为 指令计数器

==作用==：保存当前执行指令的地址，一旦指令执行，程序计数器将更新到下一条指令

理解作用这句话看如下图所示：

通过这张图来讲解

![image-20231118090303236](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311180903300.png)

这点内容是怎么来的呢？

下面通过一段java代码来解释上图内容怎么来的：

```java
public class Main {
    public static void main(String[] args) {
        // 创建PrintStream对象然后通过对象调用输出语句打印12345
        PrintStream out = System.out;
        out.println(1);
        out.println(2);
        out.println(3);
        out.println(4);
        out.println(5);
    }
}
```

运行一下产生.class编译文件，该文件在out目录下 通过terminal进入这个目录下找到.class文件然后执行

`javap -v Main.class`即可输出反编译的内容如下：

`javap` 就是jdk自带的反编译工具，它的作用就是根据字节码文件反编译出当前对应的汇编指令，本地变量表，异常表，代码行迁移量，常量池等信息。

```java
bash:>> javap -v .\Main.class
Classfile /E:/Demo/JavaSEJVM8/JavaSEJVM8/out/production/JavaSEJVM8/Main.class
  Last modified 2023-11-18; size 532 bytes
  MD5 checksum ed0f9435459e8422980820c711db3bcc
  Compiled from "Main.java"
public class Main
  minor version: 0
  // jdk版本对应的是jdk8
  major version: 52
  flags: ACC_PUBLIC, ACC_SUPER
// 常量池
Constant pool:
   #1 = Methodref          #5.#21         // java/lang/Object."<init>":()V
   #2 = Fieldref           #22.#23        // java/lang/System.out:Ljava/io/PrintStream;
   #3 = Methodref          #24.#25        // java/io/PrintStream.println:(I)V
   #4 = Class              #26            // Main
   #5 = Class              #27            // java/lang/Object
   #6 = Utf8               <init>
   #7 = Utf8               ()V
   #8 = Utf8               Code
   #9 = Utf8               LineNumberTable
  #10 = Utf8               LocalVariableTable
  #11 = Utf8               this
  #12 = Utf8               LMain;
  #13 = Utf8               main
  #14 = Utf8               ([Ljava/lang/String;)V
  #15 = Utf8               args
  #16 = Utf8               [Ljava/lang/String;
  #17 = Utf8               out
  #18 = Utf8               Ljava/io/PrintStream;
  #19 = Utf8               SourceFile
  #20 = Utf8               Main.java
  #21 = NameAndType        #6:#7          // "<init>":()V
  #22 = Class              #28            // java/lang/System
  #23 = NameAndType        #17:#18        // out:Ljava/io/PrintStream;
  #24 = Class              #29            // java/io/PrintStream
  #25 = NameAndType        #30:#31        // println:(I)V
  #26 = Utf8               Main
  #27 = Utf8               java/lang/Object
  #28 = Utf8               java/lang/System
  #29 = Utf8               java/io/PrintStream
  #30 = Utf8               println
  #31 = Utf8               (I)V
{
  // 构造器
  public Main();
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: return
      LineNumberTable:
        line 10: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   LMain;
  // main方法
  public static void main(java.lang.String[]);
    descriptor: ([Ljava/lang/String;)V
    flags: ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=2, args_size=1
         0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
         3: astore_1
         4: aload_1
         5: iconst_1
         6: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
         9: aload_1
        10: iconst_2
        11: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
        14: aload_1
        15: iconst_3
        16: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
        19: aload_1
        20: iconst_4
        21: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
        24: aload_1
        25: iconst_5
        26: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
        29: return
      LineNumberTable:
        line 12: 0
        line 13: 4
        line 14: 9
        line 15: 14
        line 16: 19
        line 17: 24
        line 18: 29
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      30     0  args   [Ljava/lang/String;
            4      26     1   out   Ljava/io/PrintStream;
}
SourceFile: "Main.java"
```

我们重点看main方法下的内容它跟上图中的内容是一样的，我们写的java源代码如下：

```java
public static void main(String[] args) {
   // 创建PrintStream对象然后通过对象调用输出语句打印12345
   PrintStream out = System.out;
   out.println(1);
   out.println(2);
   out.println(3);
   out.println(4);
   out.println(5);
}
```

我们写的代码只有上面点内容，但是真正编译执行的是如下内容：

```java
// main方法
public static void main(java.lang.String[]);
descriptor: ([Ljava/lang/String;)V
              flags: ACC_PUBLIC, ACC_STATIC
              Code:
              stack=2, locals=2, args_size=1
              0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
              3: astore_1
              4: aload_1
              5: iconst_1
              6: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
              9: aload_1
              10: iconst_2
              11: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
              14: aload_1
              15: iconst_3
              16: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
              19: aload_1
              20: iconst_4
              21: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
              24: aload_1
              25: iconst_5
              26: invokevirtual #3                  // Method java/io/PrintStream.println:(I)V
              29: return
```

**解释程序计数器的作用**：程序执行前会将程序指令的起始地址也就是程序第一条指令所在的内存单元存储程序计数器，可以将字节码行号指示器理解为内存地址也就是首先把0存入程序计数器。接下来CPU就会按照程序计数器的指示来执行它到程序计数器看到是0那么它就会从字节码为0的地址开始执行。同时一旦开始从0执行的时候程序计数器就会修改自己的内容它的内容将会变成下一条将要取址的指令地址此时它会变成3

当0位置的内容执行完毕之后CPU再从程序计数器里面去读，读到3就从字节码为3的地方开始执行，然后程序计数器的值修改为4

![image-20231118181544753](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181815851.png)

## 虚拟机栈

![image-20231118181816050](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181818246.png)

**栈的特点**：先进后出。

每个程序在执行的时候都有一个进程，而每个进程最小单元是一个线程，线程在执行的时候有一个内存空间，这个内存空间就是==虚拟机栈==。

每个线程都有一个对应的虚拟机栈，所以它是线程私有的，而程序的执行是通过调用方法来实现的而每个方法对应一个栈帧。也就是虚拟机栈中有一个或多个栈帧，而每个栈帧包含了局部变量表，操作数栈，动态链接，方法返回值等信息。

下面通过代码来看虚拟机栈，栈帧的信息：

```java
public class Demo {
    public static void main(String[] args) {
*       int a = 10;
        int b = 20;
        method(a, b);
    }

    private static void method(int a, int b)
    {
        a += 10;
        b += 10;
        method2(a, b);
    }

    private static void method2(int a, int b)
    {
        int c = a + b;
        System.out.println(c);
    }
}
```

*号 为断点处，启动debug

红色线框的地方可以看成是虚拟机栈

![image-20231118182955267](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181829215.png)

每个方法的调用会有一个栈帧，当main函数中往下执行调用method后就会出现一个栈帧

有两个栈帧而这整个的我们可以看成是虚拟机栈

![image-20231118183212305](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181832757.png)

当调用method2方法后就会出现第三个栈帧

每个栈帧中的信息不同，因为局部变量只作用于它当前的函数中

![image-20231118183412161](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181834138.png)

这个method2函数马上调用结束，它就会从虚拟机栈中弹出。

![image-20231118183641250](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181836416.png)

接着method弹出

![image-20231118183711524](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181837721.png)

最后main函数弹出程序结束

![image-20231118183740942](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181837063.png)

>  **总结**：
>
>  每个线程运行时所需要的内存空间，称为虚拟机栈
>
>  每个栈由多个栈帧 (Frame) 组成，对应着每次方法调用时所占用的内存
>
>  每个线程只能有一个活动栈帧，对应着当前正在执行的那个方法

## 栈帧

**组成**：局部变量表，动态链接，方法返回地址

![image-20231118184553513](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181845482.png)

**局部变量表**：

存放局部变量的列表：

一个局部变量可以保存类型为double，byte，char，short，float，refernce和returnAddress的数据两个局部变量可以保存一个类型为long和double的数据

局部变量使用索引来进行定位访问，第一个局部变量的索引值为零。



**操作数栈**：

也称为操作栈，它是一个后进先出的栈



当一个方法刚刚开始执行时，其操作数栈是空的

随着方法执行和字节码指令的执行，会从局部变量表或对象实例的字段中复制常量或变量写入到操作数栈，再随着计算的进行将一个完整的方法执行期间往往包含多个这样出栈/入栈的过程

简单理解，操作数栈是线程实际的操作台



**动态链接**：

简单的理解为指向运行时常量池的引用



在class文件里面，描述一个方法调用了其它方法，或者访问其成员变量是通过符号引用来表示的，动态链接的作用就是将这些符号引用所表示的方法转换为实际方法的直接引用



**方法返回地址**：

方法调用的返回，包括正常返回 (有返回值) 和异常返回 (没有返回值)，不同的返回类型有不同的指令



无论方法采用何种方式退出，在方法退出后都需要返回到方法被调用的位置，程序才能继续执行，方法返回时可能需要在当前栈帧中保存一些信息，用来帮助它恢复它的上层方法执行状态

## 栈内存溢出

栈帧过多导致栈内存溢出

栈帧过大导致栈内存溢出

栈内存大小是可以通过一个参数来指定的

下面是官网的文档说明：https://docs.oracle.com/en/java/javase/11/tools/java.html#GUID-3B1CE181-CD30-4178-9602-230B800D4FAE

![image-20231118191506910](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181915979.png)

通过下面代码来进行演示：

```java
/**
 * java.lang.StackOverflowError
 * -Xss256k：次数减少
 * -Xss10m：次数变多
 */
public class Demo01 {
    private static int count;

    public static void method()
    {
        count++;
        method();
    }

    public static void main(String[] args) {
        try
        {
            method();
        }catch(Throwable e)
        {
            e.printStackTrace();
            System.out.println(count);
        }
    }
}
```

运行结果：

可以看到堆栈溢出了

```
java.lang.StackOverflowError
	at Demo01.method(Demo01.java:12)
	... 此处省略几万行
	at Demo01.method(Demo01.java:12)
23220

Process finished with exit code 0
```

设置栈内存大小

![image-20231118192148715](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181921134.png)

再次运行后查看count的值

可以看到比上次少了很多

```
2729
```

我们再设置一个10m的栈内存来查看下结果

可以看到这次调用的次数比较多值比较大

```
600803
```

## 栈帧过大导致栈内存溢出

运行如下java代码：

使用到的第三方api如下

![image-20231118194332010](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181943254.png)

```java
public class Demo02 {
    public static void main(String[] args) throws JsonProcessingException {
        Dept d = new Dept();
        d.setName("yanfa");
        Emp e1 = new Emp();
        e1.setName("lisi");
        e1.setDept(d);
        Emp e2 = new Emp();
        e2.setName("wangwu");
        e2.setDept(d);
        d.setEmps(Arrays.asList(e1, e2));
        ObjectMapper mapper = new ObjectMapper();
        System.out.println(mapper.writeValueAsString(d));
    }
}
class Emp
{
    private String name;
    private Dept dept;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Dept getDept() {
        return dept;
    }

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "name='" + name + '\'' +
                ", dept=" + dept +
                '}';
    }
}

class Dept
{
    private String name;
    private List<Emp> emps;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public List<Emp> getEmps() {
        return emps;
    }

    public void setEmps(List<Emp> emps) {
        this.emps = emps;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "name='" + name + '\'' +
                ", emps=" + emps +
                '}';
    }
}
```

运行结果如下

报错了，说堆栈溢出异常了。

```
*** java.lang.instrument ASSERTION FAILED ***: "!errorOutstanding" with message transform method call failed at JPLISAgent.c line: 844
*** java.lang.instrument ASSERTION FAILED ***: "!errorOutstanding" with message transform method call failed at JPLISAgent.c line: 844
Exception in thread "main" com.fasterxml.jackson.databind.JsonMappingException: Infinite recursion (StackOverflowError) (through reference chain: Dept["emps"]->java.util.ArrayList[0]->Emp["dept"]->Dept["emps"]->java.util.ArrayList[0]->Emp["dept"]->Dept["emps"]->java.util.ArrayList[0]->Emp["dept"]->Dept["emps"]->java.util
```

为何jackson会报这种错误呢？如下分析：

![image-20231118194625268](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311181946865.png)

jackson在做转换的时候，找到这个Dept的时候首先要去看所有的emps。当它找到第一个员工lisi的时候而lisi里面又有Dept而lisi它的Dept还是上次的yanfa而这个Dept又要找emps而yanfa下的Dept找到的第一个emps还是lisi，而lisi中有一个Dept这个还是yanfa又要找emps而第一个又是lisi。进入了死循环的状态。

那么如何解决这种异常呢？我们可以使用注解@JsonIgnore来忽略private Dept dept;字段的json转换

```java
class Emp
{
    private String name;
    @JsonIgnore
    private Dept dept;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Dept getDept() {
        return dept;
    }

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "name='" + name + '\'' +
                ", dept=" + dept +
                '}';
    }
}
```

运行查看结果：

这次就没有报错了

```
{"name":"yanfa","emps":[{"name":"lisi"}]}
```

## 本地方法栈

![image-20231118200949688](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311182009801.png)

我们在idea中查看什么是本地方法栈

搜索Object

![image-20231118201042315](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311182010314.png)

在Object类中如下就是本地方法栈

为什么呢？因为它被native修饰，被native修饰的方法就是本地方法

![image-20231118201126028](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311182011731.png)

本地方法栈的功能和特点类似于虚拟机栈，也是线程私有的

**不同的是**：

本地方法栈服务的对象是JVM执行的native方法，而虚拟机栈服务的是JVM执行的Java方法



如何去服务native方法？

native方法使用什么语言实现？

怎么组织像栈帧这种为了服务方法的数据结构？

虚拟机规范并未给出强制规定，因此不同的虚拟机可以进行自由实现

## 堆

![image-20231118202259050](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311182023550.png)

**作用**：

堆是用于存放对象内存区域

**特点**：

1、堆是被所有线程共享的一块内存区域，在虚拟机启动时创建

2、堆的区域是用来存放对象实例的，因此也是垃圾收集器管理的主要区域

3、堆在逻辑上划分为 “新生代” 和 “老年代” ，新生代分为Eden区，ServivorFrom，ServicorTo三个区

4、堆一般实现成大小是可扩展的，使用 “-Xms” 与 “-Xmx” 控制堆的最小与最大内存

## 堆内存溢出

代码演示：

```java
public class Demo03 {
    public static void main(String[] args) {
        int count = 0;
        try
        {
            ArrayList<String> list = new ArrayList<>();
            String s = "itheima";
            for (int i = 0; i < 20; i++)
            {
                list.add(s);
                s += s;
                count++;
            }
//            while(true)
//            {
//                list.add(s);
//                s += s;
//                count++;
//            }
        }
        catch (Throwable e)
        {
            e.printStackTrace();
            System.out.println(count);
        }
    }
}
```

运行结果

```
什么都没有
```

没有报错，也什么也没有输出，我们设置下堆内存大小

-Xmx8m大小

![image-20231119084146609](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311190841695.png)

再次运行后就得到一串报错信息

循环到了16就抛出的异常

报错信息是 堆内存溢出了

```
java.lang.OutOfMemoryError: Java heap space
	at java.util.Arrays.copyOfRange(Arrays.java:3664)
	at java.lang.String.<init>(String.java:207)
	at java.lang.StringBuilder.toString(StringBuilder.java:413)
	at Demo03.main(Demo03.java:20)
16
```

我们将堆内存设置100m看看结果：

![image-20231119125752678](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311191257715.png)

运行结果：

此时可以承受循环20次的量了

```
没有任何信息
```

那我们把while(true) 给打开，把循环20次的给注释掉看看运行结果：

count从0开始计数，也就是到了21就报错了，只需要比循环20次多一步就会报错。

```
java.lang.OutOfMemoryError: Java heap space
	at java.util.Arrays.copyOf(Arrays.java:3332)
	at java.lang.AbstractStringBuilder.ensureCapacityInternal(AbstractStringBuilder.java:124)
	at java.lang.AbstractStringBuilder.append(AbstractStringBuilder.java:448)
	at java.lang.StringBuilder.append(StringBuilder.java:142)
	at Demo03.main(Demo03.java:26)
20
```

>  **总结**：
>
>  **错误提示**：
>
>  java.lang.OutOfMemoryError：Java heap space
>
>  **错误原因**：
>
>  内存真不够，通过调整堆内存大小解决
>
>  存在死循环，通过修改代码解决

## 堆内存诊断

jps：查看当前系统中有哪些java进程

jmap 工具：查看堆内存占用情况 (某一个时刻)

使用方式：`jmap -heap 进程id`

jconsole 工具：图形界面的，内置java性能分析器，多功能的检测工具，可以连续监测

**代码演示**：

```java
public class Demo04 {
    public static void main(String[] args) throws InterruptedException {
        System.out.println("1...");
        Thread.sleep(30000);
        byte[] bytes = new byte[1024 * 1024 * 10]; // 10mb
        System.out.println("2...");
        Thread.sleep(10000);
        bytes = null;
        System.gc();
        System.out.println("3...");
        Thread.sleep(1000000);
    }
}
```

首先启动程序后使用jps命令查看当前的java进程

可以看到当前程序对应的进程id是14148

```shell
bash:>> jps
5760 
14148 Demo04
5656 Launcher
16396 Jps
```

然后使用jmap -heap 14148命令，等待线程2出现打印后再执行jmap -heap 14148，等待线程3打印后再执行jmap -heap 14148查看它们每一个时刻的堆内存情况

```shell
bash:>> jmap -heap 14148
Attaching to process ID 14148, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.381-b09

using thread-local object allocation.
Parallel GC with 8 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   MaxHeapSize              = 5326766080 (5080.0MB)
   NewSize                  = 111149056 (106.0MB)
   MaxNewSize               = 1775239168 (1693.0MB)
   OldSize                  = 222298112 (212.0MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
PS Young Generation
Eden Space:
   capacity = 83886080 (80.0MB)
   used     = 6710968 (6.400077819824219MB)
   free     = 77175112 (73.59992218017578MB)
   8.000097274780273% used
From Space:
   capacity = 13631488 (13.0MB)
   used     = 0 (0.0MB)
   free     = 13631488 (13.0MB)
   0.0% used
To Space:
   capacity = 13631488 (13.0MB)
   used     = 0 (0.0MB)
   free     = 13631488 (13.0MB)
   0.0% used
PS Old Generation
   capacity = 222298112 (212.0MB)
   used     = 0 (0.0MB)
   free     = 222298112 (212.0MB)
   0.0% used

3178 interned Strings occupying 260944 bytes.
 jmap -heap 14148
Attaching to process ID 14148, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.381-b09

using thread-local object allocation.
Parallel GC with 8 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   MaxHeapSize              = 5326766080 (5080.0MB)
   NewSize                  = 111149056 (106.0MB)
   MaxNewSize               = 1775239168 (1693.0MB)
   OldSize                  = 222298112 (212.0MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
PS Young Generation
Eden Space:
   capacity = 83886080 (80.0MB)
   used     = 17196744 (16.40009307861328MB)
   free     = 66689336 (63.59990692138672MB)
   20.5001163482666% used
From Space:
   capacity = 13631488 (13.0MB)
   used     = 0 (0.0MB)
   free     = 13631488 (13.0MB)
   0.0% used
To Space:
   capacity = 13631488 (13.0MB)
   used     = 0 (0.0MB)
   free     = 13631488 (13.0MB)
   0.0% used
PS Old Generation
   capacity = 222298112 (212.0MB)
   used     = 0 (0.0MB)
   free     = 222298112 (212.0MB)
   0.0% used

3179 interned Strings occupying 260992 bytes.
 jmap -heap 14148
Attaching to process ID 14148, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 25.381-b09

using thread-local object allocation.
Parallel GC with 8 thread(s)

Heap Configuration:
   MinHeapFreeRatio         = 0
   MaxHeapFreeRatio         = 100
   MaxHeapSize              = 5326766080 (5080.0MB)
   NewSize                  = 111149056 (106.0MB)
   MaxNewSize               = 1775239168 (1693.0MB)
   OldSize                  = 222298112 (212.0MB)
   NewRatio                 = 2
   SurvivorRatio            = 8
   MetaspaceSize            = 21807104 (20.796875MB)
   CompressedClassSpaceSize = 1073741824 (1024.0MB)
   MaxMetaspaceSize         = 17592186044415 MB
   G1HeapRegionSize         = 0 (0.0MB)

Heap Usage:
PS Young Generation
   0.0% used
To Space:
   capacity = 13631488 (13.0MB)
   used     = 0 (0.0MB)
   free     = 13631488 (13.0MB)
   0.0% used
PS Old Generation
   capacity = 222298112 (212.0MB)
   used     = 989040 (0.9432220458984375MB)
   free     = 221309072 (211.05677795410156MB)
   0.44491605938605544% used

3165 interned Strings occupying 260000 bytes.
```

进行解释：

线程1执行时通过jmap获取的堆内存的占用情况如下：

使用了6兆字节的内存

```
Eden Space:
   capacity = 83886080 (80.0MB)
   used     = 6710968 (6.400077819824219MB)
```

线程2执行时获取的堆内存的占用情况如下：

使用了16兆字节

```
Eden Space:
   capacity = 83886080 (80.0MB)
   used     = 17196744 (16.40009307861328MB)
```

线程3情况如下：

使用gc垃圾回收机制进行了回收使用了1兆字节

```
Eden Space:
   capacity = 83886080 (80.0MB)
   used     = 1677744 (1.6000213623046875MB)
```

如果我们每一时刻使用jmap -heap 进程id 来进行查看太麻烦了所以可以使用命令：`jconsole`工具来进行查看，它能连续监测

首先还是一样将程序启动起来。

接着在控制台输入`jconsole`

选择当前的程序然后进行连接。

![image-20231119134310164](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311191343495.png)

接着选择不安全的连接。

![image-20231119134412410](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311191344880.png)

可以看到堆内存的使用情况，当线程3执行时内存直线下降就是被垃圾回收了

![image-20231119134529307](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311191346613.png)

## 方法区

![image-20231119134746888](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311191347546.png)

**作用**：存储每个类的结构

例如运行时常量池，字段和方法数据，以及方法和构造函数的代码，包括用于实例初始化以及接口初始化的特殊方法等

### jdk6与jdk8的方法区的不同

![image-20231119135249121](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311191352108.png)

## 方法区内存溢出

代码演示：

```java
/**
 * 元空间，默认使用系统内存，一般跟物理内存有关，一般很难出现问题
 * 现在加一个参数，设置元空间内存
 * 演示元空间内存溢出 java.lang.OutOfMemoryError: Metaspace
 * -XX:MaxMetaspaceSize=8m
 * 设置大小，警用指针压缩
 * -XX:MaxMetaspaceSize=10m -XX:-UseCompressedOops
 *
 * -XX:MaxMetaspaceSize=8m -XX:MaxMetaspaceSize=10m -XX:-UseCompressedOops
 */
public class Demo05 extends ClassLoader {
    public static void main(String[] args) {
        int count = 0;
        try {
            Demo05 d = new Demo05();
            for (int i = 0; i < 20000; i++, count++) {
                // 作用：生成类的二进制字节码
                ClassWriter cw = new ClassWriter(i);
                // 版本号，方法修饰符，类名，包名，父类，接口
                cw.visit(Opcodes.V1_8, Opcodes.ACC_PUBLIC, "Class" + i, null, "java/lang/Object", null);
                byte[] code = cw.toByteArray();
                // 执行类的加载器
                d.defineClass("Class" + i, code, 0, code.length);
            }
        } finally {
            System.out.println(count);
        }
    }
}
```

运行结果如下：

```
20000
```

设置元空间大小为8m

设置指令：`-XX:MaxMetaspaceSize=8m -XX:MaxMetaspaceSize=10m -XX:-UseCompressedOops`

![image-20231119174832359](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311191749044.png)

再次运行查看结果：

```
9344
Exception in thread "main" java.lang.OutOfMemoryError: Metaspace
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:756)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:635)
	at Demo05.main(Demo05.java:26)
```

### 测试jdk6的代码

**代码演示**：

```java
/**
 * 演示永久代内存溢出 java.lang.OutOfMemoryError: permGen space
 * -XX:MaxPermSize=8m
 */
public class Demo06 extends ClassLoader {
    public static void main(String[] args) {
        int j = 0;
        try {
            Demo06 test = new Demo06();
            for (int i = 0; i < 20000; i++, j++) {
                ClassWriter cw = new ClassWriter(i);
                cw.visit(Opcodes.V1_6, Opcodes.ACC_PUBLIC, "Class" + i, null, "java/lang/Object", null);
                byte[] code = cw.toByteArray();
                test.defineClass("Class" + i, code, 0, code.length);
            }
        } finally {
            System.out.println(j);
        }
    }
}
```

运行结果如下：

```
20000
```

设置jdk6的方法区永久代最大内存大小-XX:MaxPermSize=8m

![image-20231119205837638](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311192058810.png)

运行结果如下：

![image-20231119205921719](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202311192059392.png)

>  **总结**：
>
>  java8中演示元空间内存溢出：
>
>  java.lang.OutOfMemoryError：Metaspace
>
>  -XX:MaxMetaspaceSize=10m -XX:-UseCompressedOops
>
>  java6中演示永久代内存溢出
>
>  java.lang.OutOfMemoryError：PermGen space
>
>  -XX:MaxPermSize=8m
