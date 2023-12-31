---
title: 子类实例化是否会实例化父类？
categories:
    - [计算机学科,java,知识点]
tags:
    - 对象
    - 知识点
---

## 子类实例化是否会实例化父类？

>  不会。父类在子类实例化过程中是并没有被实例化，java中new子类没有实例化父类，只是调用父类的构造方法初始化了，子类从父类继承来的属性，这个调用是子类的对象调用的父类的构造方法，而子类自己的构造方法完成对自己属性的初始化（这里的初始化是指我们在内存分配完了，虚拟机初始化之后，我们按自己的要求进行的初始化）。

## 子类对象实例化的全过程

1.  当最底层子类实例化对象时，它的父类，父类的父类...到Object类的所有类的构造器都会被调用，只不过当一个类拥有多个构造器时，调用的是其中一个。

2.  子类构造器内，默认调用父类构造器：super();当有this关键字时，就不调用父类构造器了，就会调用同一个类内的其他构造器，所以一个类当有n个构造器时，仅允许最多有n-1个构造器内使用this关键字，最少有一个构造器去调用上层父类的构造器。
3.  当父类重载一个构造器，则默认的无形参构造器就会消失，父类又不重载另一个无形参的构造器，那么子类构造器不使用this或super关键字就会出错，因为子类构造器不使用this和super关键字，默认调用父类的无形参构造器，而这个构造器不存在，就会出错，解决办法：1）父类声明一个无形参的构造器2）调用父类另一个参数不为空的构造器
4.  所以建议：创建类时，都创建一个无形参的构造器
5.  当有类实例化对象时，Object类的无形参构造器一定会被调用。

