---
title: NIO深入剖析
categories:
    - [计算机学科,java,知识点,IO]
tags:
    - IO
    - 知识点
---

# 一、NIO深入剖析

在讲解利用NIO实现通信架构之前，我们需要先来了解一下NIO的基本特点和使用

## 1.1 Java NIO基本介绍

-  Java NIO(New IO)也有人称之为java non-blocking IO是从java 1.4版本开始引入的一个新的IO API，可以代替标准的Java IO API。NIO与原来的IO有同样的作用和目的，但是使用的方式完全不同，NIO支持面向缓冲区的，基于通道的IO操作。NIO将以更加高效的方式进行文件的读写操作。NIO可以理解为非阻塞IO。传统的IO的read和write只能阻塞执行，线程在读写IO期间不能干其他事情，比如调用socket.read()时，如果服务器一直没有数据传输过来，线程就一直阻塞，而NIO中可以配置socket为非阻塞模式。
-  NIO相关类都被放在java.nio包及子包下，并且对原java.io包中的很多类进行改写
-  NIO有三大核心部分：Channel(通道)，Buffer(缓冲区)，Selector(选择器)
-  Java NIO的非阻塞模式，使一个线程从某通道发送请求或者读取数据，但是它仅能得到目前可用的数据，如果目前灭有数据可用时，就什么都不会获取，而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他事情。非阻塞写也是如此，一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。
-  通俗理解：NIO是可以做到用一个线程来处理多个操作的。假设有1000个请求过来，根据实际情况，可以分配20或者80个线程来处理。不像之前的阻塞IO那样，非得分配1000个。

## 1.2 NIO 和BIO 比较

-  BIO以流的方式处理数据，而NIO以块的方式处理数据，块IO的效率比 IO高很多
-  BIO是阻塞的，NIO则是非阻塞的
-  BIO基于字节流和字符流进行操作，而NIO基于Channel(通道)和Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。Selector(选择器)用于监听多个通道的事件(比如：连接请求，数据到达等)，因此使用单个线程可以监听多个客户端通道

| NIO                     | BIO                  |
| ----------------------- | -------------------- |
| 面向缓冲区(Buffer)      | 面向流(Stream)       |
| 非阻塞(Non Blocking IO) | 阻塞IO (Blocking IO) |
| 选择器(Selectors)       |                      |

先来看下BIO模式的图解：

![image-20240305215849007](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240305215849007.png)

再来看NIO模式的图解：

![image-20240305221811257](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240305221811257.png)

## 1.3 NIO三大核心原理示意图

NIO有三大核心部分：Channel(通道)，Buffer(缓冲区)，Selector(选择器)

### 1.3.1 Buffer缓冲区

缓冲区本质上是一块可以写入数据，然后可以从中读取数据的内存。这块内存被包装成NIO Buffer对象，并提供了一组方法，用来方便的访问该块内存。相比较直接对数组的操作，Buffer API更加容易操作和管理。

### 1.3.2 Channel(通道)

Java NIO的通道类似流，但又有些不同：即可以从通道中读取数据，又可以写数据到通道。但流的(input或output)读写通常是单向的。通道可以非阻塞读取和写入通道，通道可以支持读取或写入缓冲区，也支持异步读写。

### 1.3.3 Selector选择器

Selector是一个Java NIO组件，可以能够检查一个或多个NIO通道，并确定拿些通道已经准备好进行读取或写入。这样，一个单独的线程可以管理多个Channel，从而管理多个网络连接，提高效率。

![image-20240305222516043](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240305222516043.png)

-  每个Channel都会对应一个Buffer
-  一个线程对应一个Selector，一个Selector对应多个Channel(连接)
-  程序切换到哪个Channel是由事件决定的
-  Selector会根据不同的事件，在各个通道上切换
-  Buffer就是一个内存块，底层是一个数组
-  数据的读取写入是通过Buffer完成的，BIO中要么是输入流，或者是输出流，不能双向，但是NIO的Buffer是可以读也可以写
-  Java NIO系统的核心在于：通道(Channel)和缓冲区(Buffer)。通道表示打开到IO设备(例如：文件，套接字)的连接。若需要使用NIO系统，需要获取用于连接IO设备的通道以及用于容纳数据的缓冲区。然后操作缓冲区，对数据进行处理。简而言之，Channel负责传输，Buffer负责存取数据。

## 1.4 NIO核心一：缓冲区(Buffer)

### 1.4.1 缓冲区(Buffer)

一个用于特定基本数据类型的容器，由java.nio包定义的，所有缓冲区都是Buffer抽象类的子类。Java NIO中的Buffer主要用于与NIO通道进行交互，数据是从通道读入缓冲区的，从缓冲区写入通道中的

![image-20240305223759612](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240305223759612.png)

### 1.4.2 Buffer类及其子类

Buffer就像一个数组，可以保存多个相同类型的数据。根据数据类型不同，有以下Buffer常用子类：

-  ByteBuffer
-  CharBuffer
-  ShortBuffer
-  IntBuffer
-  LongBuffer
-  FloatBuffer
-  DoubleBuffer

上述Buffer类它们都采用相似的方法进行管理数据，只是各自管理的数据类型不同而已。都是通过如下方法获取一个Buffer对象：

