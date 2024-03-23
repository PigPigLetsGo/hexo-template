---
title: JAVA BIO 深入剖析
categories:
    - [计算机学科,java,知识点,IO]
tags:
    - IO
    - 知识点
---

# 1 JAVA BIO 深入剖析

## 1.1 基本介绍

Java BIO ==就是传统的java io编程==，其相关的类和接口在 java.io

我们学的原生的io流就是放在java.io包下的，也就是说我们基于原生的io流比如 字节流，字符流等进行的数据交互的一种通信方式其实就是一种BIO方式。

**BIO(blocking I/O)**：==同步阻塞==，服务器实现模式为==一个连接一个线程==，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销，可以通过线程池机制改善(实现多个客户端连接服务器)

## 1.2 Java BIO工作机制

![image-20240303174323321](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303174323321.png)

## 1.3 传统的BIO编程实例回顾

网编编程的基本模型是Client/Server模型，也就是两个进程之间进行相互通信，其中服务器端提供位置信息(绑定IP地址和端口)，客户端通过连接操作乡服务端监听的端口地址发起连接请求，基于TCP协议下进行三次握手连接，连接成功后，双方通过网络套接字 (Socket) 进行通信。

传统的同步阻塞模型开发中，服务端ServerSocket负责绑定IP地址，启动监听端口；客户端Socket负责发起基于BIO模式下的通信，客户端- 服务端是完全同步，完全耦合的。

代码：

服务端

```java
/**
 * 目标：客户端发送消息，服务端接收消息。
 */
public class Server {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        try {
            System.out.println("服务端 启动！");
            // 1.定义一个ServerSocket对象进行服务端的端口注册
            serverSocket = new ServerSocket(8080);
            // 2.监听客户端的Socket的连接请求
            socket = serverSocket.accept();
            // 3.从socket管道中得到一个字节输入流对象
            InputStream is = socket.getInputStream();
            // 4.把字节输入流包装成一个缓冲字符输入流
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String msg;
            while (((msg = br.readLine()) != null)) {
                System.out.println("服务端接收到：" + msg);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

客户端

```java
/**
 * 客户端：发送消息到服务端
 */
