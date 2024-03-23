---
title: TCP+JFrame实现聊天
categories:
   - [计算机学科,java,案例Demo]
tags:
   - 计算机学科
   - java
   - 案例Demo
   - 网络编程
---

	第一步，完成界面搭建

![6091766aa0904e5fbe51cc24bead6491.png](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051347544.png)

要求：

-  创建两个窗口：一个客户端，一个服务端，完成代码书写

步骤：

1.  定义JFrame窗体中的组件：多行文本域，滚动条，画板，单行文本域，按钮

    ```java
    //    画板
        private JPanel jPanel;
    //    多行文本域
        private JTextArea jTextArea;
    //    单行文本域
        private JTextField jTextField;
    //    按钮
        private JButton jButton;
    //    滚动条
        private JScrollPane jScrollPane;
    ```

2.  在构造方法中初始化窗口的组件

    ```java
    //        初始化组件
            jTextArea = new JTextArea();
    //        默认文本域不可编辑
            jTextArea.setEditable(false);
    //        初始化滚动条
            jScrollPane = new JScrollPane(jTextArea);
    //        初始化画板
            jPanel = new JPanel();
    //         初始化文本框,设置输入大小为10个字符
            jTextField = new JTextField(10);
            jButton = new JButton("发送消息");
            jPanel.add(jTextField);
            jPanel.add(jButton);
    ```

    注意：需要将滚动条与画板全部添加到窗体中，继承了窗体的属性，这里this就是窗体

3.  设置“标题”，大小，位置，关闭，是否可见

    ```java
    this.setTitle("B-方");
    this.setSize(500,500);
    this.setLocation(758,236);
    this.setFont(new Font("楷体",0,12));
    this.setVisible(true);
    this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    ```

第二步：TCP通信的思路与步骤

使用网络编程完成数据点的传输(TCP，UDP协议)

TCP协议

>  TCP是面向连接的运输层协议，应用程序在使用TCP协议之前，必须先建立TCP连接，在传送数据完毕后，必须释放已经建立的TCP连接。
>
>  每一条TCP连接只能有两个端点，每一条TCP连接只能是点对点的(一对一)
>
>  TCP提供可靠交互的服务，通过TCP连接传送的数据，无差错，不丢失，不重复，并且按序到达。
>
>  TCP提供全双工通信，TCP允许通信双方的应用进程在任何时候都能发送数据，TCP连接的两端都设有发送缓存和接收缓存，用来临时存放双向通信的数据。
>
>  面向字节流。TCP中的"流"指的是流入到进程或从进程流出的字节序列

![4a76e73a354a4b75bca59c0eb13b5782.png](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051347078.png)

![b1ee168f384d4d419db72cd683b00bc8.png](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051347173.png)

完成代码：