```java
static XxxBuffer allocate(int capacity)：创建一个容器为capacity的XxxBuffer对象
```

### 1.4.3 缓冲区的基本属性

Buffer中的重要概念：

-  容量(capacity)：作为一个内存块，Buffer具有一定的固定大小，也称为 "容量"，缓冲区容量不能为负，并且创建后不能更改。
-  限制(limit)：表示缓冲区中可以操作数据的大小(limit后数据不能进行读写)。缓冲区的限制不能为负，并且不能大于其容量。写入模式，限制等于Buffer的容量。读取模式下，limit等于写入的数据量。
-  位置(position)：下一个要读取或写入的数据的索引。缓冲区的位置不能为负，并且不能大于其限制。
-  标记(mark)与重置(reset)：标记是一个索引，通过Buffer中的mark()方法 指定Buffer中一个特定的position，之后可以通过调用reset()方法恢复到这个postion。标记，位置，限制，容量遵守以下不变式：0 <= mark <= positoin <= limit <= capacity
-  图示：
-  ![image-20240305225000497](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240305225000497.png)

### 1.4.4 Buffer常见方法API

| 方法                   | 作用                                                 |
| ---------------------- | ---------------------------------------------------- |
| Buffer clear()         | 清空缓冲区并返回对象缓冲区的引用                     |
| Buffer flip()          | 将缓冲区的界限设置为当前位置，并将当前位置重置为 0   |
| int capacity()         | 返回Buffer的capacity大小                             |
| boolean hasRemaining() | 判断缓冲区中是否还有元素                             |
| int limit()            | 返回Buffer的界限(limit)的位置                        |
| Buffer limit(int n)    | 设置缓冲区界限为n，并返回一个具有新limit的缓冲区对象 |
| Buffer mark()          | 对缓冲区设置标记                                     |
| int position()         | 返回缓冲区的当前位置position                         |
| Buffer position(int n) | 设置缓冲区的当前位置为n，并返回修改后的Buffer对象    |
| int remaining()        | 返回position和limit之间的元素个数                    |
| Buffer reset()         | 将位置position转到以前设置的mark所在的位置           |
| Buffer rewind()        | 将位置设为0，取消设置的mark                          |

### 1.4.5 缓冲区的数据操作

Buffer 所有子类提供了两个用于数据操作的方法：get()， put()方法

| 方法            | 作用                                     |
| --------------- | ---------------------------------------- |
| get()           | 读取单个字节                             |
| get(byte[] dst) | 批量读取多个字节到dst中                  |
| get(int index)  | 读取指定索引位置的字节(不会移动position) |

放入数据到Buffer中

| 方法                   | 作用                                             |
| ---------------------- | ------------------------------------------------ |
| put(byte b)            | 将给定单个字节写入缓冲区的当前位置               |
| put(byte[] src)        | 将src中的字节写入缓冲区的当前位置                |
| put(int index, byte b) | 将指定字节写入缓冲区的索引位置(不会移动position) |

**使用Buffer读写数据一般遵循以下四个步骤**：

1.  写入数据到Buffer
2.  调用flip()方法，转换为读取模式
3.  从Buffer中读取数据
4.  调用buffer.clear()方法或者buffer.compact()方法清除缓冲区

代码演示：

```java
public static void main(String[] args) {
   // 分配一个缓冲区，容量设置成10
   ByteBuffer buffer = ByteBuffer.allocate(10);
   System.out.println(buffer.position()); // 0
   System.out.println(buffer.limit()); // 10
   System.out.println(buffer.capacity()); // 10
   System.out.println("------------------------");
   String name = "itheima";
   // 向ByteBuffer中放入数据
   buffer.put(name.getBytes());
   // position特性：记录下一个要读取或写入的索引，缓冲区的位置不能为负，并且不能大于其限制
   // position作用：返回缓冲区的当前位置
   System.out.println(buffer.position()); // 7
   System.out.println(buffer.limit()); // 10
   System.out.println(buffer.capacity()); // 10
   System.out.println("------------------------");
   // flip作用：切换为读取模式
   // flip特新：将缓冲区的界限设置为当前位置，并将当前位置重置为0
   // 从position位置开始 读取到 limit位置
   buffer.flip();
   System.out.println(buffer.position()); // 0
   System.out.println(buffer.limit()); // 7
   System.out.println(buffer.capacity()); // 10
   System.out.println("------------------------");
   char ch = (char)buffer.get();
   System.out.println("读取的字符：" + ch);
   // 读取一个字符后position指向下一个字符就变成了1
   System.out.println(buffer.position()); // 1
   System.out.println(buffer.limit()); // 7
   System.out.println(buffer.capacity()); // 10
}
```

