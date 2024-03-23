---
title: join()方法的详细分析
categories:
    - [计算机学科,java,知识点,多线程]
tags:
    - 知识点
    - 多线程
---

# join()方法的详细分析

本文章来探讨 系统中运行多个线程时，join() 到底是暂停了那些线程

**结论**：A.join()方法只会使主线程(或者或调用A.join()的线程)进入等待池并等待A线程执行完毕之后才会被唤醒。并不影响同一时刻处在运行状态的其它线程

代码

```java
public class Demo01 {
    public static void main(String[] args) {
        DemoT A = new DemoT("A");
        DemoT B = new DemoT("B");
        A.start();
        B.start();
    }
}
class DemoT extends Thread {
    private String name;
    public DemoT (String name) {
        this.name = name;
    }

    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(name + " - " + i);
        }
    }
}
```

打印结果

```
B - 1
A - 1
B - 2
A - 2
B - 3
A - 3
B - 4
A - 4
B - 5
A - 5
```

可以看出A线程和B线程是交替执行的。

而在其中加入join()方法后，代码如下

```java
public class Demo01 {
    public static void main(String[] args) {
        DemoT A = new DemoT("A");
        DemoT B = new DemoT("B");
        A.start();
        try {
            A.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        B.start();
    }
}
class DemoT extends Thread {
    private String name;
    public DemoT (String name) {
        this.name = name;
    }

    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(name + " - " + i);
        }
    }
}
```

打印结果

```
A - 1
A - 2
A - 3
A - 4
A - 5
B - 1
B - 2
B - 3
B - 4
B - 5
```

显然，使用A.join()之后，B线程需要等待A线程执行完毕之后才能执行。需要注意的是，A.join()需要等A.start()执行之后才有效果，此外，如果A.join()放在B.start()之后的话，扔然会是交替执行，然而并没有效果，这点就很困惑了

代码

```java
public class Demo01 {
    public static void main(String[] args) {
        DemoT A = new DemoT("A");
        DemoT B = new DemoT("B");
        A.start();
        B.start();
        try {
            A.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
class DemoT extends Thread {
    private String name;
    public DemoT (String name) {
        this.name = name;
    }

    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(name + " - " + i);
        }
    }
}
```

打印结果

```
A - 1
B - 1
A - 2
B - 2
A - 3
B - 3
A - 4
B - 4
A - 5
B - 5
```

为了深入理解，我们看下join()的源码

```java
/**
     * Waits for this thread to die.
     *
     * <p> An invocation of this method behaves in exactly the same
     * way as the invocation
     *
     * <blockquote>
     * {@linkplain #join(long) join}{@code (0)}
     * </blockquote>
     *
     * @throws  InterruptedException
     *          if any thread has interrupted the current thread. The
     *          <i>interrupted status</i> of the current thread is
     *          cleared when this exception is thrown.
     */
public final void join() throws InterruptedException {
   join(0);            //join()等同于join(0)
}
/**
     * Waits at most {@code millis} milliseconds for this thread to
     * die. A timeout of {@code 0} means to wait forever.
     *
     * <p> This implementation uses a loop of {@code this.wait} calls
     * conditioned on {@code this.isAlive}. As a thread terminates the
     * {@code this.notifyAll} method is invoked. It is recommended that
     * applications not use {@code wait}, {@code notify}, or
     * {@code notifyAll} on {@code Thread} instances.
     *
     * @param  millis
     *         the time to wait in milliseconds
     *
     * @throws  IllegalArgumentException
     *          if the value of {@code millis} is negative
     *
     * @throws  InterruptedException
     *          if any thread has interrupted the current thread. The
     *          <i>interrupted status</i> of the current thread is
     *          cleared when this exception is thrown.
     */
public final synchronized void join(long millis) throws InterruptedException {
   long base = System.currentTimeMillis();
   long now = 0;

   if (millis < 0) {
      throw new IllegalArgumentException("timeout value is negative");
   }

   if (millis == 0) {
      while (isAlive()) {
         wait(0);           //join(0)等同于wait(0)，即wait无限时间直到被notify
      }
   } else {
      while (isAlive()) {
         long delay = millis - now;
         if (delay <= 0) {
            break;
         }
         wait(delay);
         now = System.currentTimeMillis() - base;
      }
   }
}
```

可以看出，join()方法的底层是利用wait()方法实现的。可以看出join()方法是一个同步方法，当主线程调用A.join()方法时，主线程先获得了A对象的锁，随后进入方法，调用了A对象的wait()方法，使主线程进入了A对象的等待池，此时，A线程则还在执行，并且随后B.start()还没被执行，因此，B线程也还没开始。等待A线程执行完毕之后，主线程继续执行，走到了B.strt()，B线程才会开始执行。

此外，对于join()的位置和作用关系，我们可以用下面的例子来分析：

```java
public class Demo01 {
    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName() + " start");
        DemoT A = new DemoT("A");
        DemoT B = new DemoT("B");
        DemoT C = new DemoT("C");
        System.out.println("A start");
        A.start();
        System.out.println("B start");
        B.start();
        System.out.println("C start");
        C.start();
        System.out.println(Thread.currentThread().getName() + " end");
    }
}
class DemoT extends Thread {
    private String name;
    public DemoT (String name) {
        this.name = name;
    }

    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(name + " - " + i);
        }
    }
}
```

打印结果

```
main start
A start
B start
C start
A - 1
main end
B - 1
C - 1
A - 2
C - 2
B - 2
C - 3
A - 3
C - 4
B - 3
C - 5
A - 4
A - 5
B - 4
B - 5
```

此时A，B，C和主线程交替运行，加入join()方法后

代码

```java
public class Demo01 {
    public static void main(String[] args) {
        System.out.println(Thread.currentThread().getName() + " start");
        DemoT A = new DemoT("A");
        DemoT B = new DemoT("B");
        DemoT C = new DemoT("C");
        System.out.println("A start");
        A.start();
        System.out.println("B start");
        B.start();
        try {
            A.join();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("C start");
        C.start();
        System.out.println(Thread.currentThread().getName() + " end");
    }
}
class DemoT extends Thread {
    private String name;
    public DemoT (String name) {
        this.name = name;
    }

    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(name + " - " + i);
        }
    }
}
```

打印结果

```
main start
A start
B start
A - 1
B - 1
A - 2
B - 2
A - 3
B - 3
A - 4
B - 4
A - 5
B - 5
C start
main end
C - 1
C - 2
C - 3
C - 4
C - 5
```

多次试验可以看出，主线程在A.join()方法处停止，并需要等待A线程执行完毕后才会执行C.start()，然而，并不影响B线程的执行。因此，可以得出结论，A.join()方法只会使主线程进入等待池并等待A线程执行完毕后才会被唤醒。并不影响同一时刻处在运行状态的其它线程。

PS: join源码中，只会调用wait方法，并没有在结束时调用notify，这是因为线程在消亡(结束)的时候会自动调用自身的notifyAll方法，来释放所有的资源和锁。

