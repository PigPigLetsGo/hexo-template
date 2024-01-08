---
title: Java值传递和引用传递的详解
categories:
    - [计算机学科,java,知识点]
tags:
    - 知识点
---


```java
public class Test1 {
    public static void main (String[]args) {
        int a = 8;//实参
        test(a);
    }
    public static void test (int a) {
        System.out.println(a);//形参
    }
}
结果: 8
```

==值传递:== **对形参的修改不会影响到实参 ** 

==引用传递:== **对形参的修改能够影响到实参**  

```java
public class Test1 {
    public static void main (String[]args) {
        int a = 8;//实参
        test(a);
        System.out.println("实参: "+a);
    }
    public static void test (int a) {
        a = 18;
        System.out.println("形参: "+a);//形参
    }
}
结果: 
形参: 18
实参: 8
```

形参的修改没有影响到实参 ==> 符合值传递

```java
public class Test2 {
    public static void main (String[]args) {
        Person p = new Person("张三");
        f(p);
        System.out.println("实参: "+p.name);
    }
    public static void f (Person p) {
        p.name = "李四";
        System.out.println("形参: "+p.name);
    }
}
结果:
形参: 李四
实参: 李四
```

形参的修改影响到了实参 ==> 符合引用传递

- 但是我们在方法中 引用p来new一个Person构造器传入值此时,实参和形参的值就不一样了

```java
public class Test2 {
    public static void main (String[]args) {
        Person p = new Person("张三");
        f(p);
        System.out.println("实参: "+p.name);
    }
    public static void f (Person p) {
        p = new Person("李四");
        System.out.println("形参: "+p.name);
    }
}
结果:
形参: 李四
实参: 张三
```

JVM中划分了好几块内存区域,其中有一个栈空间和堆空间,我们创建的所有对象都是在堆空间里,而基本数据类型和局部变量在栈中

![image_2023-04-05-09-47-04](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081443209.png)

当传递基本数据类型时是将数据创建了一个副本,传递到方法中,那自然形参修改不会影响到实参的值,符合==值传递== <font style="color:red">另一个栈空间是调用方法里面的形参属于该方法内的一个局部变量外部不可调用</font>。

![image_2023-04-05-09-48-44](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081443802.png)

操作对象时,对象是存放在堆中的,我们拿到的只是这个对象的引用,通过对象的引用就可以操作对象.可以将引用理解为遥控器,而对象就好像一个电视机,这也是为什么我们说Java的数据类型分两种 ==一种是基本数据类型== ,==一种是引用数据类型== 

![image_2023-04-05-09-50-18](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081444587.png)

当对==象引用传递给方法时==,其实是==创建了一个引用副本==此时,==形参==和==实参是两个遥控器==,==但都指向了同一个对象==,所以==通过形参引用操作对象时==,就==显得实参好像改变了==,但==实参本身是没有改变==的,因为==实参就是一个遥控器嘛,电视机内容是改变了,遥控器本身又没有改变==。

![image_2023-04-05-09-52-31](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081444289.png)

解释：`p = new Person("李四");`所以当我们将形参重新==实例化对象赋值时==,==实参是不会收到任何影响==的,此时==形参和实参已经指向不同的对象==。

![image_2023-04-05-09-55-08](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081444826.png)

这里可以看出Java完全就是==值传递==如果是==基本数据类型==那就是==复制一份值传递给形参==,如果是==引用类型==,那就==将引用复制一份传递给形参==,==形参始终拿到的都是一个副本==,<strong style="color:red">无论如何都无法通过形参改变实参,毕竟形参只是操作的副本而已</strong>。