```java
public static void main(String[] args) {
   // 分配一个缓冲区，容量设置成10
   ByteBuffer buffer = ByteBuffer.allocate(10);
   System.out.println(buffer.position()); // 0
   System.out.println(buffer.limit()); // 10
   System.out.println(buffer.capacity()); // 10
   System.out.println("------------------------");
   String name = "itheima";
   // 向ByteBuffer中放入数据
   buffer.put(name.getBytes());
   // position特性：记录下一个读取或写入的索引，缓冲区的位置不能为负，并且不能大于其限制
   // position作用：返回缓冲区的当前位置
   System.out.println(buffer.position()); // 7
   System.out.println(buffer.limit()); // 10
   System.out.println(buffer.capacity()); // 10
   System.out.println("------------------------");
   // 清空缓冲区并返回对象缓冲区的引用
   buffer.clear();
   System.out.println(buffer.position()); // 0
   System.out.println(buffer.limit()); // 10
   System.out.println(buffer.capacity()); // 10
   System.out.println("------------------------");
   // 调用了clear为什么还能获取到上面存储的itheima的i呢？ 这是因为clear并没有真正的清除数据，它只是让position的位置恢复到了0，但是后续的数据还是在的
   System.out.println("从缓冲区中获取position位置的字符：" + (char)buffer.get()); // i
}
```

```java
// 分配一个缓冲区容量为10
ByteBuffer buf = ByteBuffer.allocate(10);
// 切换可读模式position置为0
buf.flip(); // 报错：.BufferUnderflowException 意思是我们必须保证缓冲区中有数据才能切换可读模式进行读写数据
// 创建缓冲区 大小为2
byte[] bytes = new byte[2];
// 从缓冲区中获取大小为2字节的数据
buf.get(bytes);
// 将字节数据转换为十进制
String s = new String(bytes);
// 打印
System.out.println(s);
```

```java
public static void main(String[] args) {
   ByteBuffer buffer = ByteBuffer.allocate(10);
   String n = "itheima";
   buffer.put(n.getBytes());
   buffer.flip();
   byte[] bytes = new byte[2];
   buffer.get(bytes);
   String rs = new String(bytes);
   System.out.println(rs);
   System.out.println("----------------");
   System.out.println(buffer.position()); // 2 取出了两个字符所以position的索引位置在2
   System.out.println(buffer.limit()); // 7
   System.out.println(buffer.capacity()); // 10
   // 标记一处索引
   buffer.mark();
   byte[] bytes1 = new byte[3];
   System.out.println("----------------");
   buffer.get(bytes1);
   System.out.println(new String(bytes1));
   System.out.println(buffer.position()); // 5 又取出了3个字符所以position的索引位置在5
   System.out.println(buffer.limit()); // 7
   System.out.println(buffer.capacity()); // 10
   // 回到上一次标记处
   buffer.reset();
   // 判断是否还有剩余的数据
   if (buffer.hasRemaining()) {
      // 打印输出 剩余的数据个数
      System.out.println(buffer.remaining()); // 5
   }
}
```

### 1.4.6 直接与非直接缓冲区

什么是直接内存与非直接内存

**根据官方文档的描述**：

`bytebuffer `可以是两种类型，一种是==基于直接内存==(也就是非堆内存)；另一种是==非直接内存==(也就是堆内存)。对于直接内存来说，==JVM将会在IO操作上具有更高的性能，因为它直接用于本地系统的IO操作==。而非直接内存，也就是堆内存中的数据，如果要操作IO操作，==会先从本进程内存复制到直接内存，再利用本地IO处理==。

从数据流的角度，非直接内存是下面这样的作用链：

`本地IO ——> 直接内存 ——> 非直接内存 ——> 直接内存 ——> 本地IO`

而直接内存是：

`本地IO ——> 直接内存 ——> 本地IO`

很明显，在做IO处理时，比如网络发送量大数据时，直接内存会具有更高的效率。直接内存使用allocateDirect创建，但是它比申请普通的堆内存需要耗费更高的性能。不过，这部分的数据是在JVM之外的，因此它不会占用应用的内存。所以呢，当你有很大的数据要缓存，并且它的生命周期又很长，那么就比较适合使用直接内存。只是一般来说，如果不是能带来很明显的性能提升，还是推荐直接使用堆内存。字节缓冲区是字节缓冲区还是非直接缓冲区可通过调用其isDirect()方法来确定。

**使用场景**

-  有很大的数据需要存储，它的生命周期又很长
-  适合频繁的IO操作，比如网络并发场景

### 1.4.7 NIO核心二：通道(Channel)

#### 1.4.7.1 通道Channel概述

通道(Channel)：由java.nio.channels包定义的。Channel表示IO源与目标打开的连接。Channel类似于传统的 "流"。只不过Channel本身不能直接访问数据，Channel只能与Buffer进行交互

1、NIO的通道类似于流，但有些区别如下：

-  通道可以同时进行读写，而流只能读或者只能写
-  通道可以实现异步读写数据
-  通道可以从缓冲读数据，也可以写数据到缓冲区

2、BIO中的stream是单向的，例如FileInputStream对象只能进行读取数据的操作，而NIO中的通道(Channel)是双向的，可以读操作，也可以写操作。

3、Channel在NIO中是一个接口

```java
public interface Channel extends Clonseable{}
```

#### 1.4.7.2 常用的Channel实现类

-  FileChannel：用于读取，写入，映射和操作文件的通道。
-  DatagramChannel：通过UDP读写网络中的数据通道。
-  SocketChannel：通过TCP读写网络中的数据。
-  ServerSOcketChanbnel：可以监听新进来的TCP连接，对每一个新进来的连接都会创建一个SocketChannel。[ServerSocketChannel类似ServerSocket，SocketChannel类似Socket]

#### 1.4.7.3 FileChannel类

获取通道的一种方式是对支持通道的对象调用getChannel()方法。支持通过的类如下：

