---
title: This引用逃逸详解
categories:
    - [计算机学科,java,知识点,对象]
tags:
    - 对象
    - 知识点
---

# 一、This引用逃逸

## 1 什么是This逃逸

在构造器还未彻底完成前(即实例初始化阶段还未完成)，将自身this引用向外抛出并被其它线程复制(访问) 了该引用，可能会访问到还未被初始化的变量，甚至可能会造成更大严重的问题。

代码

```java
/**
  * 模拟this逃逸
  * @author Lijian
  *
  */
public class ThisEscape {
   //final常量会保证在构造器内完成初始化（但是仅限于未发生this逃逸的情况下，具体可以看多线程对final保证可见性的实现）
   final int i;
   //尽管实例变量有初始值，但是还实例化完成
   int j = 0;
   static ThisEscape obj;
   public ThisEscape() {
      i=1;
      j=1;
      //将this逃逸抛出给线程B
      obj = this;
   }
   public static void main(String[] args) {
      //线程A：模拟构造器中this逃逸,将未构造完全对象引用抛出
      /*Thread threadA = new Thread(new Runnable() {
             @Override
             public void run() {
                 //obj = new ThisEscape();
             }
         });*/
      //线程B：读取对象引用，访问i/j变量
      Thread threadB = new Thread(new Runnable() {
         @Override
         public void run() {
            31                 //可能会发生初始化失败的情况解释：实例变量i的初始化被重排序到构造器外，此时1还未被初始化
               ThisEscape objB = obj;
            try {
               System.out.println(objB.j);
            } catch (NullPointerException e) {
               System.out.println("发生空指针错误：普通变量j未被初始化");
            }
            try {
               System.out.println(objB.i);
            } catch (NullPointerException e) {
               System.out.println("发生空指针错误：final变量i未被初始化");
            }
         }
      });
      //threadA.start();
      threadB.start();
   }
}
```

输出结果：

```
发生空指针错误：普通变量j未被初始化
发生空指针错误：final变量i未被初始化
```

这说明ThisEscape还未完成实例化，构造还未彻底结束。

另一种情况是利用线程A模拟this逃逸，但不一定会发生，线程A模拟构造器正在构造... 而线程B尝试访问变量，这是因为

1.  由于JVM的指令重排序存在，实例变量i的初始化被安排到构造器外(final可见性保证是final变量规定在构造器中完成的)
2.  类似于this逃逸，线程A中构造器还未完全完成

所以尝试多次输出(相信我一定会发生的，只是概率相对低)，也会发生类似this引用逃逸的情况

```java
/**
  * 模拟this逃逸
  * @author Lijian
  *
  */
public class ThisEscape {
   //final常量会保证在构造器内完成初始化（但是仅限于未发送this逃逸的情况下）
   final int i;
   //尽管实例变量有初始值，但是还实例化完成
   int j = 0;
   static ThisEscape obj;
   public ThisEscape() {
      i=1;
      j=1;
      //obj = this ;
   }
   public static void main(String[] args) {
      //线程A：模拟构造器中this逃逸,将未构造完全对象引用抛出
      Thread threadA = new Thread(new Runnable() {
         @Override
         public void run() {
            //构造初始化中...线程B可能获取到还未被初始化完成的变量
            //类似于this逃逸，但并不定发生
            obj = new ThisEscape();
         }
      });
      //线程B：读取对象引用，访问i/j变量
      Thread threadB = new Thread(new Runnable() {
         @Override
         public void run() {
            //可能会发生初始化失败的情况解释：实例变量i的初始化被重排序到构造器外，此时1还未被初始化
            ThisEscape objB = obj;
            try {
               System.out.println(objB.j);
            } catch (NullPointerException e) {
               System.out.println("发生空指针错误：普通变量j未被初始化");
            }
            try {
               System.out.println(objB.i);
            } catch (NullPointerException e) {
               System.out.println("发生空指针错误：final变量i未被初始化");
            }
         }
      });
      threadA.start();
      threadB.start();
   }
}
```

## 1.2 什么情况下会This逃逸？

1.  在构造器中很明显的抛出this引用提供其它线程使用(如上述的明显将this抛出)
2.  在构造器中内部类使用外部情况：内部类访问外部类是没有任何条件的，也不要任何代价，也就造成了当外部类还未初始化完成的时候，内部类就尝试获取未初始化完成的变量
    -  在构造器中启动线程：启动的线程任务是内部类，在内部类中xxx.this访问了外部类实例，就会发生访问到还未初始化完成的变量
    -  在构造器中注册事件，这是因为在构造器中监听事件是有回调函数(可能访问了操作实例变量)，而事件监听一般都是异步的。在还未初始化完成之前就可能发生会回调访问了未初始化的变量。

在构造器中启动线程代码实现：

```java
/**
  * 模拟this逃逸2：构造器中启动线程
  * @author Lijian
  *
  */
public class ThisEscape2 {
   final int i;
   int j;
   public ThisEscape2() {
      i = 1;
      j = 1;
      new Thread(new RunablTest()).start();
   }
   //内部类实现Runnable：引用外部类
   private class RunablTest implements Runnable{
      @Override
      public void run() {
         try {
            System.out.println(ThisEscape2.this.j);
         } catch (NullPointerException e) {
            System.out.println("发生空指针错误：普通变量j未被初始化");
         }
         try {
            System.out.println(ThisEscape2.this.i);
         } catch (NullPointerException e) {
            System.out.println("发生空指针错误：final变量i未被初始化");
         }
      }

   }
   public static void main(String[] args) {
      new ThisEscape2();
   }
}
```

构造器中注册事件，引用网上的一段伪代码将以解释：

```java
public class ThisEscape3 {
    private final int var;
 
    public ThisEscape3(EventSource source) {
　　　　 //注册事件，会一直监听，当发生事件e时，会执行回调函数doSomething
        source.registerListener(
　　　　　　　//匿名内部类实现
            new EventListener() {
                public void onEvent(Event e) {
　　　　　　　　　　　 //此时ThisEscape3可能还未初始化完成，var可能还未被赋值，自然就发生严重错误
                    doSomething(e);
                }
            }
        );
        var = 10;
    }
    // 在回调函数中访问变量
    int doSomething(Event e) {
        return var;
    }
}
```

## 1.3 怎样避免This逃逸？

1 单独编写一个启动线程的方法，不要在构造器中启动线程，尝试在外部启动。

```java
private Thread t;
public ThisEscape2() {
    t = new Thread(new EscapeRunnable());
}

public void initStart() {
    t.start();
}
```

2 将事件监听放置于构造器外，比如new Object()的时候就启动事件监听，但是在构造器内不能使用事件监听，那可以在static{}中加事件监听，这样就跟构造器解耦了

```java
static{
    source.registerListener(
            new EventListener() {
                public void onEvent(Event e) {
                    doSomething(e);
                }
            }
        );
        var = 10;
    }
}
```

## 1.4 总结

　　this引用逃逸问题实则是Java多线程编程中需要注意的问题，引起逃逸的原因无非就是在多线程的编程中“滥用”引用（往往涉及构造器中显式或隐式地滥用this引用），在使用到this引用的时候需要特别注意！

　　同时这会涉及到：final的内存语义，即final域禁止重排序问题（2020.11.22增加），包括写final域与读final域重排序两个规则（参考资料《Java并发编程的艺术》）