public class Client {
    public static void main(String[] args) {
        // 1.创建socket对象请求服务端连接
        InetAddress localHost = null;
        try {
            System.out.println("客户端 启动！");
            localHost = InetAddress.getLocalHost();
            Socket socket = new Socket(localHost.getHostAddress(), 8080);
            OutputStream os = socket.getOutputStream();
            PrintStream ps = new PrintStream(os);
            ps.print("hello world! 服务端 你好！");
            ps.flush();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

执行结果：

![image-20240303194303611](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303194303611.png)

我们可以看到 服务端报错了说：连接重置。这是为什么呢？

分析：

服务端刚开始启动的时候就会进入暂停(阻塞)状态在accept代码处，此时会等待客户端的接入

当客户端启动的时候服务端就可以接收到客户端的请求，于是会在下面的代码while(((msg = br.readLine()) != null))按照 ==行== 等数据。

但是因为客户端那边没有数据过来所以服务端就会在while(((msg = br.readLine()) != null)) 暂停等待

而客户端最后发送的数据ps.print("hello world! 服务端 你好！")这是否是一行呢？

这个地方是一个关键，原因就是因为服务端是一种同步阻塞的机制，就是说while(((msg = br.readLine()) != null))会不断地等待客户端的数据而这里是要等一行数据。

而客户端并没有发一行数据而是发了一堆文字

当客户端把数据发送出去后客户端就死掉了，而服务端还在等，服务端在等的时候也会接到客户端的数据但是他并不认为客户端给它的数据是一行数据。服务端一直在等客户端给他发数据因为他认为客户端还有数据，最后客户端死掉后，服务端最终也死掉 就跑出了 连接重置 的异常

这种通信机制是 同步阻塞 制，就是双方都在等对方

如果客户端的socket挂掉那么服务端socket也会跟着一起挂掉

那我们怎么在原有的代码上让客户端发送一行数据给服务端呢？

修改代码如下：

我们使用println 这个方法的意思是：用于打印字符串后并终止该行

服务端

```java
/**
 * 目标：客户端发送消息，服务端接收消息。
 */
public class Server {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        try {
            System.out.println("服务端 启动！");
            // 1.定义一个ServerSocket对象进行服务端的端口注册
            serverSocket = new ServerSocket(8080);
            // 2.监听客户端的Socket的连接请求
            socket = serverSocket.accept();
            // 3.从socket管道中得到一个字节输入流对象
            InputStream is = socket.getInputStream();
            // 4.把字节输入流包装成一个缓冲字符输入流
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String msg;
            while (((msg = br.readLine()) != null)) {
                System.out.println("服务端接收到：" + msg);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

客户端

```java
/**
 * 客户端：发送消息到服务端
 */
public class Client {
    public static void main(String[] args) {
        // 1.创建socket对象请求服务端连接
        InetAddress localHost = null;
        try {
            System.out.println("客户端 启动！");
            localHost = InetAddress.getLocalHost();
            Socket socket = new Socket(localHost.getHostAddress(), 8080);
            OutputStream os = socket.getOutputStream();
            PrintStream ps = new PrintStream(os);
            ps.println("hello world! 服务端 你好！");
            ps.flush();
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

打印结果：

![image-20240303195710801](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303195710801.png)

可以看到 服务端这次接收到了客户端发送过来的数据了，但是还是报错了说：连接重置

这是因为服务端接收到客户端的一条数据后 他并没有停止他还在等客户端的第二条消息，客户端发完消息后就挂了，服务端在等消息时发现客户端挂了，然后他们跟着挂了 然后抛出 连接重置 异常

那怎么解决这个问题呢？我们只需要让服务端不要循环的一直等待客户端的消息，而是在客户端有消息的时候服务端再去接收

服务端修改代码如下：

```java
/**
 * 目标：客户端发送消息，服务端接收消息。
 */
public class Server {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        try {
            System.out.println("服务端 启动！");
            // 1.定义一个ServerSocket对象进行服务端的端口注册
            serverSocket = new ServerSocket(8080);
            // 2.监听客户端的Socket的连接请求
            socket = serverSocket.accept();
            // 3.从socket管道中得到一个字节输入流对象
            InputStream is = socket.getInputStream();
            // 4.把字节输入流包装成一个缓冲字符输入流
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String msg;
            if (((msg = br.readLine()) != null)) {
                System.out.println("服务端接收到：" + msg);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

打印结果：

![image-20240303200055745](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303200055745.png)

服务端接收到客户端的消息后就会判断读取到的数据是否 不为null 当客户端挂掉后 服务端就不会去读取客户端的消息了，也就不会抛出 连接重置 的异常信息了

## 1.4 小结

-  在以上通信中，服务端会一直等待客户端的消息，如果客户端没有进行消息的发送，服务端将一直进入阻塞状态
-  同时服务端是按照==行==获取消息的，这意味着客户端也必须按照==行==进行消息的发送，否则服务端将进入等待消息的阻塞状态！

## 1.5 BIO模式下多发和多收消息

在1.3的案例中，==只能实现客户端发送消息，服务端接收消息==，并不能实现反复的收消息和反复的发消息，我们只需要在客户端案例中，加上反复按照行发送消息的逻辑即可！案例代码如下：

客户端

通过while循环加上扫描器来进行不断地询问发送消息给服务端

```java
/**
 * 客户端：反复的发送消息到服务端
 */
public class Client {
    public static void main(String[] args) {
        // 1.创建socket对象请求服务端连接
        InetAddress localHost = null;
        try {
            System.out.println("客户端 启动！");
            localHost = InetAddress.getLocalHost();
            Socket socket = new Socket(localHost.getHostAddress(), 8080);
            OutputStream os = socket.getOutputStream();
            PrintStream ps = new PrintStream(os);
            Scanner scanner = new Scanner(System.in);
            while(true) {
                System.out.printf("请说！");
                String next = scanner.next();
                ps.println(next);
                ps.flush();
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

服务端

只需要将if 再改回来 成 while 循环不断地去接收客户端发送的消息

```java
/**
 * 目标：服务端可以反复的接收消息，客户端可以反复的发送消息
 */
public class Server {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        try {
            System.out.println("服务端 启动！");
            // 1.定义一个ServerSocket对象进行服务端的端口注册
            serverSocket = new ServerSocket(8080);
            // 2.监听客户端的Socket的连接请求
            socket = serverSocket.accept();
            // 3.从socket管道中得到一个字节输入流对象
            InputStream is = socket.getInputStream();
            // 4.把字节输入流包装成一个缓冲字符输入流
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String msg;
            while (((msg = br.readLine()) != null)) {
                System.out.println("服务端接收到：" + msg);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

打印结果：

![image-20240303201238782](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303201238782.png)

### 小结

-  本案例中确实可以实现客户端多发多收
-  但是服务端只能处理一个客户端的请求，因为服务端是单线程的。一次只能与一个客户端进行消息通信

## 1.6 BIO模式下接受多个客户端

### 概述

在上述的案例中，一个服务端只能接受一个客户端的通信请求，那么如果服务端需要处理很多个客户端的消息通信请求应该如何处理呢？此时我们就需要在服务端引入线程了，也就是说客户端每发起一个请求，服务端就创建一个新的线程来处理这个客户端的请求，这样就实现了一个客户端一个线程的模型，图解模式如下：

![image-20240303202416569](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303202416569.png)

先看上面的代码的方式，当客户端启动多个会造成的问题！

客户端

```java
/**
 * 客户端：发送消息到服务端
 */
public class Client {
    public static void main(String[] args) {
        // 1.创建socket对象请求服务端连接
        InetAddress localHost = null;
        try {
            System.out.println("客户端 启动！");
            localHost = InetAddress.getLocalHost();
            Socket socket = new Socket(localHost.getHostAddress(), 8080);
            OutputStream os = socket.getOutputStream();
            PrintStream ps = new PrintStream(os);
            Scanner scanner = new Scanner(System.in);
            while(true) {
                System.out.printf("请说！");
                String next = scanner.next();
                ps.println(next);
                ps.flush();
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

服务端

```java
/**
 * 目标：服务端可以反复的接收消息，客户端可以反复的发送消息
 */
public class Server {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        try {
            System.out.println("服务端 启动！");
            // 1.定义一个ServerSocket对象进行服务端的端口注册
            serverSocket = new ServerSocket(8080);
            // 2.监听客户端的Socket的连接请求
            socket = serverSocket.accept();
            // 3.从socket管道中得到一个字节输入流对象
            InputStream is = socket.getInputStream();
            // 4.把字节输入流包装成一个缓冲字符输入流
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String msg;
            while (((msg = br.readLine()) != null)) {
                System.out.println("服务端接收到：" + msg);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

我们启动多个客户端来给服务端发送消息测试一下会出什么问题

在Edit Configurations中勾选上 可以多开的选项

![image-20240303203147907](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303203147907.png)

我们启动一起服务端然后启动两个客户端

![image-20240303203345659](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303203345659.png)

我们可以看到 第二个启动的 客户端发送消息服务端并不会接收到 他的消息，中间的是最后一个启动的客户端

![image-20240303203512427](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303203512427.png)

这是为什么？

分析：

当服务端启动时accept等待客户端连接，此时客户端1启动连接了服务，此时服务端代码就会执行到 while (((msg = br.readLine()) != null)) 服务端就不会再去接收其他客户端的请求了，所以客户端2没有连接到服务端发送消息自然不会有人接收

使用线程来解决问题

代码：

服务端

```java
/**
 * 目标：实现服务端可以同时接收多个客户端的Socket通信请求
 * 思路：是服务端每接收到一个客户端Socket请求对象之后都交给一个独立的线程来处理客户端的数据交互需求
 */
public class Server {
    public static void main(String[] args) {
        try {
            System.out.println("服务端 启动！");
            ServerSocket serverSocket = new ServerSocket(8080);
            // 定义一个死循环不断地接收客户端Socket请求
            while (true) {
                Socket socket = serverSocket.accept();
                // 创建一个独立的线程来处理与这个客户端的Socket通信请求
                new ServerThreadReader(socket).start();
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

线程类

```java
public class ServerThreadReader extends Thread {
    private Socket socket;

    public ServerThreadReader(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            InputStream is = socket.getInputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String msg;
            while (((msg = br.readLine()) != null)) {
                System.out.println(msg);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }

    }
}
```

客户端

```java
public class Client {
    public static void main(String[] args) {
        try {
            System.out.println("客户端 启动！");
            // 请求与服务端的Socket对象连接
            InetAddress localHost = InetAddress.getLocalHost();
            Socket socket = new Socket(localHost.getHostAddress(), 8080);
            // 得到打印流
            PrintStream ps = new PrintStream(socket.getOutputStream());
            Scanner sc = new Scanner(System.in);
            while (true) {
                System.out.print("请说！");
                String next = sc.next();
                ps.println(next);
                ps.flush();
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```



打印结果：

![image-20240303230637523](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303230637523.png)

### 小结

-  1 每个Socket接收到，都会创建一个线程，线程的竞争，切换上下文影响性能
-  2 每个线程都会占用栈空间和CPU资源
-  3 并不是每个Socket都进行IO操作，无意义的线程处理
-  4 客户端的并发访问增加时。服务端将呈现1:1的线程开销，访问量越大，系统将发生线程栈溢出，线程创建失败，最终导致进程宕机或者僵死，从而不能对外提供服务

## 1.7 伪异步I/O编程

### 概述

在上述案例中：客户端的并发访问增加时。服务端将呈现1:1的线程开销，访问量越大，系统将发生线程栈溢出，线程创建失败，最终导致进程宕机或者僵死，从而不能对外提供服务。

接下来我们采用一个伪异步I/O的通信框架，采用线程池和任务队列实现，当客户端接入时，将客户端的Socket封装成一个Task(该任务实现java.lang.Runnable线程任务接口)交给后端的线程池中进行处理。JDK的线程池维护一个消息队列和N个活跃的线程，对消息队列中Socket任务进行处理，由于线程池可以设置消息队列的大小和最大线程数，因此，它的资源占用是可控的，无论多少个客户端并发访问，都不会导致资源的耗尽和宕机

图示如下：

![image-20240303232928002](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240303232928002.png)

代码：

服务端

创建线程类

```java
public class ServerRunnableTarget implements Runnable {
    private Socket socket;

    public ServerRunnableTarget(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        // 处理接收到的客户端socket通信需求
        try {
            InputStream is = socket.getInputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(is));
            String msg;
            while ((msg = br.readLine()) != null) {
                System.out.println("服务端接收到：" + msg);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

创建线程池类

```java
public class HandlerSocketServerPool {
    // 创建一个线程池的成员变量 用于存储一个线程池对象
    private ExecutorService executorService;
    // 创建这个类的对象的时候就需要初始化线程池对象
    public HandlerSocketServerPool (int maxThreadNum, int queueSize) {
        // 参数1：核心线程数量，参数2：最大线程数量，参数3：线程空闲时间，参数4：时间单位
        executorService = new ThreadPoolExecutor(3, maxThreadNum, 120, TimeUnit.SECONDS,
                new ArrayBlockingQueue<Runnable>(queueSize));
    }

    // 提供一个方法来提交任务给线程池的任务队列来暂存，等着线程池来处理
    public void execute(Runnable target) {
        executorService.execute(target);
    }
}
```

创建服务端类

```java
/**
 * 目标：开发实现伪异步通信架构
 */
public class Server {
    public static void main(String[] args) {
        // 注册端口
        try {
            System.out.println("服务端 启动！");
            ServerSocket serverSocket = new ServerSocket(8080);
            HandlerSocketServerPool pool = new HandlerSocketServerPool(6, 10);
            // 定义一个循环接收客户端的Socket连接请求
            while (true) {
                Socket socket = serverSocket.accept();
                // 把socket对象交给一个线程池处理
                // 把socket封装成一个任务对象交给线程池处理
                ServerRunnableTarget target = new ServerRunnableTarget(socket);
                pool.execute(target);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

客户端

```java
public class Client {
    public static void main(String[] args) {
        try {
            System.out.println("客户端 启动！");
            InetAddress localHost = InetAddress.getLocalHost();
            Socket socket = new Socket(localHost.getHostAddress(), 8080);
            OutputStream os = socket.getOutputStream();
            PrintStream ps = new PrintStream(os);
            Scanner sc = new Scanner(System.in);
            while (true) {
                System.out.print("请说！");
                String next = sc.next();
                ps.println(next);
                ps.flush();
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

打印结果:

![image-20240304000841607](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240304000841607.png)

### 小结

-  伪异步IO采用了线程池实现，因此避免了为每个请求创建一个独立线程造成线程资源耗尽的问题，但由于底层依然是采用的同步阻塞模型，因此无法从根本上解决问题
-  如果单个消息处理的缓慢，或者服务器线程池中的全部线程都被阻塞，那么后续socket的io消息都将在队列中排队。新的socket请求将被拒绝，客户端会发生大量连接超时

## 1.8 基于BIO形式下的文件上传

### 目标

支持任意类型文件形式的上传

线程类

```java
public class ServerReaderThread extends Thread {
    private Socket socket;
    public ServerReaderThread(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            // 得到一个数据输入流读取客户端发送过来的数据
            DataInputStream dis = new DataInputStream(socket.getInputStream());
            // 读取客户端发送过来的文件类型
            // 接收客户端发送过来的utf-8字符串
            String suffix = dis.readUTF();
            System.out.println("服务端已经成功接收到了文件类型：" + suffix);
            // 定义一个字节输出管道负责把客户端发过来的文件数据写出去
            FileOutputStream os = new FileOutputStream("C:\\Users\\Administrator\\Desktop\\download\\" +
                     UUID.randomUUID().toString() + suffix);
            // 从数据输入流中读取文件数据，写出到字节输出流中去
            byte[] bytes = new byte[1024];
            int i = -1;
            while (((i = dis.read(bytes)) != -1)) {
                os.write(bytes, 0, i);
            }
            os.close();
            System.out.println("服务端接收文件，保存成功！");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

服务端

```java
/**
 * 目标：服务端开发，可以实现接收客户端的任意类型文件，并保存到服务端磁盘。
 */
public class Server {
    public static void main(String[] args) {
        try {
            ServerSocket serverSocket = new ServerSocket(8080);
            while (true) {
                Socket socket = serverSocket.accept();
                // 交给一个独立的线程来处理与这个客户端的文件通信需求
                new ServerReaderThread(socket).start();
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

客户端

```java
/**
 * 目标：实现客户端上传任意类型的文件数据给服务端保存起来
 */
public class Client {
    public static void main(String[] args) {
        // 请求与服务端的Socket连接
        try {
            InetAddress localHost = InetAddress.getLocalHost();
            Socket socket = new Socket(localHost.getHostAddress(), 8080);
            // 把字节输出流作一个包装 包装成一个数据输出流
            DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
            // 先发送上传文件的后缀给服务端 （先让服务端知道自己接收的是什么类型的文件）
            // 发送utf-8编码的字符串到服务端中
            dos.writeUTF(".png");
            // 把文件数据发送给服务端进行接收
            FileInputStream is = new FileInputStream("C:\\Users\\Administrator\\Desktop\\Minecraft.png");
            byte[] bytes = new byte[1024];
            int i = -1;
            while (((i = is.read(bytes)) != -1)) {
                dos.write(bytes, 0, i);
            }
            dos.flush();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

测试代码

![image-20240304211407647](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240304211407647.png)

可以看到 客户端发送的文件 服务端接收到了，但是服务端 却报错了！

而且 图片 也不能正常打开查看了。

![image-20240304211519844](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240304211519844.png)

为什么服务端报错呢？

因为，客户端发送了一个文件后 服务端代码执行到while (((i = dis.read(bytes)) != -1))，服务端一直在等而客户端死掉了服务端也就跟着死掉然后服务端就跑出了 连接重置的 异常了

之前使用if 解决了该问题，这次就不能使用if了 因为客户端发送文件数据是一个循环，而服务端如果使用if的话 文件数据只发送一次根本发送不完。

我们可以使用socket的shudownOutput()来 告诉服务端 上一个回话已经结束了来解决报错。

代码如下：

线程类

```java
public class ServerReaderThread extends Thread {
    private Socket socket;
    public ServerReaderThread(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            // 得到一个数据输入流读取客户端发送过来的数据
            DataInputStream dis = new DataInputStream(socket.getInputStream());
            // 读取客户端发送过来的文件类型
            // 接收客户端发送过来的utf-8字符串
            String suffix = dis.readUTF();
            System.out.println("服务端已经成功接收到了文件类型：" + suffix);
            // 定义一个字节输出管道负责把客户端发过来的文件数据写出去
            FileOutputStream os = new FileOutputStream("C:\\Users\\Administrator\\Desktop\\download\\" +
                     UUID.randomUUID().toString() + suffix);
            // 从数据输入流中读取文件数据，写出到字节输出流中去
            byte[] bytes = new byte[1024];
            int i = -1;
            while (((i = dis.read(bytes)) != -1)) {
                os.write(bytes, 0, i);
            }
            os.close();
            System.out.println("服务端接收文件，保存成功！");
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

服务端

```java
/**
 * 目标：服务端开发，可以实现接收客户端的任意类型文件，并保存到服务端磁盘。
 */
public class Server {
    public static void main(String[] args) {
        try {
            ServerSocket serverSocket = new ServerSocket(8080);
            while (true) {
                Socket socket = serverSocket.accept();
                // 交给一个独立的线程来处理与这个客户端的文件通信需求
                new ServerReaderThread(socket).start();
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

客户端

```java
/**
 * 目标：实现客户端上传任意类型的文件数据给服务端保存起来
 */
public class Client {
    public static void main(String[] args) {
        // 请求与服务端的Socket连接
        try {
            InetAddress localHost = InetAddress.getLocalHost();
            Socket socket = new Socket(localHost.getHostAddress(), 8080);
            // 把字节输出流作一个包装 包装成一个数据输出流
            DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
            // 先发送上传文件的后缀给服务端 （先让服务端知道自己接收的是什么类型的文件）
            // 发送utf-8编码的字符串到服务端中
            dos.writeUTF(".png");
            // 把文件数据发送给服务端进行接收
            FileInputStream is = new FileInputStream("C:\\Users\\Administrator\\Desktop\\Minecraft.png");
            byte[] bytes = new byte[1024];
            int i = -1;
            while (((i = is.read(bytes)) != -1)) {
                dos.write(bytes, 0, i);
            }
            dos.flush();
            socket.shutdownOutput();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

打印结果

![image-20240304212555193](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240304212555193.png)

我们再查看下载好的文件

![image-20240304212626337](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240304212626337.png)

### 小结

-  客户端发送文件数据到服务端此时客户端使用while发送给服务端数据从而服务端不能使用if来解决报错问题，我们可以在客户端发送完文件数据后通过Socket调用shutdownOutput()来告诉服务端这次的回话结束了，服务端就不会报错了
-  客户端发送文件使用的是数据输出流那么我们在服务端接收文件数据的时候就需要使用数据输入流来接收(什么流对应什么流)

## 1.9 Java BIO 模式下的端口转发思想

**需求**：需要实现一个客户端的消息可以发送给所有的客户端去接收。(群聊实现)

我们要实现上面需求 也不能省略不访问服务端的，以QQ为例，他其实就是把消息推送到 QQ的服务器然后在转发到其他的客户端(自己也是一个客户端)，我们把这种称之为端口转发的思想

![image-20240304213533355](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240304213533355.png)

客户端的线程类

```java
public class ClientReaderThread extends Thread {
    private Socket socket;
    public ClientReaderThread(Socket socket){
        this.socket = socket;
    }
    @Override
    public void run() {
        try{
            BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            String msg;
            while ((msg = br.readLine())!=null){
                System.out.println(msg);
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

客户端

```java
/**
 目标：实现客户端的开发
 基本思路：
 1、客户端发送消息给服务端
 2、客户端可能还需要接收服务端发送过来的消息
 */
public class Client {
    public static void main(String[] args) {
        try{
            InetAddress localHost = InetAddress.getLocalHost();
            // 1、创建于服务端的Socket链接
            Socket socket = new Socket(localHost.getHostAddress(), 8080);
            // 4、分配一个线程为客户端socket服务接收服务端发来的消息
            new ClientReaderThread(socket).start();
            // 2、从当前socket管道中得到一个字节输出流对应的打印流
            PrintStream ps = new PrintStream(socket.getOutputStream());
            // 3、接收用户输入的消息发送出去
            Scanner sc = new Scanner(System.in);
            while (true) {
                String msg = sc.nextLine();
                ps.println("波妞："+msg);
                ps.flush();
            }
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

服务端的线程类

```java
public class ServerReaderThread extends Thread {
    private Socket socket;
    public ServerReaderThread (Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        // 从socket中去获取当前客户端的输入流
        try {
            BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            String msg;
            while (((msg = br.readLine()) != null)) {
                // 服务端接收到了客户端的消息之后，是需要推送给当前所有的在线socket
                sendMsgToAllClient(msg);
            }
            // 如果有Socket下线了那么服务端的br.readLine处就会抛出异常此时我们在catch中来进行处理下线的Socket
        } catch (IOException e) {
            System.out.println(socket.getLocalAddress() + " 下线了！");
            // 从在线Socket集合中移除 下线的Socket
            Server.allSocketOneLine.remove(socket);
        }
    }

    /**
     * 把当前客户端发来的消息推送给全部在线的Socket
     * @param msg
     */
    private void sendMsgToAllClient(String msg) {
        try {
            for (Socket sk : Server.allSocketOneLine) {
                PrintStream ps = new PrintStream(sk.getOutputStream());
                ps.println(msg);
                ps.flush();
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

服务端

```java
/**
 * 目标：BIO模式下的端口转发思想-服务端实现
 * 服务端实现的需求：
 * 1、注册端口
 * 2、接收客户端的Socket连接，交给一个独立的线程来处理
 * 3、把当前连接的客户端Socket存入一个所谓的在线Socket集合中保存
 * 4、接收客户端的消息，然后推送给当前所有在线的Socket接收
 */
public class Server {
    // 定义一个静态集合(在整个系统中加载过程中只有一份，存储在线Socket容器就需要一个集合)
    public static List<Socket> allSocketOneLine = new ArrayList<>();
    public static void main(String[] args) {
        try {
            System.out.println("Server Start!");
            ServerSocket serverSocket = new ServerSocket(8080);
            while (true) {
                Socket socket = serverSocket.accept();
                // 把登录的客户端Socket存入到一个在线集合中去
                allSocketOneLine.add(socket);
                // 为当前登录成功的Socket分配一个独立的线程来处理与之通信
                new ServerReaderThread(socket).start();
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

打印效果

![recording](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/recording.gif)

## 1.10 基于BIO模式下即时通信

基于BIO模式下的即时通信，我们需要解决客户端到客户端的通信，也就是需要实现客户端与客户端端口消息转发逻辑。

### 项目案例说明

本项目案例为即时通信的软件项目，适合基础加强的大案例，具备综合性学习本项目案例至少需要具备如下JavaSE技术点：

-  1 Java面向对象设计，语法设计
-  2 多线程技术
-  3 IO流技术
-  4 网络通信相关技术
-  5 集合框架
-  6 项目开发思维
-  7 Java常用api使用

**功能清单简单说明**：

1.  客户端登录功能

    可以启动客户端进行登录，客户端登录只需要输入用户名和服务端ip地址即可

2.  在线人数实时更新

    客户端用户登录以后，需要同步更新所有客户端的联系人信息栏

3.  离线人数更新

    检测到有客户端下线后，需要同步更新所有客户端的联系人信息栏

4.  群聊

    任意一个客户端的消息，可以推送给当前所有客户端接收

5.  私聊

    可以选择某个员工，点击私聊按钮，然后发出的消息可以被该客户端单独接收

6.  @消息

    可以选择某个员工，然后发出的消息可以@该用户，但是其他所有人都能看到

7.  消息用户和消息时间点

    服务端可以实时记录该用户的消息时间点，然后进行消息的多路转发或者选择

#### 服务端

线程类

```java
public class ServerReader extends Thread {
	private Socket socket;
	public ServerReader(Socket socket) {
		this.socket = socket;
	}

	@Override
	public void run() {
		DataInputStream dis = null;
		try {
			dis = new DataInputStream(socket.getInputStream());
			/** 1.循环一直等待客户端的消息 */
			while(true){
				/** 2.读取当前的消息类型 ：登录,群发,私聊 , @消息 */
				int flag = dis.readInt();
				if(flag == 1){
					/** 先将当前登录的客户端socket存到在线人数的socket集合中   */
					String name = dis.readUTF() ;
					System.out.println(name+"---->"+socket.getRemoteSocketAddress());
					ServerChat.onLineSockets.put(socket, name);
				}
				writeMsg(flag,dis);
			}
		} catch (Exception e) {
			System.out.println("--有人下线了--");
			// 从在线人数中将当前socket移出去  
			ServerChat.onLineSockets.remove(socket);
			try {
				// 从新更新在线人数并发给所有客户端 
				writeMsg(1,dis);
			} catch (Exception e1) {
				e1.printStackTrace();
			}
		}

	}

	private void writeMsg(int flag, DataInputStream dis) throws Exception {
//		DataOutputStream dos = new DataOutputStream(socket.getOutputStream()); 
		// 定义一个变量存放最终的消息形式 
		String msg = null ;
		if(flag == 1){
			/** 读取所有在线人数发给所有客户端去更新自己的在线人数列表 */
			StringBuilder rs = new StringBuilder();
			Collection<String> onlineNames = ServerChat.onLineSockets.values();
			// 判断是否存在在线人数 
			if(onlineNames != null && onlineNames.size() > 0){
				for(String name : onlineNames){
					rs.append(name+ Constants.SPILIT);
				}
				// 去掉最后的一个分隔符
				msg = rs.substring(0, rs.lastIndexOf(Constants.SPILIT));

				/** 将消息发送给所有的客户端 */
				sendMsgToAll(flag,msg);
			}
		}else if(flag == 2 || flag == 3){
			// 读到消息  群发的 或者 @消息
			String newMsg = dis.readUTF() ; // 消息
			// 得到发件人 
			String sendName = ServerChat.onLineSockets.get(socket);

			//    内容--
			StringBuilder msgFinal = new StringBuilder();
			// 时间  
			SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss EEE");
			if(flag == 2){
				msgFinal.append(sendName).append("  ").append(sdf.format(System.currentTimeMillis()*2)).append("\r\n");
				msgFinal.append("    ").append(newMsg).append("\r\n");
				sendMsgToAll(flag,msgFinal.toString());
			}else if(flag == 3){
				msgFinal.append(sendName).append("  ").append(sdf.format(System.currentTimeMillis()*2)).append("对您私发\r\n");
				msgFinal.append("    ").append(newMsg).append("\r\n");
				// 私发 
				// 得到给谁私发 
				String destName = dis.readUTF();
				sendMsgToOne(destName,msgFinal.toString());
			}
		}
	}

	/**
	 * @param destName 对谁私发 
	 * @param msg 发的消息内容 
	 * @throws Exception
	 */
	private void sendMsgToOne(String destName, String msg) throws Exception {
		// 拿到所有的在线socket管道 给这些管道写出消息
		Set<Socket> allOnLineSockets = ServerChat.onLineSockets.keySet();
		for(Socket sk :  allOnLineSockets){
			// 得到当前需要私发的socket 
			// 只对这个名字对应的socket私发消息
			if(ServerChat.onLineSockets.get(sk).trim().equals(destName)){
				DataOutputStream dos = new DataOutputStream(sk.getOutputStream());
				dos.writeInt(2); // 消息类型
				dos.writeUTF(msg);
				dos.flush();
			}
		}

	}

	private void sendMsgToAll(int flag, String msg) throws Exception {
		// 拿到所有的在线socket管道 给这些管道写出消息
		Set<Socket> allOnLineSockets = ServerChat.onLineSockets.keySet();
		for(Socket sk :  allOnLineSockets){
			DataOutputStream dos = new DataOutputStream(sk.getOutputStream());
			dos.writeInt(flag); // 消息类型
			dos.writeUTF(msg);
			dos.flush();
		}
	}
}
```

服务端类

```java
public class ServerChat {

	/**
	 * 定义一个集合存放所有在线的socket
	 * 在线集合只需要一个：存储客户端socket的同时还需要知道这个Socket客户端的名称
	 */
	public static Map<Socket, String> onLineSockets = new HashMap<>();

	public static void main(String[] args) {
		try {
			/** 注册端口   */
			ServerSocket serverSocket = new ServerSocket(Constants.PORT);
			/** 循环一直等待所有可能的客户端连接 */
			while(true){
				Socket socket = serverSocket.accept();
				/** 把客户端的socket管道单独配置一个线程来处理 */
				new ServerReader(socket).start();
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

#### 客户端

线程类

```java
class ClientReader extends Thread {
	private Socket socket;
	private ClientChat clientChat ;

	public ClientReader(ClientChat clientChat, Socket socket) {
		this.clientChat = clientChat;
		this.socket = socket;
	}

	@Override
	public void run() {
		try {
			DataInputStream dis = new DataInputStream(socket.getInputStream());
			/** 循环一直等待客户端的消息 */
			while(true){
				/** 读取当前的消息类型 ：登录,群发,私聊 , @消息 */
				int flag = dis.readInt();
				if(flag == 1){
					// 在线人数消息回来了
					String nameDatas = dis.readUTF();
					// 展示到在线人数的界面
					String[] names = nameDatas.split(Constants.SPILIT);
					clientChat.onLineUsers.setListData(names);
				}else if(flag == 2){
					//群发,私聊 , @消息 都是直接显示的。
					String msg = dis.readUTF() ;
					clientChat.smsContent.append(msg);
					// 让消息界面滾動到底端
					clientChat.smsContent.setCaretPosition(clientChat.smsContent.getText().length());
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

客户端类

```java
/**
 * 客户端界面
 */
public class ClientChat implements ActionListener {
	/** 1.设计界面  */
	private JFrame win = new JFrame();
	/** 2.消息内容框架 */
	public JTextArea smsContent =new JTextArea(23 , 50);
	/** 3.发送消息的框  */
	private JTextArea smsSend = new JTextArea(4,40);
	/** 4.在线人数的区域  */
	/** 存放人的数据 */
	/** 展示在线人数的窗口 */
	public JList<String> onLineUsers = new JList<>();

	// 是否私聊按钮
	private JCheckBox isPrivateBn = new JCheckBox("私聊");
	// 消息按钮
	private JButton sendBn  = new JButton("发送");

	// 登录界面
	private JFrame loginView;

	private JTextField ipEt , nameEt , idEt;

	private Socket socket ;

	public static void main(String[] args) {
		new ClientChat().initView();
	}

	private void initView() {
		/** 初始化聊天窗口的界面 */
		win.setSize(650, 600);
		/** 展示登录界面  */
		displayLoginView();
		/** 展示聊天界面 */
		//displayChatView();
	}

	private void displayChatView() {
		JPanel bottomPanel = new JPanel(new BorderLayout());
		//-----------------------------------------------
		// 将消息框和按钮 添加到窗口的底端
		win.add(bottomPanel, BorderLayout.SOUTH);
		bottomPanel.add(smsSend);
		JPanel btns = new JPanel(new FlowLayout(FlowLayout.LEFT));
		btns.add(sendBn);
		btns.add(isPrivateBn);
		bottomPanel.add(btns, BorderLayout.EAST);
		//-----------------------------------------------
		// 给发送消息按钮绑定点击事件监听器
		// 将展示消息区centerPanel添加到窗口的中间
		smsContent.setBackground(new Color(0xdd,0xdd,0xdd));
		// 让展示消息区可以滚动。
		win.add(new JScrollPane(smsContent), BorderLayout.CENTER);
		smsContent.setEditable(false);
		//-----------------------------------------------
		// 用户列表和是否私聊放到窗口的最右边
		Box rightBox = new Box(BoxLayout.Y_AXIS);
		onLineUsers.setFixedCellWidth(120);
		onLineUsers.setVisibleRowCount(13);
		rightBox.add(new JScrollPane(onLineUsers));
		win.add(rightBox, BorderLayout.EAST);
		//-----------------------------------------------
		// 关闭窗口退出当前程序
		win.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		win.pack();  // swing 加上这句 就可以拥有关闭窗口的功能
		/** 设置窗口居中,显示出来  */
		setWindowCenter(win,650,600,true);
		// 发送按钮绑定点击事件
		sendBn.addActionListener(this);
	}

	private void displayLoginView(){

		/** 先让用户进行登录
		 *  服务端ip
		 *  用户名
		 *  id
		 *  */
		/** 显示一个qq的登录框     */
		loginView = new JFrame("登录");
		loginView.setLayout(new GridLayout(3, 1));
		loginView.setSize(400, 230);

		JPanel ip = new JPanel();
		JLabel label = new JLabel("   IP:");
		ip.add(label);
		ipEt = new JTextField(20);
		ip.add(ipEt);
		loginView.add(ip);

		JPanel name = new JPanel();
		JLabel label1 = new JLabel("姓名:");
		name.add(label1);
		nameEt = new JTextField(20);
		name.add(nameEt);
		loginView.add(name);

		JPanel btnView = new JPanel();
		JButton login = new JButton("登陆");
		btnView.add(login);
		JButton cancle = new JButton("取消");
		btnView.add(cancle);
		loginView.add(btnView);
		// 关闭窗口退出当前程序
		loginView.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setWindowCenter(loginView,400,260,true);
		/** 给登录和取消绑定点击事件 */
		login.addActionListener(this);
		cancle.addActionListener(this);
	}

	private static void setWindowCenter(JFrame frame, int width , int height, boolean flag) {
		/** 得到所在系统所在屏幕的宽高 */
		Dimension ds = frame.getToolkit().getScreenSize();
		/** 拿到电脑的宽 */
		int width1 = ds.width;
		/** 高 */
		int height1 = ds.height ;
		System.out.println(width1 +"*" + height1);
		/** 设置窗口的左上角坐标 */
		frame.setLocation(width1/2 - width/2, height1/2 -height/2);
		frame.setVisible(flag);
	}

	@Override
	public void actionPerformed(ActionEvent e) {
		/** 得到点击的事件源 */
		JButton btn = (JButton) e.getSource();
		switch(btn.getText()){
			case "登陆":
				String ip = ipEt.getText().toString();
				String name = nameEt.getText().toString();
				// 校验参数是否为空
				// 错误提示
				String msg = "" ;
				// 12.1.2.0
				// \d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\
				if(ip==null || !ip.matches("\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}\\.\\d{1,3}")){
					msg = "请输入合法的服务端ip地址";
				}else if(name==null || !name.matches("\\S{1,}")){
					msg = "姓名必须1个字符以上";
				}

				if(!msg.equals("")){
					/** msg有内容说明参数有为空 */
					// 参数一：弹出放到哪个窗口里面
					JOptionPane.showMessageDialog(loginView, msg);
				}else{
					try {
						// 参数都合法了
						// 当前登录的用户,去服务端登陆
						/** 先把当前用户的名称展示到界面 */
						win.setTitle(name);
						// 去服务端登陆连接一个socket管道
						socket = new Socket(ip, Constants.PORT);
						//为客户端的socket分配一个线程 专门负责收消息
						new ClientReader(this,socket).start();
						// 带上用户信息过去
						DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
						dos.writeInt(1); // 登录消息
						dos.writeUTF(name.trim());
						dos.flush();
						// 关系当前窗口 弹出聊天界面
						loginView.dispose(); // 登录窗口销毁
						displayChatView(); // 展示了聊天窗口了
					} catch (Exception e1) {
						e1.printStackTrace();
					}
				}
				break;
			case "取消":
				/** 退出系统 */
				System.exit(0);
				break;
			case "发送":
				// 得到发送消息的内容
				String msgSend = smsSend.getText().toString();
				if(!msgSend.trim().equals("")){
					/** 发消息给服务端 */
					try {
						// 判断是否对谁发消息
						String selectName = onLineUsers.getSelectedValue();
						int flag = 2 ;// 群发 @消息
						if(selectName!=null&&!selectName.equals("")){
							msgSend =("@"+selectName+","+msgSend);
							/** 判断是否选中了私法 */
							if(isPrivateBn.isSelected()){
								/** 私法 */
								flag = 3 ;//私发消息
							}
						}
						DataOutputStream dos = new DataOutputStream(socket.getOutputStream());
						dos.writeInt(flag); // 群发消息  发送给所有人
						dos.writeUTF(msgSend);
						if(flag == 3){
							// 告诉服务端我对谁私发
							dos.writeUTF(selectName.trim());
						}
						dos.flush();

					} catch (Exception e1) {
						e1.printStackTrace();
					}
				}
				smsSend.setText(null);
				break;
		}
	}
}
```

### 小结

-  实现了接收客户端的登录消息，然后提取当前在线的全部的用户名称和当前登录的用户名称发送给全部在线用户更新自己的在线人数列表
-  私聊消息需要只要推送给某个具体的客户端Socket管道
-  我们可以接收客户端发来的私聊用户名称，根据用户名称定位该用户的Socket管道，然后单独推送消息给该Socket管道
-  服务端收到新的登录消息后，会响应一个在线列表用户回来给客户端更新在线人数
-  实现了客户端发送群聊消息，@消息，以及私聊消息
-  如果直接点发送，默认发送群聊消息
-  如果选中右侧在线列表某个用户，默认发送@消息
-  如果选中右侧在线列表某个用户，然后选择右下侧私聊按钮默认发送私聊消息