-  FileInputStream

-  FileOutputStream

-  RandomAccessFile

-  DatagramSocket

-  Socket

-  ServerSocket

   获取通道的其它方式是使用Files类的静态方法newByteChannel()获取字节通道。或者通过通道的静态方法open()打开并返回指定通道

#### 1.4.7.4 FileChannel的常用方法

| 方法                          | 作用                                         |
| ----------------------------- | -------------------------------------------- |
| int read(ByteBuffer dst)      | 从Channel中读取数据到ByteBuffer              |
| long read(ByteBuffer[] dsts)  | 将Channel中的数据 "分散" 到ByteBuffer[]      |
| long write(ByteBuffer src)    | 将ByteBuffer中的数据写入到Channel            |
| long write(ByteBuffer[] srcs) | 将ByteBuffer[]中的数据 "聚集" 到Channel      |
| long position()               | 返回此通道的文件位置                         |
| FileChannel position(long p)  | 设置此通道的文件位置                         |
| long size()                   | 返回此通道的文件的当前大小                   |
| FileChannel truncate(long s)  | 将此通道的文件截取为给定大小                 |
| void force(boolean metaData)  | 强制将所有对此通道的文件更新写入到存储设备中 |

#### 1.4.7.5 案例-本地文件写数据

需求：使用前面学习后的ByteBuffer(缓冲区) 和 FileChannel(通道)，将 "hello" 写入到ar.txt