服务器端：

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class ClientDemo  extends JFrame implements ActionListener, KeyListener {
   //    画板
   private JPanel jPanel;
   //    多行文本域
   private JTextArea jTextArea;
   //    单行文本域
   private JTextField jTextField;
   //    按钮
   private JButton jButton;
   //    滚动条
   private JScrollPane jScrollPane;
   private BufferedWriter writer;
   public ClientDemo () {
      //        初始化组件
      jTextArea = new JTextArea();
      //        默认文本域不可编辑
      jTextArea.setEditable(false);
      //        初始化滚动条
      jScrollPane = new JScrollPane(jTextArea);
      //        初始化画板
      jPanel = new JPanel();
      //         初始化文本框,设置输入大小为10个字符
      jTextField = new JTextField(10);
      jButton = new JButton("发送消息");
      jPanel.add(jTextField);
      jPanel.add(jButton);
      this.setTitle("B-方");
      this.setSize(500,500);
      this.setLocation(758,236);
      this.setFont(new Font("楷体",0,12));
      this.setVisible(true);
      this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
      this.add(jPanel,BorderLayout.SOUTH);
      this.add(jScrollPane, BorderLayout.CENTER);
      Socket socket = null;
      jButton.addActionListener(this);
      jTextField.addKeyListener(this);
      try{
         ServerSocket serverSocket = new ServerSocket(8888);
         socket = serverSocket.accept();
         BufferedReader reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
         writer = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
         String str = null;
         while((str = reader.readLine()) != null){
            jTextArea.append(str+System.lineSeparator());
         }
      }catch(Exception e){
         e.printStackTrace();
      }finally{
         if(socket != null){
            try{
               socket.close();
            }catch(IOException e){
               e.printStackTrace();
            }
         }
      }
   }
   public static void main(String...args){
      new ClientDemo();
   }

   @Override
   public void actionPerformed(ActionEvent e) {
      //        当发送按钮被点击后执行
      sendData();
   }

   @Override
   public void keyTyped(KeyEvent e) {

   }

   @Override
   public void keyPressed(KeyEvent e) {
      //        判断键盘按下的事件
      if(e.getKeyCode() == KeyEvent.VK_ENTER){
         sendData();
      }
   }

   @Override
   public void keyReleased(KeyEvent e) {

   }

   public void sendData(){
      //        获取单行文本域的内容
      String text = jTextField.getText();
      text = "B方说:  " + text;
      jTextArea.append(text + System.lineSeparator());
      try{
         writer.write(text);
         writer.newLine();
         writer.flush();
         jTextField.setText("");
      }catch(IOException e){
         e.printStackTrace();
      }
   }
}
```

客户端：

```java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.io.*;
import java.net.Socket;

public class SocketDemo extends JFrame implements ActionListener, KeyListener {
   //    画板
   private JPanel jPanel;
   //    多行文本域
   private JTextArea jTextArea;
   //    单行文本域
   private JTextField jTextField;
   //    按钮
   private JButton jButton;
   //    滚动条
   private JScrollPane jScrollPane;
   private BufferedWriter writer;

   public SocketDemo () {
      //        初始化多行文本域
      jTextArea = new JTextArea();
      jTextArea.setEditable(false);
      //        初始化单行文本域
      jTextField = new JTextField(10);
      //        初始化按钮
      jButton = new JButton("发送消息");
      //        初始化滚动条
      jScrollPane = new JScrollPane(jTextArea);
      //        初始化画板
      jPanel = new JPanel();
      jPanel.add(jTextField);
      jPanel.add(jButton);
      this.setTitle("A-方");
      this.setSize(500,500);
      this.setLocation(758,236);
      this.setFont(new Font("楷体",0,12));
      this.setVisible(true);
      this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
      this.add(jPanel,BorderLayout.SOUTH);
      this.add(jScrollPane, BorderLayout.CENTER);
      Socket socket = null;
      jButton.addActionListener(this);
      jTextField.addKeyListener(this);
      try{
         socket = new Socket("127.0.0.1",8888);
         BufferedReader reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
         writer = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
         String str = null;
         while((str = reader.readLine()) != null){
            jTextArea.append(str+System.lineSeparator());
         }
      }catch(IOException e){
         e.printStackTrace();
      }finally{
         if(socket != null){
            try{
               socket.close();
            }catch(IOException e){
               e.printStackTrace();
            }
         }
      }
   }

   public static void main(String...args){
      new SocketDemo();
   }

   @Override
   public void actionPerformed(ActionEvent e) {
      //        点击按钮后发送消息
      sendData();
   }

   @Override
   public void keyTyped(KeyEvent e) {
      if(e.getKeyCode() == KeyEvent.VK_ENTER){
         sendData();
      }
   }

   @Override
   public void keyPressed(KeyEvent e) {

   }

   @Override
   public void keyReleased(KeyEvent e) {

   }

   public void sendData(){
      String text = jTextField.getText();
      text = "A方说:  " + text;
      jTextArea.append(text + System.lineSeparator());
      try{
         writer.write(text);
         writer.newLine();
         writer.flush();
         jTextField.setText("");
      }catch(IOException e){
         e.printStackTrace();
      }
   }
}
```

效果：

![image-20230530112142014](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401051347989.png)