```java
public class ChannelTest {
    @Test
    public void test() {
        FileOutputStream os = null;
        FileChannel channel = null;
        try {
            // 得到一个字节输出流通向目标文件
            os = new FileOutputStream(System.getProperty("user.dir") + "\\ar.txt");
            // 得到字节输出流对应的通道
            channel = os.getChannel();
            // 创建缓冲区
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            // 向缓冲区中添加字节数据
            buffer.put("hello".getBytes());
            // 切换到可读模式
            buffer.flip();
            channel.write(buffer);
        } catch (FileNotFoundException e) {
            throw new RuntimeException(e);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (channel != null) {
                    channel.close();
                }
                if (os != null) {
                    os.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

执行结果：

![image-20240308221531463](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240308221531463.png)

#### 1.4.7.6 案例-本地文件读数据

需求：使用前面学习后的ByteBuffer(缓冲区)和FileChannel(通道)，将ar.txt中的数据读入到程序，并显示在控制台屏幕

```java
@Test
public void test1() {
   // 获取当前工程的绝对路径
   String filePath = new File("").getAbsolutePath();
   try {
      // 创建输入流读取ar.txt文件内容
      FileInputStream is = new FileInputStream(filePath + "\\ar.txt");
      // 获取通道对象
      FileChannel channel = is.getChannel();
      // 创建缓冲区
      ByteBuffer buffer = ByteBuffer.allocate(1024);
      // 将通道中数据读取到缓冲区
      channel.read(buffer);
      // 切换读模式position置为0
      buffer.flip();
      // 打印内容buffer.array将缓冲区的数据转化为数组从0开始读取到 缓冲区剩余数据为止
      System.out.println(new String(buffer.array(), 0, buffer.remaining()));
   } catch (FileNotFoundException e) {
      throw new RuntimeException(e);
   } catch (IOException e) {
      throw new RuntimeException(e);
   }
}
```

打印结果：

```
hello
```

#### 1.4.7.7 案例-使用Buffer完成文件复制

使用FileChannel(通道)，完成文件的拷贝

```java
@Test
public void copy() {
   FileInputStream is = null;
   FileOutputStream os = null;
   FileChannel isChannel = null;
   FileChannel osChannel = null;
   try {
      // 创建字节输入流读取指定文件
      is = new FileInputStream(new File("").getAbsolutePath() + "\\ar.txt");
      // 创建字节输出流 指定写出地址
      os = new FileOutputStream(new File("").getAbsolutePath() + "\\arTwo.txt");
      // 获取 读数据通道
      isChannel = is.getChannel();
      // 获取 写数据通道
      osChannel = os.getChannel();
      // 创建1024的缓冲区
      ByteBuffer buffer = ByteBuffer.allocate(1024);
      while (true) {
         try {
            // 上一次读取的数据已经写出完了 清空上次的数据继续下次的数据
            buffer.clear();
            // 将读取的缓冲区个数据的值赋值给flag
            int flag = isChannel.read(buffer);
            // 如果flag = -1说明没有数据此时就 跳出执行
            if (flag == -1) {
               break;
            }
            // 切换可读模式
            buffer.flip();
            // 将数据写出
            osChannel.write(buffer);
         } catch (IOException e) {
            throw new RuntimeException(e);
         }
      }
      System.out.println("复制完成！");
   } catch (FileNotFoundException e) {
      throw new RuntimeException(e);
   } finally {
      // 释放资源
      try {
         if (isChannel != null) {
            isChannel.close();
         }
         if (osChannel != null) {
            osChannel.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

执行结果：

![image-20240309171606609](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240309171606609.png)

#### 1.4.7.8 案例-分散(Scatter)和聚集(Gather)

分散读取(Scatter)：是指把Channel通道的数据读入到多个缓冲区中去

聚集写入(Gathering)：是指将多个Buffer中的数据 "聚集" 到Channel。

```java
@Test
public void test2() {
   FileChannel isChannel = null;
   FileChannel osChannel = null;
   try {
      // 创建 读写 流 并获取对应的通道
      FileInputStream is = new FileInputStream(new File("").getAbsolutePath() + "\\ar.txt");
      isChannel = is.getChannel();
      FileOutputStream os = new FileOutputStream(new File("").getAbsolutePath() + "\\arThree.txt");
      osChannel = os.getChannel();
      // 创建容量为5字节的缓冲区
      ByteBuffer bufOne = ByteBuffer.allocate(5);
      // 创建容量为1KB的缓冲区
      ByteBuffer bufTwo = ByteBuffer.allocate(1024);
      // 将两个缓冲区放入到一个bufs的数组中
      ByteBuffer[] bufs = {bufOne, bufTwo};
      // 读取数据到通道
      isChannel.read(bufs);
      // 遍历bufs
      for (ByteBuffer buf : bufs) {
         // 切换可读模式
         buf.flip();
         // 打印输出读取到的数据
         System.out.println(new String(buf.array(), 0, buf.remaining()));
      }
   } catch (FileNotFoundException e) {
      throw new RuntimeException(e);
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      // 释放资源
      try {
         if (isChannel != null) {
            isChannel.close();
         }
         if (osChannel != null) {
            osChannel.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

打印结果：

```
hello
World
```

#### 1.4.7.9 案例-transferFrom()

从目标通道中去复制原通道数据

```java
@Test
public void test3() {
   FileChannel isChannel = null;
   FileChannel osChannel = null;
   try {
      // 创建 读写流并获取对应通道
      FileInputStream is = new FileInputStream(new File("").getAbsolutePath() + "\\ar.txt");
      FileOutputStream os = new FileOutputStream(new File("").getAbsolutePath() + "\\arThree.txt");
      isChannel = is.getChannel();
      osChannel = os.getChannel();
      // 将读数据的通道的数据复制到写通道中
      osChannel.transferFrom(isChannel, isChannel.position(), isChannel.size());
   } catch (FileNotFoundException e) {
      throw new RuntimeException(e);
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      // 释放资源
      try {
         if (isChannel != null) {
            isChannel.close();
         }
         if (osChannel != null) {
            osChannel.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

![image-20240310005045706](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240310005045706.png)

#### 1.4.7.10 案例-transferTo()

把原通道数据复制到目标通道

```java
@Test
public void test3() {
   FileChannel isChannel = null;
   FileChannel osChannel = null;
   try {
      // 创建 读写流并获取对应通道
      FileInputStream is = new FileInputStream(new File("").getAbsolutePath() + "\\ar.txt");
      FileOutputStream os = new FileOutputStream(new File("").getAbsolutePath() + "\\arThree.txt");
      isChannel = is.getChannel();
      osChannel = os.getChannel();
      // 将读数据的通道的数据复制到写通道中
      isChannel.transferTo(isChannel.position(), isChannel.size(), osChannel);
   } catch (FileNotFoundException e) {
      throw new RuntimeException(e);
   } catch (IOException e) {
      throw new RuntimeException(e);
   } finally {
      // 释放资源
      try {
         if (isChannel != null) {
            isChannel.close();
         }
         if (osChannel != null) {
            osChannel.close();
         }
      } catch (IOException e) {
         throw new RuntimeException(e);
      }
   }
}
```

这两个复制通道数据的方法 作用都是一样的 只是谁给谁数据的位置不同而已。

### 1.5 NIO核心三：选择器(Selector)

#### 1.5.1 选择器(Selector)概述

选择器(Selector)是SelectableChannel对象的多路复用器，Selector可以同时监控多个SelectableChannel的IO状况，也就是说，利用Selector可使一个单独的线程管理多个Channel。Selector是非阻塞IO的核心。

![image-20240310005558845](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240310005558845.png)

-  Java的NIO，用非阻塞IO方式。可以用一个线程，处理多个的客户端连接，就会使用到Selector(选择器)
-  Selector能够检测多个注册的通道上是否有事件发生(注意：多个Channel以事件的方式可以注册到同一个Selector)，如果有事件发生，便获取事件然后针对每个事件进行相应的处理。这样就可以只用一个单线程去管理多个通道，也就是管理多个连接和请求。
-  只有在 连接/通道 真正有读写事件发生时，才会进行读写，就大大的减少了系统开销，并且不必为每个连接都创建一个线程，不用去维护多个线程
-  避免了多线程之间的上下文切换导致的开销

#### 1.5.2 选择器(Selector)的应用

创建Selector：通过调用Selector.open()方法创建一个Selector。

```java
Selector selector = Selector.open();
```

向选择器注册通道：SelectableChannel.register(Selector sel, int ops)

```java
// 1.获取通道
ServerSocketChannel ssChannel = ServerSocketChannel.open();
// 2.切换非阻塞模式
ssChannel.configureBlocking(false);
// 3.绑定连接
ssChannel.bind(new InetSocketAddress(9898));
// 4.获取选择器
Selector selector = Selector.open();
// 5.将通道注册到选择器上，并且指定 "监听接收事件"
ssChannel.register(selector, SelectionKey.OP_ACCEPT);
```

当调用register(Selector sel, int ops)将通道注册选择器时，选择器对通道的监听事件，需要通过第二个参数ops指定。可以监听的事件类型(可使用SelectionKey的四个常量 表示)：

-  读：SelectionKey.OP_READ (1)
-  写：SelectionKey.OP_WRITE (4)
-  连接：SelectionKey.OP_CONNECT (8)
-  接收：SelectionKey.OP_ACCEPT (16)
-  若注册时不止监听一个事件，则可以使用 "位， 或" 操作符连接。

```java
int interrestSet = SelectionKey.OP_READ|SelectionKey.OP_WRITE
```

#### 1.5.3 NIO非阻塞式网络通信原理分析

##### 1.5.3.1 Selector示意图和特点说明

Selector可以实现：一个IO线程可以并发处理N个客户端连接和读写操作，这从根本上解决了传统同步阻塞IO一个链接一个线程模型，架构的性能，弹性伸缩能力和可靠性都得到了极大的提升。

![image-20240310124620521](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240310124620521.png)

##### 1.5.3.2 服务端流程

1.  当客户端连接服务端时，服务端会通过ServerScoketChannel得到SocketChannel：获取通道

    ```java
    ServerSocketChannel ssChannel = ServerSocketChannel.open();
    ```

2.  切换非阻塞模式

    ```java
    ssChannel.configureBlocking(false);
    ```

3.  绑定链接

    ```java
    ssChannel.bind(new InetSocketAddress(9999));
    ```

4.  获取选择器

    ```java
    Selector selector = Selctor.open();
    ```

5.  将通道注册到选择器上，并且指定 "监听接收事件"

    ```java
    ssChannel.register(selector.SelectionKey.OP_ACCEPT)
    ```

6.  轮询式的获取选择器上已经 "准备就绪" 的事件


服务端

```java
/**
 目标：NIO非阻塞通信下的入门案例：服务端开发
 */
public class Server {
    public static void main(String[] args) throws Exception {
        System.out.println("服务端启动");
        // 1、获取通道
        ServerSocketChannel ssChannel = ServerSocketChannel.open();
        // 2、切换为非阻塞模式
        ssChannel.configureBlocking(false);
        // 3、绑定连接的端口
        ssChannel.bind(new InetSocketAddress(9999));
        // 4、获取选择器Selector
        Selector selector = Selector.open();
        // 5、将通道都注册到选择器上去，并且开始指定监听接收事件
        ssChannel.register(selector , SelectionKey.OP_ACCEPT);
        // 6、使用Selector选择器轮询已经就绪好的事件
        while (selector.select() > 0){
            System.out.println("开始一轮事件处理~~~");
            // 7、获取选择器中的所有注册的通道中已经就绪好的事件
            Iterator<SelectionKey> it = selector.selectedKeys().iterator();
            // 8、开始遍历这些准备好的事件
            while (it.hasNext()){
                // 提取当前这个事件
                SelectionKey sk = it.next();
                // 9、判断这个事件具体是什么
                if(sk.isAcceptable()){
                    // 10、直接获取当前接入的客户端通道
                    SocketChannel schannel = ssChannel.accept();
                    // 11 、切换成非阻塞模式
                    schannel.configureBlocking(false);
                    // 12、将本客户端通道注册到选择器
                    schannel.register(selector , SelectionKey.OP_READ);
                }else if(sk.isReadable()){
                    // 13、获取当前选择器上的读就绪事件
                    SocketChannel sChannel = (SocketChannel) sk.channel();
                    // 14、读取数据
                    ByteBuffer buf = ByteBuffer.allocate(1024);
                    int len = 0;
                    while((len = sChannel.read(buf)) != 0){
                        buf.flip();
                        System.out.println(new String(buf.array() , 0, len));
                        buf.clear();// 清除之前的数据
                    }
                }
                it.remove(); // 处理完毕之后需要移除当前事件
            }
        }
    }
}
```

##### 1.5.3.3 客户端流程

1.  获取通道

    ```java
    SocketChannel sChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 9999));
    ```

2.  切换非阻塞模式

    ```java
    sChannel.configureBlocking(false);
    ```

3.  分配指定大小的缓冲区

    ```java
    ByteBuffer buf = ByteBuffer.allocate(1024);
    ```

4.  发送数据给服务端

    ```java
    public class Client {
        public static void main(String[] args) {
            try {
                System.out.println("客户端 启动！");
                // 获取通道
                SocketChannel sChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 9999));
                // 设置为非阻塞模式
                sChannel.configureBlocking(false);
                // 创建1024字节的缓冲区
                ByteBuffer buf = ByteBuffer.allocate(1024);
                // 发送数据给服务端
                Scanner sc = new Scanner(System.in);
                while (true) {
                    System.out.print("请说！");
                    String msg = sc.nextLine();
                    // 将输入的话添加到缓冲区中以字节形式
                    buf.put(("波仔: " + msg).getBytes());
                    // 切换可读模式
                    buf.flip();
                    // 写出缓冲区中的数据
                    sChannel.write(buf);
                    // 清空缓冲区
                    buf.clear();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
    ```

执行结果：

![recording](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/recording.gif)

#### 1.5.4 NIO非阻塞式网络通信入门案例

需求：服务端接收客户端的连接请求，并接收多个客户端发送过来的事件

```java
/**
  客户端
 */
public class Client {

	public static void main(String[] args) throws Exception {
		//1. 获取通道
		SocketChannel sChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", 9999));
		//2. 切换非阻塞模式
		sChannel.configureBlocking(false);
		//3. 分配指定大小的缓冲区
		ByteBuffer buf = ByteBuffer.allocate(1024);
		//4. 发送数据给服务端
		Scanner scan = new Scanner(System.in);
		while(scan.hasNext()){
			String str = scan.nextLine();
			buf.put((new SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(System.currentTimeMillis())
					+ "\n" + str).getBytes());
			buf.flip();
			sChannel.write(buf);
			buf.clear();
		}
		//5. 关闭通道
		sChannel.close();
	}
}

/**
 服务端
 */
public class Server {
    public static void main(String[] args) throws IOException {
        //1. 获取通道
        ServerSocketChannel ssChannel = ServerSocketChannel.open();
        //2. 切换非阻塞模式
        ssChannel.configureBlocking(false);
        //3. 绑定连接
        ssChannel.bind(new InetSocketAddress(9999));
        //4. 获取选择器
        Selector selector = Selector.open();
        //5. 将通道注册到选择器上, 并且指定“监听接收事件”
        ssChannel.register(selector, SelectionKey.OP_ACCEPT);
        //6. 轮询式的获取选择器上已经“准备就绪”的事件
        while (selector.select() > 0) {
            System.out.println("轮一轮");
            //7. 获取当前选择器中所有注册的“选择键(已就绪的监听事件)”
            Iterator<SelectionKey> it = selector.selectedKeys().iterator();
            while (it.hasNext()) {
                //8. 获取准备“就绪”的是事件
                SelectionKey sk = it.next();
                //9. 判断具体是什么事件准备就绪
                if (sk.isAcceptable()) {
                    //10. 若“接收就绪”，获取客户端连接
                    SocketChannel sChannel = ssChannel.accept();
                    //11. 切换非阻塞模式
                    sChannel.configureBlocking(false);
                    //12. 将该通道注册到选择器上
                    sChannel.register(selector, SelectionKey.OP_READ);
                } else if (sk.isReadable()) {
                    //13. 获取当前选择器上“读就绪”状态的通道
                    SocketChannel sChannel = (SocketChannel) sk.channel();
                    //14. 读取数据
                    ByteBuffer buf = ByteBuffer.allocate(1024);
                    int len = 0;
                    while ((len = sChannel.read(buf)) > 0) {
                        buf.flip();
                        System.out.println(new String(buf.array(), 0, len));
                        buf.clear();
                    }
                }
                //15. 取消选择键 SelectionKey
                it.remove();
            }
        }
    }
}
```

#### 1.5.5 NIO网络编程应用实例-群聊系统

##### 目标

需求：进一步理解NIO非阻塞网络编程机制，实现多人群聊

-  编写一个NIO群聊系统，实现客户端与客户端的通信需求(非阻塞)
-  服务端：可以监测用户上线，离线，并实现消息转发功能
-  客户端：通过channel可以无阻塞发送消息给其它所有客户端用户，同时可以接收其它客户端用户通过服务端转发过来的消息

服务端代码实现：

```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.util.Iterator;

// 服务端群聊系统实现
public class Server {
    // 定义成员属性：选择器，服务端通道，端口
    private Selector selector;
    private ServerSocketChannel ssChannel;
    private static final int PORT = 9999;

    // 定义初始化代码逻辑
    public Server() {
        try {
            // 接收选择器
            selector = Selector.open();
            // 获取通道
            ssChannel = ServerSocketChannel.open();
            // 绑定连接端口
            ssChannel.bind(new InetSocketAddress(PORT));
            // 设置非阻塞通信模式
            ssChannel.configureBlocking(false);
            // 把通道注册到选择器上并且指定接收连接事件
            ssChannel.register(selector, SelectionKey.OP_ACCEPT);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    public static void main(String[] args) {
        System.out.println("服务端 启动！");
        // 初始化服务端对象
        Server server = new Server();
        // 开始监听客户端的各种消息事件：连接，群聊消息，离线消息
        server.listen();
    }

    // 开始监听
    private void listen() {
        try {
            // 轮询事件
            while (selector.select() > 0) {
                // 获取选择器中所有注册通道的就绪事件
                Iterator<SelectionKey> it = selector.selectedKeys().iterator();
                // 遍历事件
                while (it.hasNext()) {
                    // 提取事件
                    SelectionKey sk = it.next();
                    // 判断事件类型
                    if (sk.isAcceptable()) {
                        // isAcceptable = true 说明 客户端接入了
                        // 获取当前客户端通道
                        SocketChannel channel = ssChannel.accept();
                        // 设置当前客户端通道为 非阻塞模式
                        channel.configureBlocking(false);
                        // 注册 读取 事件到选择器上
                        channel.register(selector, SelectionKey.OP_READ);
                    } else if (sk.isReadable()) {
                        // 如果客户端发送了读事件就会进入到当前代码块
                        // 处理这个客户端的消息，接收它然后实现转发逻辑
                        // 专门写一个api叫readClientData去读取当前客户端的数据
                        // 传入事件，这个事件是定位当前客户端的关键它是一个反向代理的一个对象，它可以提取当前客户端的通道
                        readClientData(sk);
                    }
                    // 处理完毕之后需要移除当前事件
                    it.remove();
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    // 接收当前客户端通道的消息，转发给其它全部客户端通道
    private void readClientData(SelectionKey sk) {
        SocketChannel channel = null;
        try {
            // 获取当前客户端的通道
            channel = (SocketChannel) sk.channel();
            // 创建缓冲区
            ByteBuffer buf = ByteBuffer.allocate(1024);
            // 读取数据
            int count = channel.read(buf);
            // 判断是否读到了数据
            if (count > 0) {
                // 切换可读模式
                buf.flip();
                // 将读取的数据传入msg中
                String msg = new String(buf.array(), 0, buf.remaining());
                System.out.println("接收到了客户端消息：" + msg);
                // 把这个消息推送给全部客户端接收，传入消息和 当前通道，传入当前通道是为了判断这个消息是否要发送给自己
                sendMsgToAllClient(msg, channel);
            }
        } catch (IOException e) {
            System.out.println("有人离线了");
            // 当前客户端离线
            try {
                System.out.println(channel.getRemoteAddress() + " 离线了");
                // 取消注册
                // 把当前通道从选择器里面取消掉而事件也会随之取消掉，取消掉以后就不会去管这个通道了
                sk.cancel();
                // 取消通道后也应该将其关闭
                channel.close();
            } catch (IOException ex) {
                throw new RuntimeException(ex);
            }
        }
    }

    // 把当前客户端的消息推送给当前全部在线注册的通道
    private void sendMsgToAllClient(String msg, SocketChannel channel) throws IOException {
        System.out.println("服务端开始转发 消息 当前处理的线程：" + Thread.currentThread().getName());
        // 获取选择器中的所有事件
        for (SelectionKey key : selector.keys()) {
            // 得到所有通道
            /**
             * 在把消息分发给所有通道时我们需要注意的是，我们从选择器中提取的所有的事件对应的通道
             * 这个通道会有一个服务端自己的通道，服务端自己的通道也是注册到了多路复用器的，所以这里我们不能直接强转成
             * SocketChannel
             * 这样就可以排除掉 服务端的通道 和 当前客户端通道
             */
            Channel channel1 = (Channel) key.channel();
            // 将自己的通道从所有通道中排除掉
            if (channel1 instanceof SocketChannel && channel != channel1) {
                // 将msg转换为字节数据包装到ByteBuffer中
                ByteBuffer buf = ByteBuffer.wrap(msg.getBytes());
                ((SocketChannel)channel1).write(buf);
            }
        }
    }
}
```

客户端

```java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
import java.util.Scanner;

// 目标：客户端代码逻辑的实现
public class Client {
    // 定义客户端相关属性
    private Selector selector;
    private SocketChannel socketChannel;
    private static final int PORT = 9999;

    public Client() {
        try {
            // 创建选择器
            selector = Selector.open();
            // 连接服务端 获取通道
            socketChannel = SocketChannel.open(new InetSocketAddress("127.0.0.1", PORT));
            // 设置当前通道为非阻塞模式
            socketChannel.configureBlocking(false);
            // 注册事件到选择器
            // 接收服务端的读事件，而不用监听服务端的接入事件，因为接入事件是 客户端 找 服务端的事儿
            socketChannel.register(selector, SelectionKey.OP_READ);
            System.out.println("当前客户端准备完成");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    public static void main(String[] args) {
        Client client = new Client();
        // 定义一个线程专门负责监听服务端发送过来的读消息事件
        new Thread(new Runnable() {
            @Override
            public void run() {
                client.readInfo();
            }
        }).start();
        // 发送消息
        Scanner sc = new Scanner(System.in);
        // 如果有输入消息就执行
        while (sc.hasNextLine()) {
            // 接收输入的消息
            String s = sc.nextLine();
            // 调用函数发送给服务端
            client.sendToServer(s);
        }
    }

    private void sendToServer(String s) {
        try {
            // 将输入的消息写出给服务端
            socketChannel.write(ByteBuffer.wrap(("波仔：" + s).getBytes()));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private void readInfo() {
        try {
            // 如果有事件就会执行，使用循环让其不断地 获取是否有 通道事件
            while (selector.select() > 0) {
                // 遍历事件
                Iterator<SelectionKey> it = selector.selectedKeys().iterator();
                while (it.hasNext()) {
                    // 提取事件类型
                    SelectionKey sk = it.next();
                    // 判断是否为读事件，当然服务端发送过来的都是读事件
                    if (sk.isReadable()) {
                        // 得到当前通道
                        SocketChannel sc = (SocketChannel) sk.channel();
                        // 创建缓冲区
                        ByteBuffer buf = ByteBuffer.allocate(1024);
                        // 将数据从通道读取到缓冲区
                        sc.read(buf);
                        // 打印输出trim防止 服务端送过来空的多余的字符所以进行清除
                        System.out.println(new String(buf.array()).trim());
                    }
                    // 事件处理完毕后移除事件
                    it.remove();
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

总结：

1.  注册选择器，端口，客户端通道
2.  得到选择器，得到客户端与服务端连接通道，通道注册为非阻塞，注册读事件到选择器上

执行效果：

![image-20240311093828540](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240311093828540.png)
