---
title: 异常机制和File类
categories:
    - [计算机学科,java,基础-核心类库]
    - java
    - 基础
---

# 异常机制和File类:christmas_tree: 

[TOC]

##### 1.异常机制(重点):palm_tree: 

###### 1.2基本概念:tanabata_tree: 

-   异常就是=="不正常"==的含义,在Java语言中主要指 程序执行中发生的==不正常情况== 
-   java.lang.Throwable类是java语言中==错误(Error)==和==异常(Exception)==的==超类== 
-   其中Error类主要用于描述java虚拟机无法解决的严重错误,通常无法编码解决如,如:0作为除数等(ArithmeticException)

###### 1.3 异常的分类:palm_tree: 

-   java.lang.Exception 类是所有==异常的超类== , 主要分为以下两种:

    RuntimeException - 运行时异常,也叫作==非检测性异常== 

​		  IOException 和其它异常,其他异常,也叫作检测性异常,所谓检测性异常就是指在编译阶段都能被编译器检测出来的异常

-   其中RuntimeException类的主要子类:

​	   ArithmeticException类,-算术异常

​	   ArrayIndexOutOfBoundsException类,-数组下标越界异常

​	   NullPointerException类,-空指针异常

​	   ClassCastException类,-类型转换异常

​	   NumberForamtException类,-数字格式化异常

​       IllegalAccessException类 - 没有访问权限

​       InstantiationException类 - 初始化异常

​	   ClassNotFoundException类 - 没有找到类

​       NoSuchMethodException类 - 没有找到方法

​       NoSuchFieldException类 - 没有找到字段

-   注意: 

    当程序执行过程中发生异常但没有动手处理时,则由java虚拟机采用默认方式处理异常,而默认处理方式就是:打印异常名称,异常发生的原因,异常发生的位置以及终止程序

![image_2023-03-14-12-24-59](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081148711.png)

###### 1.4 异常的避免:palm_tree: 

-   在以后的开发中尽量先使用if条件判断来避免异常的发生


```java
//        算术异常
        int a = 1;
        int b = 0;
        if(a != 0 & b != 0){
            System.out.println(a/b);//ArithmeticException
        }
//        数组下标越界异常
        int[] arr = new int[5];
        int visitindex = 5;
        if(visitindex >= 0 && arr.length > visitindex){
            System.out.println(arr[visitindex]);//IndexOutOfBoundException
        }
//        空指针异常
        String str = null;
        if(str != null){
            System.out.println(str.length());//NullPointerException
        }
//        类型转换异常
        Exception e = new Exception();
        if(e instanceof IOException){
            IOException io = (IOException)e;//ClassCastException
        }
//        数字格式化异常
        String str1 = "123a";
        if(str1.matches("\\d+")){
            System.out.println(Integer.parseInt(str1));//NumberFormatException
        }
        System.out.println("程序正常结束了");
-----------------RUN-----------------
程序正常结束了
```

-   但是过多的if条件判断会导致程序的代码加长,臃肿,可读性差

###### 1.5 异常的捕获:palm_tree: 

- 语法格式

```java
try{
    编写可能发生异常的代码;
}catch(异常类型 引用变量名){
    编写针对该异常的处理代码;
}
#三个点表示:可能有多个不同的异常
...
finally{
    编写无论是否发生异常都要执行的代码;
}
```

- 注意事项

a. 当需要编写多个catch分支时,切记小类型应该放在大类型的前面;

b. 懒人的写法: <font style="color:red">**不建议**</font>,建议在直到会发生什么报错时就针对什么报错比如IO流的IOException多线程的InterruptedException

==PS==(重点)：用到finally关闭资源的时候，给大家提个醒，应该尽量避免在`finally`语句块中出现运行时错误，可以适当添加判断语句以增加程序健壮性。

```java
finally {
    if (out != null) { 
        System.out.println("Closing PrintWriter");
        out.close(); // 不要在finally语句中直接调用close()
    } else { 
        System.out.println("PrintWriter not open");
    } 
}
```

-  catch(Exception e){ }

c. finally通常用于进行善后处理,如:关闭已经 开打的文件等

- 执行流程

```java
try{
 a. ;
 b. 可能发生异常的语句;
 c. ;
}catch(Exception e){
 d;
}finally{
 e;
}
```

当没有发生异常时的执行流程:a , b , c , e;

**异常捕获catch** 

```java
@SuppressWarnings("all")
public class ExceptionCatchTet {
    public static void main(String[]args){
        FileInputStream input = null;
        try{
            System.out.println("1");
//        创建字节输入流对象,构造器中传入文件地址对文件进行关联操作
//            当程序发生异常后直奔catch进行处理
            input = new FileInputStream("D:\\a.txt");
            System.out.println("2");
        }catch(FileNotFoundException e){
            System.out.println("3");
//            打印站区的跟踪信息
            e.printStackTrace();
            System.out.println("4");
        }finally{
            System.out.println("5");
            try{
                System.out.println("6");
//        关闭流,释放资源
//                因为上面的字节输入流触发了FileNotFoundException异常所以这里关闭对象得到的是一个Null也会报错
               if(input != null){
                  input.close();
               }
                System.out.println("7");
//这里可能抛出的异常有IOException异常和NullPointerException异常为了让它抛异常走catch我们选择Exception最大的异常
            }catch(IOException e1){
                System.out.println("8");
                e1.printStackTrace();
                System.out.println("9");
            }catch(NullPointerException e2){
                System.out.println("10");
                e2.printStackTrace();
                System.out.println("11");
            }
        }
//        程序员版的经典语录
        /**
         * 别问我 默默是谁  静静是谁
         * 问了我也不会告诉你!
         */
        System.out.println(getFormatLogString("世界上最真情的相依就是你在try我在catch,无论是发什么脾气,我都默默承受静静处理!到那时再来期待我们的finlly!",34,5));
        System.out.println(getFormatLogString("别问我 默默是谁  静静是谁",35,5)+"\n"+getFormatLogString("问了我也不会告诉你!",31,5)+"\n"+getFormatLogString("                ---程序员经典语录",36,0));
    }
    private static String getFormatLogString(String content, int colour, int type) {
        boolean hasType = type != 1 && type != 3 && type != 4;
        if (hasType) {
            return String.format("\033[%dm%s\033[0m", colour, content);
        } else {
            return String.format("\033[%d;%dm%s\033[0m", colour, type, content);
        }
    }
}
-----------------------------------RUN-----------------------------------
1
3
4
5
6
10
11
java.io.FileNotFoundException: D:\a.txt (The system cannot find the file specified)
	at java.base/java.io.FileInputStream.open0(Native Method)
	at java.base/java.io.FileInputStream.open(FileInputStream.java:219)
	at java.base/java.io.FileInputStream.<init>(FileInputStream.java:157)
	at java.base/java.io.FileInputStream.<init>(FileInputStream.java:112)
	at com.dkx.ecpetion.ExceptionCatchTet.main(ExceptionCatchTet.java:16)
java.lang.NullPointerException
	at com.dkx.ecpetion.ExceptionCatchTet.main(ExceptionCatchTet.java:29)
世界上最真情的相依就是你在try我在catch,无论是发什么脾气,我都默默承受静静处理!到那时再来期待我们的finlly!
别问我 默默是谁  静静是谁
问了我也不会告诉你!
                ---程序员经典语录

Process finished with exit code 0
```

**最终执行finally** 

```java
@SuppressWarnings("all")
public class ExceptionFinlly {
    public static int test(){
        try{
//            创建一个数组
            int[] arr = new int[5];
//            让其访问数组角标越界产生ArrayIndexOutOfBoundsException异常
            int i = arr[5];
            return 0;
//            catch到程序的异常
        }catch(ArrayIndexOutOfBoundsException e){
            e.printStackTrace();
//            因为程序报错进入到catch执行return 1
            return 1;
//            但是finally是最终执行结果catch刚要走就被finally叫住了说:今天是我去站岗
        }finally{
//            最终执行return 2返回值为2
            return 2;
        }
    }
    public static void main(String[]args){
        try{
            System.out.println(1/0);
        }catch(ArithmeticException e){
            e.printStackTrace();
            String str = null;
            int i = str.length();
        }finally{
            System.out.println("无论是否发生异常都来执行 finally - finally runing...");
            System.out.println(test());
        }
//        异常被catch后就会继续执行此语句,异常没有被catch那么就会终止JVM不执行此语句,finally始终被执行
        System.out.println("Over!!!");
    }
}
----------------------------RUN----------------------------
java.lang.ArithmeticException: / by zero
	at com.dkx.ecpetion.ExceptionFinlly.main(ExceptionFinlly.java:22)
java.lang.ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 5
	at com.dkx.ecpetion.ExceptionFinlly.test(ExceptionFinlly.java:10)
	at com.dkx.ecpetion.ExceptionFinlly.main(ExceptionFinlly.java:29)
Exception in thread "main" java.lang.NullPointerException
	at com.dkx.ecpetion.ExceptionFinlly.main(ExceptionFinlly.java:26)
无论是否发生异常都来执行 finally - finally runing...
2
```

- 在实际开发中我们要尽量的避免异常,如果实在是避免不了了,那就捕获,如果依然处理不了这个异常,那么我们可以进行异常的抛出,<font style="color:red">不建在main方法中抛出异常这样会给JVM处理,会增大JVM的压力</font> 

###### 1.6 异常的抛出:palm_tree: 

- 基本概念

在某些特殊情况下有些异常不能处理或者不便处理时,就可以将该异常转移给该方法的调用者,这种方法就叫异常的抛出,当方法执行时出现异常,则底层生成一个异常类对象抛出,此时异常代码后续的代码就不再执行

```java
@SuppressWarnings("all")
public class ExceptionThrowsTest {
    public static void show() throws IOException {
        FileInputStream input = new FileInputStream("D:\\a.txt");
        System.out.println("抛出异常后是否继续向下执行");
        input.close();
    }
//    不建议在main方法中抛出异常
    public static void main(String[]args){
        try{
            show();
        }catch(IOException e){
            e.printStackTrace();
        }
    }
}
---------------------------RUN---------------------------
java.io.FileNotFoundException: D:\a.txt (The system cannot find the file specified)
	at java.base/java.io.FileInputStream.open0(Native Method)
	at java.base/java.io.FileInputStream.open(FileInputStream.java:219)
	at java.base/java.io.FileInputStream.<init>(FileInputStream.java:157)
	at java.base/java.io.FileInputStream.<init>(FileInputStream.java:112)
	at com.dkx.ecpetion.ExceptionThrowsTest.show(ExceptionThrowsTest.java:9)
	at com.dkx.ecpetion.ExceptionThrowsTest.main(ExceptionThrowsTest.java:16)
```

- 语法格式

[访问权限]  [返回值类型]  [方法名称] (形参列表) throws [异常类型1],[异常类型2], [...]  {方法体};

如:

```java
public void show() throws IOException {}
```

- 方法重写的原则

a. 要求方法名相同,参数列表相同以及返回值类型相同,从jdk1.5开始支持返回子类类型

b. 要求方法的访问权限不能变小,可以相同或者变大

c. 要求方法不能抛出更大的异常

- 注意:

子类重写的方法不能抛出更大的异常(孩子不能比爹坏),不能抛出平级不一样的异常

可以抛出更小的异常,也可以不抛出异常

- 经验分享:
    - 若父类中被重写的方法没有抛出异常时,则子类中重写的方法只能进行异常的捕获处理(就是不能抛出异常)
       - 若一个方法内部又以递进方式分别调用了好几个其它方法,则建议这些方法内可以使用抛出的方法处理到最后一层进行捕获方式处理

```java
@SuppressWarnings("all")
public class SubExceptionMethodTest extends ExceptionMethodTest{
//    @Override
    //public void show()throws IOException {}//子类中可以重写父类方法可以抛出与父类中相同的异常
//    @Override       FileNotFoundException是IOException的子类
    //public void show()throws FileNotFoundException {}//子类重写父类方法可以抛出更小的异常
//    @Override
//    public void show(){}//父类方法抛出异常子类重写父类方法可以不跑出异常
//    @Override
//    public void show()throws ClassNotLoadedException {}//子类不能抛出重写父类方法平级但是不同的异常
//    @Override
//    public void show()throws Exception{}//子类重写父类方法不能抛出比父类更大的异常
}
```


```java
@SuppressWarnings("all")
public class ExceptionThrowsTest {
    public static void show() throws IOException {
        FileInputStream input = new FileInputStream("D:\\a.txt");
        System.out.println("抛出异常后是否继续向下执行");
        input.close();
    }
//    递进式调用将异常抛给调用者进行处理
    public static void test1() throws IOException {
        show();
    }
    public static void test2() throws IOException {
        test1();
    }
    public static void test3() throws IOException {
        test2();
    }
//    不建议在main方法中抛出异常
    public static void main(String[]args){
        try{
            show();
        }catch(IOException e){
            e.printStackTrace();
        }
        System.out.println("------------------完美的分割线----------------");
        try{
            test3();
        }catch(IOException e){
            e.printStackTrace();
        }
    }
}
-------------------------------RUN-------------------------------
java.io.FileNotFoundException: D:\a.txt (The system cannot find the file specified)
	at java.base/java.io.FileInputStream.open0(Native Method)
	at java.base/java.io.FileInputStream.open(FileInputStream.java:219)
	at java.base/java.io.FileInputStream.<init>(FileInputStream.java:157)
	at java.base/java.io.FileInputStream.<init>(FileInputStream.java:112)
	at com.dkx.ecpetion.ExceptionThrowsTest.show(ExceptionThrowsTest.java:9)
	at com.dkx.ecpetion.ExceptionThrowsTest.main(ExceptionThrowsTest.java:26)
java.io.FileNotFoundException: D:\a.txt (The system cannot find the file specified)
	at java.base/java.io.FileInputStream.open0(Native Method)
	at java.base/java.io.FileInputStream.open(FileInputStream.java:219)
	at java.base/java.io.FileInputStream.<init>(FileInputStream.java:157)
	at java.base/java.io.FileInputStream.<init>(FileInputStream.java:112)
	at com.dkx.ecpetion.ExceptionThrowsTest.show(ExceptionThrowsTest.java:9)
	at com.dkx.ecpetion.ExceptionThrowsTest.test1(ExceptionThrowsTest.java:15)
	at com.dkx.ecpetion.ExceptionThrowsTest.test2(ExceptionThrowsTest.java:18)
	at com.dkx.ecpetion.ExceptionThrowsTest.test3(ExceptionThrowsTest.java:21)
	at com.dkx.ecpetion.ExceptionThrowsTest.main(ExceptionThrowsTest.java:32)
------------------完美的分割线----------------

Process finished with exit code 0
```



# File类:deciduous_tree: 

- java.io.File类主要用于描述文件或目录路径的抽象表示信息,可以获取文件或目录的特征信息,如:大小等

| 方法声明                            | 功能概述                                     |
|-------------------------------------|----------------------------------------------|
| File(String pathname)               | 根据参数指定的路径名来构造对象               |
| File(String parent,String child)    | 根据参数指定的父路径和子路径信息构造对象     |
| File(File parent,String child)      | 根据参数指定的父抽象路径和子路径信息构造对象 |
| boolean exist()                     | 测试此抽象路径名表示的文件或目录是否存在     |
| String getName()                    | 用于获取文件的名称                           |
| long length()                       | 返回由此抽象路径名表示的文件的长度           |
| long lastModified()                 | 用于获取文件的最后一次修改时间               |
| String getAbsolutePath()            | 用于获取绝对路径信息                         |
| boolean delete()                    | 用于删除文件,当删除目录时要求时空目录        |
| boolean createNewFile()             | 用于创建不存在的空文件,如果存在则不创建                       |
| boolean mkdir()                     | 用于创建目录                                 |
| boolean mkdirs()                    | 用于创建多级目录                             |
| File[] listFiles()                  | 获取该目录下的所有内容                       |
| boolean isFile()                    | 判断是否为文件                               |
| boolean isDirectory()               | 判断是否为目录                               |
| File[] listFiles(FileFilter filter) | 获取目录下满足筛选器的所有内容               |

代码案例:

```java
@SuppressWarnings("all")
public class FileTest {
    public static void main(String[]args){
        File f = new File("D:\\a.txt");
        if(f.exists()){
            System.out.println("文件名称为:"+f.getName());//File对象中的文件不存在获取名称为其名称
            System.out.println("文件 ["+f.getName()+"] "+"内容长度:"+f.length());//获取路径长度
            Date d = new Date(f.lastModified());
            SimpleDateFormat s = new SimpleDateFormat("yyyy-MM-ddEH:mm:ss");
            System.out.println("文件最后修改时间:"+s.format(d));
            System.out.println("获取绝对路径:"+f.getAbsolutePath());
            System.out.println("删除文件 ["+f.getName()+"] "+(f.delete() ? "删除成功":"删除失败"));
        }else{
            try{
                System.out.println("文件不存在创建文件 ["+f.getName()+"] "+(f.createNewFile() ? "创建成功":"创建失败"));
            }catch(IOException e){
                e.printStackTrace();
            }
        }
        System.out.println("-------------------创建多级目录------------------");
        File f1 = new File("D:\\abc国际目录\\def公共目录\\ghi私有目录");
//        判断指定文件或目录是否存在
        if(f1.exists()){
            deleteMutiple(new File("D:\\abc国际目录"));
        }else {
//            判断如果指定文件或目录不存在则创建多级目录
            if(!f1.exists()){
                System.out.println("创建多级目录 ["+f1.getName()+"] "+(f1.mkdirs() ? "创建成功":"创建失败"));
//                如果指定文件或目录存在则创建单级目录
            }else{
                System.out.println("创建 ["+f1.getName()+"] 目录:"+(f1.mkdir() ? "创建成功":"创建失败"));
            }
        }
        System.out.println("-------------------遍历文件或目录内容------------------");
//        创建File类对象,构造器传入文件或目录位置
        File f2 = new File("D:\\abc");
//        获取File对象的文件或目录位置中的所子目录或子文件内容返回到File[]数组中
        File[] flist = f2.listFiles();
//        遍历File[]数组
        for(File i:flist){
//            获取文件或目录的名称
//            文件或目录的判断都要使用到getName我们将其提炼出来使用减少代码的冗余度
            String name = i.getName();
//            判断是否为文件
            if(i.isFile()){
                System.out.println("文件名称为: ["+i+"]");
            }
//            判断是否为目录
            if(i.isDirectory()){
                System.out.println("目录名称为: ["+i+"]");
            }
        }
        System.out.println("-------------------遍历文件或目录过滤后的内容------------------");
//        遍历File对象指定目录或文件中按条件进行过滤的子文件或者子目录
//        匿名内部类格式: 接口/父类类型 引用变量名 = new 接口/父类类型(){方法重写}
//        创建File对象指定操作的文件或目录的位置
        File file1 = new File("D:\\abc");
        FileFilter filefilter = new FileFilter(){
            @Override
            public boolean accept(File filename){
//                返回条件为以名称的结尾为.avi的文件则为true,否则为false
                return filename.getName().endsWith(".avi");
            }
        };
//        使用Lambda表达式的格式
//        格式: (参数列表) -> {返回值 方法体;}; 省略格式:如果方法体中只有一条语句则可以省略返回值  方法体  分号(三个必须一起省略)
        FileFilter filefilter1 = (File filename) -> {return filename.getName().endsWith(".avi");};
//        使用listFiles来接收File对象所操作的文件目录位置的所有子文件或子目录并保存到File[]数组中
        File[] f3 = file1.listFiles(filefilter1);
        for(File i:f3){
            System.out.println(i);
        }
        System.out.println("-------------------遍历指定位置的子目录和子文件------------------");
        showMuptiple(new File("D:\\abc"));

    }
//    删除多级目录方法
    public static void deleteMutiple(File file){
//        通过listFiles将获取到的File对象的位置的所有子文件和子目录返回到File[]数组中
        File[] f = file.listFiles();
//        遍历File[]数组
        for(File i:f){
//            判断是否为文件
            if(i.isFile()){
                System.out.println("删除文件:"+i.getName()+(i.delete() ? "删除"+i.getName()+"成功":"删除"+i.getName()+"失败"));
            }
//            判断是否为目录
            if(i.isDirectory()){
//                判断i的也就是当前位置的所有子文件或子目录是否为null不为null说明还有东西
                if(i.listFiles() != null){
//                    将当前的状态返回给本方法继续执行
                    deleteMutiple(i);
                }
            }
        }
//        删除最后的空目录
        System.out.println("删除文件:"+file.getName()+(file.delete() ? "删除"+file.getName()+"成功":"删除"+file.getName()+"失败"));
    }
    public static void showMuptiple(File file){
//        通过listFiles获取File对象传入的指定位置的所有子目录或子文件返回到File[]数组中
        File[] f = file.listFiles();
//        遍历File[]数组
        for(File i:f){
//            获取当前i文件或目录的名称
            String name = i.getName();
//            判断是否为文件
            if(i.isFile()){
//                如果为文件当前的i为当前文件的名称
                System.out.println("文件名称为:"+name);
            }
//            判断是否为目录
            if(i.isDirectory()){
//                如果为目录那么当前i的name就是当前目录的name
                System.out.println("目录名称为:"+name);
//                判断当前i的所有子文件或子目录是否为Null
                if(i.listFiles() != null){
//                    如果不为null说明还有东西将状态返回给本方法再次进行操作
                    showMuptiple(i);
                }
            }
        }
    }
}
---------------------------------------RUN---------------------------------------
文件名称为:a.txt
文件 [a.txt] 内容长度:0
文件最后修改时间:2023-03-15Wed9:44:09
获取绝对路径:D:\a.txt
删除文件 [a.txt] 删除成功
-------------------创建多级目录------------------
删除文件:ghi私有目录删除ghi私有目录成功
删除文件:def公共目录删除def公共目录成功
删除文件:abc国际目录删除abc国际目录成功
-------------------遍历文件或目录内容------------------
目录名称为: [D:\abc\a]
文件名称为: [D:\abc\a.txt]
文件名称为: [D:\abc\ac.txt]
文件名称为: [D:\abc\ar.txt]
文件名称为: [D:\abc\New Text Document.avi]
-------------------遍历文件或目录过滤后的内容------------------
D:\abc\New Text Document.avi
-------------------遍历指定位置的子目录和子文件------------------
目录名称为:a
目录名称为:b
目录名称为:c
文件名称为:dr.avi
文件名称为:cr.txt
文件名称为:be.txt
文件名称为:br.txt
文件名称为:a.txt
文件名称为:ac.txt
文件名称为:ar.txt
文件名称为:New Text Document.avi
```



- 案例题目

==遍历指定目录以及子目录中的所有内容并打印出来== 

提示:递归的形式将目录以及子目录全部的内容打印出来

```java
@SuppressWarnings("all")
public class FileTest1 {
    public static void main(String[]args){
        show(new File("D:\\abc"));
    }
    public static void show(File file){
//        通过listFiles获取File对象传入指定位置的所有子文件或子目录返回到File[]数组中
        File[] f = file.listFiles();
//        遍历File[]数组
        for(File i:f){
//          getName方法会重复使用所以将其提炼出来让下面的执行代码使用
            String name = i.getName();
//            判断是否为文件
            if(i.isFile()){
                System.out.println("当前文件名称:"+name);
            }
//            判断是否为目录
            if(i.isDirectory()){
                System.out.println("当前目录名称:"+name);
//                判断当前位置的所有子文件或子目录中是否为null如果不为null则执行
                if(i.listFiles() != null){
//                    调用自身将当前的位置状态返回给自己然后接着删除里面的东西删除
                    show(i);
                }
            }
        }
    }
}
----------------------------------------------RUN----------------------------------------------
当前目录名称:a
当前目录名称:b
当前目录名称:c
当前文件名称:dr.avi
当前文件名称:cr.txt
当前文件名称:be.txt
当前文件名称:br.txt
当前文件名称:a.txt
当前文件名称:ac.txt
当前文件名称:ar.txt
当前文件名称:New Text Document.avi
```

==遍历删除指定目录以及子目录中所有东西== 

提示:递归的形式将目录以及子目录全部删除

```java
@SuppressWarnings("all")
public class FileTest1 {
    public static void main(String[]args){
        show(new File("D:\\abc国际目录"));
    }
    public static void show(File file){
//        通过listFiles获取File对象传入指定位置的所有子文件或子目录返回到File[]数组中
        File[] f = file.listFiles();
//        遍历File[]数组
        for(File i:f){
//            判断是否为文件
            if(i.isFile()){
                System.out.println("删除当前文件:"+i.getName()+(i.delete() ? "删除"+i.getName()+"成功":"删除"+i.getName()+"失败"));
            }
//            判断是否为目录
            if(i.isDirectory()){
//                判断当前位置的所有子文件或子目录中是否为null如果不为null则执行
                if(i.listFiles() != null){
//                    调用自身将当前的位置状态返回给自己然后接着删除里面的东西删除
                    show(i);
                }
            }
        }
//        每次将目录中所有东西都删除后及不是文件又不为null此时就执行该操作 删除最后的这层空目录然后接着下次的操作
        System.out.println("删除目录:"+file.getName()+(file.delete() ? "删除"+file.getName()+"成功":"删除"+file.getName()+"失败"));
    }
}
--------------------------------------------------RUN--------------------------------------------------
删除目录:ghi私有目录删除ghi私有目录成功
删除目录:def公共目录删除def公共目录成功
删除目录:abc国际目录删除abc国际目录成功
```

# IO流:deciduous_tree: 

- IO就是Input和Output的简写,也就是输入和输出的含义

- IO流就是指读写数据时像流水一样从一端流到另外一端,因此得名为"流"

## 基本分类:evergreen_tree: 

- <font style="color:red">按照读写数据的基本单位不同</font>,分为 <font style="color:red">**字节流**</font>和<font style="color:red">**字符流**</font>.

其中<font style="color:red">**字节流**</font>主要指以字节为单位进行数据读写的流,可以读写任意类型的文件(字节流是万能的可以读取:文本文件,图片,声音文件,视频文件等).

其中<font style="color:red">**字符流**</font>主要指以字符(GBK:  <font style="color:red">2个字节</font>,UTF-8:  <font style="color:red">3个字节</font>  <font style="color:red">字符流一次读取 2 个字节</font>),为单位进行数据读写的流,这种流为了方便读取普通文本文件而存在的,不能读取:图片,声音,视频等文件,只能读取文本文件(能用 .txt 开打的文件,没有特殊格式,文件名后缀不一定是 .txt,比如 .java 文件也属于纯文件),word文件不属于纯文本

### IO流的继承体系结构图:palm_tree: 

![img](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081148894.png)

使用字符流读写文本文件

代码案例:

```java
//        1.创建字符输入流并与指定文件进行关联
        FileReader reader = null;
//        2.创建字符输出流并指定要写出的位置
        FileWriter writer = null;
        try{
            reader = new FileReader("D:\\a.txt");
            writer = new FileWriter("D:\\b.txt");
            int i = 0;
            char[] ch = new char[1024];
            while((i = reader.read(ch)) != -1){
                writer.write(ch,0,i);
            }
            System.out.println("文件Copy完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
           if(null != reader){
               try{
                       reader.close();
                  }catch(IOException e){
                   e.printStackTrace();
                  }
            }
            if(null != writer){
                try{
                       writer.close();
                }catch(IOException e){
                   e.printStackTrace();
               }
       }
 }
-----------------------------RUN-----------------------------
文件Copy完毕
关闭流,释放资源!
```

![image_2023-03-15-16-02-33](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081148193.png)

使用字符流读写图片

同样的代码更改关联文件为图片

![image_2023-03-15-16-05-55](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081148986.png)

读写后的变化

- 文件大小发生了改变

- 文件损坏

![image_2023-03-15-16-07-46](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081148506.png)

**字符流只能读写文本文件** 

- <font style="color:red">按照读写的方向不同</font>,分为 <font style="color:red">**输入流**</font>和<font style="color:red">**输出流**</font> (<font style="color:red">站在程序的角度讲</font>).

其中<font style="color:red">**输入流**</font>主要指从文件中读取数据内容输入到程序中,也就是读文件.

其中<font style="color:red">**输出流**</font>主要指将程序中的数据内容输出到文件中,也就是写文件.

- <font style="color:red">按照流的角色不同</font>,分为<font style="color:red">**节点流**</font>和<font style="color:red">**处理流**</font>.

其中<font style="color:red">**节点流**</font>主要指直接和输入输出源对接的流.

其中<font style="color:red">**处理流**</font>主要指需要建立在节点流的基础之上的流.

![image_2023-03-15-11-06-28](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081409232.png)

## 体系结构:evergreen_tree: 

| 分类       | 字节输入流           | 字节输出流            | 字符输入流        | 字符输出流         |
|------------|----------------------|-----------------------|-------------------|--------------------|
| 抽象基类   | InputStream          | OutputStream          | Reader            | Writer             |
| 访问文件   | FileInputStream      | FileOutputStream      | FileReader        | FileWriter         |
| 访问数组   | ByteArrayInputStream | ByteArrayOutputStream | CharArrayReader   | CharArrayWriter    |
| 访问管道   | PipedInputStream     | PipedOutputStream     | PipedReader       | PipedWriter        |
| 访问字符串 | ---                  | ---                   | StringReader      | StringWriter       |
| 缓冲流     | BufferedInputStream  | BufferedOutputStream  | BufferedReader    | BufferedWirter     |
| 转换流     | ---                  | ---                   | InputStreamReader | OutputStreamWriter |
| 对象流     | ObjectInputStream    | ObjectOutputStream    | ---               | ---                |
| 过滤       | FilterInputStream    | FilterOutputStream    | FilterReader      | FilterWriter       |
| 打印流     | ---                  | PrintOutputStream     | ---               | PrintWriter        |
| 推回输入流 | PushbackInputStream  | ---                   | PushbackReader    | ---                |
| 特殊流     | DataInputStream      | DataOutputStream      | ---               | ---                |

## 相关流的详解:evergreen_tree: 

### FileWriter类(重点):palm_tree: 

**1. 基本概念** 

- java.io.FileWriter类主要用于将文本内容写入到文本文件

**2. 常用的方法** 

| 方法声明                                   | 功能介绍                                                   |
|--------------------------------------------|------------------------------------------------------------|
| FileWriter(String fileName)                | 根据参数指定的文件名构造对象                               |
| FileWriter(String fileName,boolean append) | 以追加的方式根据参数指定的文件名来构造对象                 |
| void writer(int c)                         | 写入单个字符                                               |
| void writer(char[] cbuf,int off,int len)   | 将指定字符数组中从偏移量off开始的len个字符写入此文件输出流 |
| void writer(char[] cbuf)                   | 将cbuf.length个字符从指定字符数组写入此文件输出流中        |
| void flush()                               | 刷新流,将数据刷出                                          |
| void close()                               | 关闭流,并释放有关的资源                                    |

**close与flush的区别** 

![mage_2023-03-15-11-51-57](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081409803.png)

```java
@SuppressWarnings("all")
public class FileWriterTest {
    public static void main(String[]args){
        FileWriter filewriter = null;
        try{
//            若指定位置的文件不存在输出流则会创建一个新的空文件
            filewriter = new FileWriter("D:\\a.txt");//如果文件存在而再次执行则清空文件内容再写出
//            如果构造器中赋值写出模式为false或者不写都为第二次执行时如果文件存在则清空文件内容然后在将字节数据写出到文件中,所以无论执行多少此都是原有的那个数据
//            filewriter = new FileWriter("D:\\a.txt",false);//如果文件存在而再次执行则清空文件内容再写出
//            如果构造器中赋值写出模式为true则为追加模式,第二次执行不会清空存在的文件的内容,而是在文件内容的后面继续追加内容
//             filewriter = new FileWriter("D:\\a.txt",true);//如果文件存在而再次执行,将会将内容追加到后面,不会清空原有的数据
//            String str = "Hello,world";
            char[] ch = {'小','日','子','-','刘','桑'};
            filewriter.write(ch,0,ch.length);
//            存储反序的ch的中的话
            char[] ch1 = new char[ch.length];
            int c = 0;
            for(int ir = 0,end = ch.length - 1;ir <= end;end --){
                ch1[c] = ch[end];
                c ++;
            }
//            每次执行write当再次执行下一个write的时候并不会清空文件中的内容而是接着将数据写出到文件中
            filewriter.write(",");
            filewriter.write(ch1);
            filewriter.write('v');
//            写出结果为:小日子-刘桑小日子-刘桑v
            System.out.println("写出完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
// 避免异常:当Filewriter对象发生报错时FileWriter我们赋值为null再调用close方法会报NullPointerException
          if(null != filewriter){
//            close方法本身需要抛出IOException我们不可能在main方法抛出给JVM处理,所以我们try了这个异常
                  try{
                          filewriter.close();
                          System.out.println("流比关闭,释放资源");
      //          捕获IO操作时发生的异常
                  }catch(IOException e){
                      e.printStackTrace();
                  }
             }else{
                  System.out.println("NullPointerException,关闭流失败");
             }
         }
    }
    /**
     * 总结:
     *  1.当文件已经存在且有内容我们想再次执行的话内容就会被清除而后再写出所以看到的还是原始的数据,如果希望可以追加内容需要在实例对象构造器中传入true使用追加模式
     *  2.当执行多个write的时候并不会发生前者值会后者值覆盖结果为后write的情况,而是会将执行过的write都写出到文件中(类似追加效果)
     *  3.我们在处理close的时候需要注意我们catch File类的时候为了扩大使用范围选择了FileWriter = null如果File类
     *  在IO操作时发生了报错那么就会赋值为null所以我们需要先避免一下null会触发的NullPointerException异常然后在进行close方法本有的IOException异常
     */
}
--------------------------------------RUN--------------------------------------
写出完毕
流比关闭,释放资源
```

![image_2023-03-15-14-23-25](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081409855.png)

### FileReader类(重点):palm_tree: 

**1. 基本概念** 

- java.io.FileReader类主要用于从文本文件读取文本数据内容

**2. 常用方法** 

| 方法声明                                    | 功能介绍                                                     |
| ------------------------------------------- | ------------------------------------------------------------ |
| FileReader(String fileName)                 | 根据参数指定的文件名构造对象                                 |
| int read()                                  | 读取单个字符(8位)的数据并返回,如果没有字节则返回-1表示读取到末尾 |
| int read(char[] cbuf,int offset,int length) | 从输入流中将最多len个字符的数据读入一个字符数组中,返回读取到的字符个数,返回-1表示读取到末尾 |
| int read(char[] cbuf)                       | 从此输入流中将最多cbuf.length个字符的数据读入字符数组中,返回读取到的字符个数,返回-1表示读取到末尾 |
| void close()                                | 关闭流对象并释放有关的资源                                   |

```java
@SuppressWarnings("all")
public class FileReaderTest {
    public static void main(String[]args){
        FileReader reader = null;
        try{
            reader = new FileReader("D:\\a.txt");
//            读取范围字符数据到字符数组中
            char[] arr = new char[14];
//            从上往下读取第一个读取到全部字符数据后,后面的read将读取到的都是空的
            reader.read(arr,0,12);
            for(int i = 0,end = arr.length - 1;i <= end;i ++){
                System.out.print("读取到的字符: ["+arr[i]+"]  ");
            }
//            不使用字符数组读取文件的字符数据
           //代表实际读取大小
            int i = 0;
            while((i = reader.read()) != -1){
                System.out.print("读取到的字符(not array): ["+(char) i+"]  ");
            }
//            读取整个字符数组, 使用第一个read读取的数组算是共享数组了,主要看这个共享数组中是否读取到了数据
            reader.read(arr);
            for(int i1 = 0,end = arr.length - 1;i1 < end;i1 ++){
                System.out.print("读取到的字符(array): ["+arr[i1]+"]  ");
            }
            System.out.println();
            System.out.println("读取完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
            if(null != reader){
            	try{
                     reader.close();
               }catch(IOException e){
                   e.printStackTrace();
               }
             }
        }
    }
}
-------------------------------------------RUN-------------------------------------------
读取到的字符: [小]  读取到的字符: [日]  读取到的字符: [子]  读取到的字符: [-]  读取到的字符: [刘]  读取到的字符: [桑]  读取到的字符: [,]  读取到的字符: [桑]  读取到的字符: [刘]  读取到的字符: [-]  读取到的字符: [子]  读取到的字符: [日]  读取到的字符: [ ]  读取到的字符: [ ]  
读取到的字符(not array): [小]  读取到的字符(not array): [v]  
读取到的字符(array): [小]  读取到的字符(array): [日]  读取到的字符(array): [子]  读取到的字符(array): [-]  读取到的字符(array): [刘]  读取到的字符(array): [桑]  读取到的字符(array): [,]  读取到的字符(array): [桑]  读取到的字符(array): [刘]  读取到的字符(array): [-]  读取到的字符(array): [子]  读取到的字符(array): [日]  读取到的字符(array): [ ]  
读取完毕
```

第一个读取到数据后保存到arr数组中读取时剩下2个字符没读到,到while循环判断读取只能读取仅剩的字符再到最后一个也是使用着arr数组读取到的和第一个一样

### FileOutputStream类(重点):palm_tree: 

**1. 基本概念** 

- java.io.FileOutputStream类主要用于将图像数据之类的原始字节流写入到输出流中

**2. 常用的方法** 

| 方法声明                                     | 功能介绍                                                   |
| -------------------------------------------- | ---------------------------------------------------------- |
| FileOutputStream(String name)                | 根据参数指定的文件名来构造对象                             |
| FileOutputStream(String name,boolean append) | 以追加的方式根据参数指定的文件名来构造对象                 |
| void write(int b)                            | 将指定字节写入此文件输出流                                 |
| void write(byte[] b,int off,len len)         | 将指定字节数组中从偏移量off开始的len个字节写入此文件输出流 |
| void write(byte[] b)                         | 将b.length个字节从指定字节数组写入此文件输出流中           |
| void flush()                                 | 刷新此输出流并强制写出任何缓冲的输出字节                   |
| void close()                                 | 关闭流对象并释放有关的资源                                 |

### FileInputSteream类(重点):palm_tree: 

**1. 基本概念** 

- java.io.FileInputStream类主要用于从输入流中以字节流的方式读取图像数据等

**2. 常用方法** 

| 方法声明                           | 功能介绍                                                                                        |
|------------------------------------|-------------------------------------------------------------------------------------------------|
| FileInputStream(String name)       | 根据参数指定的文件路径名来构造对象                                                              |
| int read()                         | 从输入流中读取单个字节(8位)的数据并返回,返回-1表示读取到末尾                                    |
| int read(byte[] b,int off,int len) | 从此输入流中将最多len个字节的数据读入字节数组中,返回读取的字节个数,返回-1表示读取到末尾         |
| int read(byte[] b)                 | 从此输入流中将最多b.length个字节的数据读入字节数组中,返回读取到的字节个数,返回-1表示读取到末尾 |
| void close()                       | 关闭流对象,并释放有关的资源                                                                     |
| int available()                    | 获取输入流所关联文件的大小                                                                      |

- 案例题目:

编程实现两个文件之间的拷贝功能

```java
@SuppressWarnings("all")
public class FileByteCopyTest {
    public static void main(String[]args){
//        1.创建字节输入流并指定文件位置对文件进行关联
        FileInputStream input = null;
//        2.创建字节输出流并指定要写出文件的位置
        FileOutputStream out = null;
        try{
//            拷贝文件
//            input = new FileInputStream("D:\\a.txt");
//            out = new FileOutputStream("D:\\b.txt");
//            拷贝图片
            input = new FileInputStream("D:\\PictureTest\\maggie.PNG");
            out = new FileOutputStream("D:\\PictureTest\\maggie1.PNG");
//            定义读取字节数据临时存放的变量,代表实际读取大小
            int i = 0;
//            定义缓冲区,将读取到的字节数据存放到缓冲区中,然后一次性写出
            byte[] by = new byte[1024];
//            循环判断读取字节数据,从输入流中读取单个字节(8位),转化为0~255之间的整数并返回,如果没有字节返回-1
            while((i = input.read(by)) != -1){
//                从输入流读取到缓冲区的字节数据写出到输出流中
                out.write(by,0,i);
            }
            System.out.println("读取完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
//            关闭流,释放资源
                if(null != input){
                   try{
                    input.close();
                 }catch(IOException e){
                     e.printStackTrace();
            	 }
               }
             if(null != out){
               try{
                    out.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
             }
        }
    }
}
```

![image_2023-03-15-17-47-04](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081409183.png)

打开图片

![image_2023-03-15-17-47-24](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081409313.png)

图片可以正常使用

#### 文件拷贝的缺陷:tanabata_tree: 

**拷贝视频** 

```java
@SuppressWarnings("all")
public class FileByteTest1 {
    public static void main(String[]args){
//        创建字节输入流对象扩大使用范围将其提炼出来赋值为null
        FileInputStream fis = null;
//        创建字节输出流对象扩大使用范围将其提炼出来赋值为Null
        FileOutputStream fos = null;
        try{
//            实例化字节输入流对象构造器传入指定文件位置对其进行关联
            fis = new FileInputStream("D:\\videTest\\9.3.1TCP通信简介+Socket.mp4");
//            实例化字节输出流对象构造器传入要写出的文件位置
            fos = new FileOutputStream("D:\\abc\\are.mp4");
//            定义临时存放字节数据变量,代表实际读取大小
            int i = 0;
//            循环判断读写文件
//            read:读取单个字节(8位)转换为0~255之间的整数并返回,如果没有字节可读则返回-1
            System.out.println("拷贝中...");
            try{
                Thread.sleep(1000);
            }catch(Exception e){}
            int ir = 0;
            int filesize = 0;
            long start = System.currentTimeMillis();
//            方式一:以单个字节为单位进行拷贝,也就是每次只读取一个字节后再写入一个字节
            while((i = fis.read()) != -1){
//                写出数组中从偏移量off开始的i个字节数据
                fos.write(i);
                filesize += fis.available();
            }
            long end = System.currentTimeMillis();
            System.out.println("文件拷贝花费时间:" + (start - end) +"ms");
            System.out.println("文件大小:"+ filesize);
            System.out.println("读写完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
//            避免一下null,因为当实例对象发生意外的话fis就会为Null,因此会触发NullPointerException
            if(null != fis){
//                使用try{}catch捕获close自身携带过来的异常IOException
                try{
                    fis.close();
                    System.out.println("input :关闭流释放资源");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
//            一定要分开处理这两个异常因为同在try中第一个执行了发生异常就不会执行第二个,会发生不必要的弊端
            if(null != fos){
                try{
                    fos.close();
                    System.out.println("output :关闭流释放资源");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
----------------------------------------RUN----------------------------------------
拷贝中...
文件拷贝花费时间:-170505ms
文件大小:515711089
读写完毕
input :关闭流释放资源
output :关闭流释放资源
```

可以看到非常的慢,出奇的慢

![st-1678934200688-2](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081409079.gif)

- 原因:并没有使用数组缓冲区或者Buffered缓冲流,因此一个单位字节一个单位的读写,就好比去超时买鸡蛋,共需要买50个,可我却一个一个的买,买一个后跑回家然后再跑去买一个鸡蛋,大家都觉得我有病,程序亦是如此

![art](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410036.png)

-  而且这个视频足足的13,928,866 bytes 这不逆天了

#### 优化:tanabata_tree: 

-  使用数组缓冲区

```java
@SuppressWarnings("all")
public class FileByteTest1 {
    public static void main(String[]args){
//        创建字节输入流对象扩大使用范围将其提炼出来赋值为null
        FileInputStream fis = null;
//        创建字节输出流对象扩大使用范围将其提炼出来赋值为Null
        FileOutputStream fos = null;
        try{
//            实例化字节输入流对象构造器传入指定文件位置对其进行关联
            fis = new FileInputStream("D:\\videTest\\9.3.1TCP通信简介+Socket.mp4");
//            实例化字节输出流对象构造器传入要写出的文件位置
            fos = new FileOutputStream("D:\\abc\\are.mp4");
//            定义临时存放字节数据变量,代表实际读取大小
            int i = 0;
//            循环判断读写文件
//            read:读取单个字节(8位)转换为0~255之间的整数并返回,如果没有字节可读则返回-1
            System.out.println("拷贝中...");
            try{
                Thread.sleep(1000);
            }catch(Exception e){}
            int ir = 0;
            int filelen = fis.available();
            byte[] by = new byte[filelen];
            long start = System.currentTimeMillis();
//            方式一:以单个字节为单位进行拷贝,也就是每次只读取一个字节后再写入一个字节
            while((i = fis.read(by)) != -1){
//                写出数组中从偏移量off开始的i个字节数据
                fos.write(by,0,i);
            }
            long end = System.currentTimeMillis();
            System.out.println("文件拷贝花费时间:" + (start - end) +"ms");
            System.out.println("文件大小:"+ filelen);
            System.out.println("读写完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
//            避免一下null,因为当实例对象发生意外的话fis就会为Null,因此会触发NullPointerException
            if(null != fis){
//                使用try{}catch捕获close自身携带过来的异常IOException
                try{
                    fis.close();
                    System.out.println("input :关闭流释放资源");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
//            一定要分开处理这两个异常因为同在try中第一个执行了发生异常就不会执行第二个,会发生不必要的弊端
            if(null != fos){
                try{
                    fos.close();
                    System.out.println("output :关闭流释放资源");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
---------------------------------------RUN---------------------------------------
拷贝中...
文件拷贝花费时间:-37ms
文件大小:13928866
读写完毕
input :关闭流释放资源
output :关闭流释放资源
```

![jidan](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410943.png)

-  <font style="color:red">**缺点: 若文件过大时,无法申请一个和文件大小一样的缓冲区,真是物理内存不够,比如买鸡蛋,别人都说我买鸡蛋厉害让我代捎,村里所有人都让我捎的话一共的买5万个鸡蛋此时总不能一次买5万个直接带回去因为扛不动**</font> 

#### 再优化:tanabata_tree: 

- 第一个一个鸡蛋买太慢,第二个一次买5万个鸡蛋这谁扛得动,第三个就相当于是分期

- 分期:分开买,分成若干次去买,最少的时间买最多的鸡蛋

```java
@SuppressWarnings("all")
public class FileByteTest1 {
    public static void main(String[]args){
//        创建字节输入流对象扩大使用范围将其提炼出来赋值为null
        FileInputStream fis = null;
//        创建字节输出流对象扩大使用范围将其提炼出来赋值为Null
        FileOutputStream fos = null;
        try{
//            实例化字节输入流对象构造器传入指定文件位置对其进行关联
            fis = new FileInputStream("D:\\videTest\\9.3.1TCP通信简介+Socket.mp4");
//            实例化字节输出流对象构造器传入要写出的文件位置
            fos = new FileOutputStream("D:\\abc\\are.mp4");
//            定义临时存放字节数据变量,代表实际读取大小
            int i = 0;
//            循环判断读写文件
//            read:读取单个字节(8位)转换为0~255之间的整数并返回,如果没有字节可读则返回-1
            System.out.println("拷贝中...");
            try{
                Thread.sleep(1000);
            }catch(Exception e){}
//            创建字节数组缓冲区,每次读取适当的字节:1024个,然后写出,然后再读取1024字节然后再写出
            byte[] by = new byte[1024];
            long start = System.currentTimeMillis();
//            方式一:以单个字节为单位进行拷贝,也就是每次只读取一个字节后再写入一个字节
            while((i = fis.read(by)) != -1){
                /**如果直接写出1024个字节,那么可能会因为倍数导致丢失1个或者多个字节因此文件损坏,比如
                 * 读取最后的时候剩下1025个字节,读取1024后下次1024个读取了1个剩下的全是空气
                 * 如果不是使用的循环读写可能会导致丢失字节,因为一次只读写1024个不够的就不读写了
                 */
//                fos.write(by);
//                写出数组中从偏移量off开始的i个字节数据
                /**
                 * 当数据满1024个字节时,一次读取1024个字节,然后一次写出1024个字节
                 * 当数据不满1024时,一次读取剩下的n个字节,然后一次写出仅剩的n个字节
                 * 不会写出没意义的数据
                 */
                fos.write(by,0,i);
            }
            long end = System.currentTimeMillis();
            System.out.println("文件拷贝花费时间:" + (start - end) +"ms");
            System.out.println("读写完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
//            避免一下null,因为当实例对象发生意外的话fis就会为Null,因此会触发NullPointerException
            if(null != fis){
//                使用try{}catch捕获close自身携带过来的异常IOException
                try{
                    fis.close();
                    System.out.println("input :关闭流释放资源");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
//            一定要分开处理这两个异常因为同在try中第一个执行了发生异常就不会执行第二个,会发生不必要的弊端
            if(null != fos){
                try{
                    fos.close();
                    System.out.println("output :关闭流释放资源");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
---------------------------------------------RUN---------------------------------------------
拷贝中...
文件拷贝花费时间:-67ms
读写完毕
input :关闭流释放资源
output :关闭流释放资源
```

### BufferedOutputStream类(重点):palm_tree: 

**1. 基本概念** 

- java.io.BufferedOutputStream类主要用于描述缓冲输出流,此时不用为写入的每个字节调用底层系统.

**2. 常用方法** 

| 方法声明                                        | 功能介绍                                 |
|-------------------------------------------------|------------------------------------------|
| BufferedOutputStream(OutputStream out)          | 根据参数指定的引用来构造对象             |
| BufferedOutputStream(OutputStream out,int size) | 根据参数指定的引用和缓冲区大小来构造对象 |
| void write(int b)                               | 写入单个字节                             |
| void write(byte[] b,int off,int len)            | 写入字节数组中的一部分数据               |
| void write(byte[] b)                            | 写入参数指定的整个字节数组               |
| void flush()                                    | 刷新流                                   |
| void close()                                    | 关闭流对象并释放有关的资源               |

### BufferedInputStream类(重点):palm_tree: 

**1. 基本概念** 

- java.io.BufferedInputStream类主要用于描述缓冲输入流

**2. 常用的方法** 

| 方法声明                                     | 功能介绍                                                     |
| -------------------------------------------- | ------------------------------------------------------------ |
| BufferedInputStream(InputStream in)          | 根据参数指定的引用构造对象                                   |
| BufferedInputStream(InputStream in,int size) | 根据参数指定的引用和缓冲区大小构造对象                       |
| int read()                                   | 读取单个字节(8位)                                            |
| int read(byte[] b,int off,int len)           | 读取len个字节读入输出流中,返回读取到的字节个数,没有字节读取返回-1 |
| int read(byte[] b)                           | 读取b.length个字节                                           |
| void close()                                 | 关闭流对象,释放有关的资源                                    |

### BufferedWriter类(重点):palm_tree: 

**1. 基本概念** 

- java.io.BufferedWriter类主要用于写入单个字符,字符数组以及字符串到输出流中

**2. 常用的方法** 

| 方法声明                                | 功能介绍                                           |
|-----------------------------------------|----------------------------------------------------|
| BufferedWriter(Writer out)              | 根据参数指定的引用来构造对象                       |
| BufferedWriter(Writer out,int sz)       | 根据参数指定的引用和缓冲区大小来构造对象           |
| void write(int c)                       | 写入单个字节(8位)到输出流中                        |
| void write(char[] cbuf,int off,int len) | 将字符数组cbuf中下标off开始的len个字符写入输出流中 |
| void write(char[] cbuf)                 | 将字符串数组cbuf中所有的内容写入输出流中           |
| void write(String s,int off,int  len)   | 将参数s中下标从off开始的len个字符写入输出流中      |
| void write(String str)                  | 将参数指定的字符串内容写入输出流中                 |
| void newLine()                          | 用于写入行分隔符到输出流中                         |
| void flush()                            | 刷新流                                             |
| void close()                            | 关闭流对象,释放有关的资源                          |
### BufferedReader类(重点):palm_tree: 

**1. 基本概念** 

- java.io.BufferedReader类用于从输入流中读取单个字符,字符数组以及字符串

**2. 常用的方法** 

| 方法声明                              | 功能介绍                                                                                                     |
| ------------------------------------- | ------------------------------------------------------------                                                 |
| BufferedReader(Reader in)             | 根据参数指定的引用来构造对象                                                                                 |
| BufferedReader(Reader in,int sz)      | 根据参数指定的引用和缓冲区大小来构造对象                                                                     |
| int read()                            | 从输入流读取单个字符(bgk: 2bytes,utf-8: 3bytes),读取到末尾返回-1,否则返回实际读取到的字符内容                |
| int read(char[] cbuf,int off,int len) | 从输入流中读取len个字符放入数组cbuf中下标从off开始的位置上,若读取到末尾则返回-1,否则返回实际读取到的字符个数 |
| int read(char[] cbuf)                 | 从输入流中读满整个数组cbuf                                                                                   |
| String readLine()                     | 读取一行字符串并返回,返回Null表示读取到末尾                                                                  |
| void close()                          | 关闭流,释放有关的资源                                                                                        |
```java
@SuppressWarnings("all")
public class FileBufferedCharTest {
    public static void main(String[]args){
//        扩大变量使用范围将其提炼出来暂时赋值为null
        BufferedReader reader = null;
//        扩大变量使用范围将其提炼出来暂时赋值为null
        BufferedWriter writer = null;
        try{
//            创建字符缓冲流对象构造器中指定文件位置关联进行操作
            reader = new BufferedReader(new FileReader("D:\\videTest\\document.txt"));
//            创建字符缓冲流对象构造器中指定文件写出的位置
            writer = new BufferedWriter(new FileWriter("D:\\abc\\ar.txt"));
//            定义临时的存放字符数据的变量,代表实际读取到的数据
            String s = null;
//            循环判断如果位null则 停止循环
//            readLine一次读取一行的字符
            while((s = reader.readLine()) != null){
//                写出字符数据,写出实际读取到的字符数据
                writer.write(s);
                writer.newLine();//换行符 /r/n : 占用2个字节(bytes) /r:13 , /n:10
            }
            System.out.println("读写完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
//            1.避免异常  2.捕获close自身携带的IOException
            if(null != reader){
                try{
                    reader.close();
                    System.out.println("reader: 已关闭流");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
            if(null != writer){
                try{
                    writer.close();
                    System.out.println("writer: 已关闭流");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
-----------------------------RUN-----------------------------
读写完毕
reader: 已关闭流
writer: 已关闭流
```

![image_2023-03-16-18-38-45](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410258.png)

### PrintStream类:palm_tree: 

**1. 基本概念** 

- java.io.PrintStream类主要用于更加方便的打印各种数据内容

**2. 常用的方法** 

| 方法声明                      | 功能介绍                           |
|-------------------------------|------------------------------------|
| PrintStream(OutputStream out) | 根据参数指定的引用来构造对象       |
| void print(String s)          | 用于将参数指定的字符串内容打印出来 |
| void println(String x)        | 用于打印字符串后并终止该行         |
| void flush()                  | 刷新流                             |
| void close()                  | 用于关闭输出流并释放有关的资源     |

### PrintWriter类:palm_tree: 

**1. 基本概念** 

- java.io.printWriter类主要用于将对象的格式化形式打印到文本输出流

**2. 常用的方法** 

| 方法声明                | 功能介绍                       |
|-------------------------|--------------------------------|
| PrintWriter(Writer out) | 根据参数指定的引用来构造对象   |
| void print(String s)    | 将参数指定的字符串内容打印出来 |
| void println(String x)  | 打印字符串后并终止该行         |
| void flush()            | 刷新流                         |
| viod close()            | 关闭流对象并释放有关的资源     |

- 案例题目

不断地提示用户输入要发送的内容,发送的内容是"bye"则聊天结束,否则将用户输入的内容写入到指定的文件中

要求:使用BufferedReader类来读取键盘的输入 System.in代表键盘输入

要求:使用PrintStream类负责将数据写入文件中

```java
@SuppressWarnings("all")
public class PrintChatTest {
    public static void main(String[]args) {
//        扩大变量使用范围将其提炼出来并先赋值为null
        BufferedReader reader = null;
//        扩大变量使用范围将其提炼出来并先赋值为null
        PrintStream print = null;
//        判定轮回该谁发送内容
        boolean f = true;
        try {
//            通过变量实例化字符缓冲输入流对象构造器中传入新建的字符输入转换流并在构造器中传入System.in键盘输入
            reader = new BufferedReader(new InputStreamReader(System.in));
//            通过变量实例化打印流对象构造器中新建字节输出流并指定文件写出的位置
            print = new PrintStream(new FileOutputStream("D:\\chatGPT.txt"));
            System.out.println("start !");
            while (true) {
//                实例化对象每次执行创建一个新的时间来代表一个人发送消息的当前时间为多少
                Date date = new Date();
                SimpleDateFormat simple = new SimpleDateFormat("yyyy-MM-ddH:mm:ss");
                String sdate = simple.format(date);
                System.out.println("轮到 ["+(f ? "张三:":"李四:")+"] 输入要发送的聊天内容");
                String s = reader.readLine();
//                console输入bye表示退出程序
                if ("bye".equals(s)) {
                    System.out.println("end !");
                    break;
                }
                print.println(sdate + (f ? " 张三说 : ":"李四说 : ")+s);
//                轮回来赋值Boolean来表示两个不同的人
                f = !f;
            }
            print.println();
            print.println();
        } catch(IOException e){
            e.printStackTrace();
        }finally{
//            1.避免异常  2.捕获close自身带来的异常
            if(null != reader){
                try{
                    reader.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
            if (null != print) {
                print.close();
            }
        }
    }
}
----------------------------------------------RUN----------------------------------------------
start !
轮到 [张三:] 输入要发送的聊天内容
1
轮到 [李四:] 输入要发送的聊天内容
one
轮到 [张三:] 输入要发送的聊天内容
2
轮到 [李四:] 输入要发送的聊天内容
two
轮到 [张三:] 输入要发送的聊天内容
bye
end !
```

![arwe](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410141.png)

### 转换流:palm_tree: 

![awer](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410333.png)

#### OutputStreamWriter类:tanabata_tree: 

**1. 基本概念** 

- java.io.OutputStreamWriter类主要用于实现从字符流到字节流的转换

**2. 常用的方法** 

| 方法声明                                                | 功能介绍                         |
|---------------------------------------------------------|----------------------------------|
| OutputStreamWriter(OutputStream out)                    | 根据参数指定的引用来构造对象     |
| OutputStreamWriter(OutputStream out,String charsetName) | 根据参数指定的引用和编码构造对象 |
| void write(String str)                                  | 将参数指定的字符串写入           |
| void flush()                                            | 刷新流                           |
| viod close()                                            | 用于关闭输出流并释放有关的资源   |

#### InputStreamReader类:tanabata_tree: 

**1. 基本概念** 

- java.io.InputStreamReader类主要用于实现从字节流到字符流的转换

**2. 常用的方法** 

| 方法声明                                             | 功能介绍                           |
|------------------------------------------------------|------------------------------------|
| InputStreamReader(InputStream in)                    | 根据参数指定的引用来构造对象       |
| InputStreamReader(InputStream in,String charsetName) | 根据参数指定的引用和编码来构造对象 |
| int read(char[] cbuf)                                | 读取字符数据到参数指定的数组       |
| void close()                                         | 用于关闭输出流并释放有关的资源     |

### 字符编码:palm_tree: 

**1. 编码表的由来** 

- 计算机只能识别二进制数据,早期就是电信号,为了方便计算机可以识别各个国家的文字,就需要将各个国家的文字采用数字编号的方式进行描述并建立对应的关系表,该表就叫做编码表

- 字符编码顾名思义就是给字符指定编号,计算机的底层只能识别1和0的二进制序列(高低电平信号)

![image_2023-03-17-09-45-33](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410434.png)

**2. 常见的编码表** 

- ASCII: 美国标准信息交换码,使用一个字节的低7位二进制进行表示
   - ASCII码是一种7位编码,但是它在存放时<font style="color:red">必须占全一个字节即8位</font>,最高位为0,其余7为表示ASCII码

- ISO8859-1: 拉丁码表,欧洲码表,使用一个字节的8位二进制进行表示

- GB2312: 中国的文字编码表,最多使用两个字节16位二进制位进行表示

- GBK: 中国的中文编码表升级,融合了更多的中文文字符号,最多使用两个字节16位二进制位表示

- Unicode: 国际标准码,融合了目前人类使用的所有字符,为每个字符分配唯一的字符码,所有的文字都用两个字节16位二进制位来表示

- <font style="color:red">**GBK与GB2312**</font>编码的解析方式

![image_2023-03-17-10-04-20](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410846.png)

**3. 编码的发展** 

- 面向传输的众多UTF(UCS Transfer Format)标准出现了,UTF-8就是每次8个位传输数据,而UTF-16就是每次16个位,这是为传输而设计的编码并使编码无国界,这样就可以显示全世界上所有文化的字符了

- Unicode只是定义了一个庞大的,全球通用的字符集,并为了每个字符规定了唯一确定的编号,具体存储成什么样的字节流,取决于字符编码方案,推荐的Unicode编码是UTF-8和UTF-16

- UTF-8: 变长的编码方式 , 可用1~4个字节来表示一个字符

![adjio](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410198.png)

### DataOutputStream类(了解):palm_tree: 

**1. 基本概念** 

- java.io.DataOutputStream类主要用于以适当的方式将基本数据类型写入输出流中

**翻译过来就是:** <font style="color:green"><u>针对于前面将的8种基本数据类型的变量的数据进行写入操作的</u></font> 

**2. 常用的方法**  

| 方法声明                           | 功能介绍                                                     |
| ---------------------------------- | ------------------------------------------------------------ |
| DataOutputStream(OutputStream out) | 根据参数指定的引用构造对象OutputStream类是个抽象类,实参需要传递子类对象 |
| void writeInt(int v)               | 用于将参数指定的整数一次性写入输出流,优先写入高字节<br> <font style="color:red">int类型在内存空间中占4个字节,从高到底依次写入</font> |
| void close()                       | 用于关闭文件输出流并释放有关的资源                           |

```java
@SuppressWarnings("all")
public class DataOutputStremTest {
    public static void main(String[]args){
//        1.创建数据输出流对象并指定文件写出的位置
        DataOutputStream data = null;
        try{
            data = new DataOutputStream(new FileOutputStream("D:\\c.txt"));
            int number = 99;
//            优先写入高位字节,那么优先写入前面三组的八个零
//            99: 0000 0000 0000 0000 0000 0000 0110 0011
            data.writeInt(number);
            System.out.println("写出数据成功");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
//            1.避免异常  2.捕获close自身携带的异常
            if(null != data){
                try{
                    data.close();
                    System.out.println("关闭流");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
--------------------------------------------------RUN--------------------------------------------------
写出数据成功
关闭流
--------------------------------------------------c.txt--------------------------------------------------
   c
```

### DataInputStream类(了解):palm_tree: 

**1. 基本概念** 

- java.io.DataInputStream类主要用于从输入流中读取基本数据类型的数据

**翻译过来就是:** <font style="color:green"><u>针对于前面将的8种基本数据类型的变量的数据进行读取操作的</u></font> 

**2. 常用的方法** 

| 方法声明                        | 功能介绍                                                               |
|---------------------------------|------------------------------------------------------------------------|
| DataInputStream(InputStream in) | 根据参数指定的引用来构造对象,InpuStream类是抽象类,实参需要传递子类对象 |
| int readInt()                   | 用于从输入流中一次性读取一个整数数据并返回                             |
| void close()                    | 用于关闭文件输出流并释放有关的资源                                     |

```java
@SuppressWarnings("all")
public class DataInputStreamTest {
    public static void main(String[]args){
//        定义数据输入流扩大变量的使用范围将其提炼出来并赋值为null
        DataInputStream in = null;
        try{
//            通过数据输入流变量实例化对象构造器新建字节输入流并指定文件位置进行关联
            in = new DataInputStream(new FileInputStream("D:\\c.txt"));//99
//            读取输入流实际读取的字节数据
            int i = in.readInt();
            System.out.println("读取到的整数 数据是: ["+i+"]");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
//            1.避免异常  2.捕获close自身携带的异常
            if(null != in){
                try{
//                    关闭流,释放资源
                    in.close();
                    System.out.println("流已关闭");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
----------------------------------RUN----------------------------------
读取到的整数 数据是: [99]
流已关闭
```

**注意事项:** 

在DataOutputStream中如果我们使用写出的方法是write的话写出的字节数据为1个字节,我们使用readInt一次读取4个字节就会发生异常

为什么write写出1个字节和writeInt写出4个字节 , 学过来的人应该都懂为什么

- DataOutputStream

```java
@SuppressWarnings("all")
public class DataOutputStremTest {
    public static void main(String[]args){
//        1.创建数据输出流对象并指定文件写出的位置
        DataOutputStream data = null;
        try{
            data = new DataOutputStream(new FileOutputStream("D:\\c.txt"));
            int number = 99;
//            优先写入高位字节,那么优先写入前面三组的八个零
//            99: 0000 0000 0000 0000 0000 0000 0110 0011
//            data.writeInt(number);//写入4个字节
            data.write(number);//     写入1个字节
            System.out.println("写出数据成功");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
//            1.避免异常  2.捕获close自身携带的异常
            if(null != data){
                try{
                    data.close();
                    System.out.println("关闭流");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
-------------------------------------------------RUN-------------------------------------------------
写出数据成功
关闭流
--------------------------------------------------c.txt--------------------------------------------------
c
```

这里文件中的c前面没有三个空格了说明没有写出前面三个高位的三个字节

- DataInputStream

```java
@SuppressWarnings("all")
public class DataInputStreamTest {
    public static void main(String[]args){
//        定义数据输入流扩大变量的使用范围将其提炼出来并赋值为null
        DataInputStream in = null;
        try{
//            通过数据输入流变量实例化对象构造器新建字节输入流并指定文件位置进行关联
            in = new DataInputStream(new FileInputStream("D:\\c.txt"));//99
//            读取输入流实际读取的字节数据
            int i = in.readInt();
            System.out.println("读取到的整数 数据是: ["+i+"]");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
//            1.避免异常  2.捕获close自身携带的异常
            if(null != in){
                try{
//                    关闭流,释放资源
                    in.close();
                    System.out.println("流已关闭");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
-----------------------RUN-----------------------
java.io.EOFException
	at java.base/java.io.DataInputStream.readInt(DataInputStream.java:397)
	at com.dkx.ecpetion.DataInputStreamTest.main(DataInputStreamTest.java:23)
流已关闭
```

发生报错 EOFException

EOFException表示<font style="color:red">**输入过程中意外的到达文件尾或流尾的信号,导致异常**</font>,其实这个是正常的,只是告诉你,该把使用流的对象都关闭一下

**写出一个字节,我们也可以读取一个字节就不会报错了** 

使用read读取一个字节

### ObjectOutputStream类(重点):palm_tree: 

**1. 基本概念** 

- java.io.ObjectOutputStream类主要用于将一个对象的所有内容整体写入到输出流中

- 只能将支持java.io.Serializable接口的对象写入流中

- 类通过实现java.io.Serializable接口以启用其序列化功能

- 所谓序列化主要指将一个对象需要存储的相关信息有效组织成字节序列的转化过程

**2. 常用的方法** 

| 方法声明                             | 功能介绍                               |
|--------------------------------------|----------------------------------------|
| ObjectOutputStream(OutputStream out) | 根据参数指定的引用来构造对象           |
| void writeObject(Object obj)         | 用于将参数指定的对象整体写入到输出流中 |
| void close()                         | 用于关闭输出流并释放有关的资源         |

```java
@SuppressWarnings("all")
public class ObjectOutputStreamTest {
    public static void main(String[]args){
        //定义对象输出流变量扩大使用范围将其提炼出来并先赋值为null
        ObjectOutputStream objout = null;
        try{
            System.out.println("正在进行对象序列化");
            //通过对象输出流变量实例化对象构造器新建字节输出流并指定文件写出位置
            objout = new ObjectOutputStream(new FileOutputStream("D:\\a.txt"));
            //实例化要序列化的对象构造器传入对象的属性
            User u = new User("刘桑","123","11332233");
            //writeObject将对象的信息写出,如果User对象没有进行序列化的话那么可能会报错NotSerializableException
            objout.writeObject(u);
            System.out.println("序列化完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
            //1.避免异常  2.捕获close自身携带的异常
            if(null != objout){
                try{
                    objout.close();
                    System.out.println("流已经关闭");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
---------------------RUN---------------------
正在进行对象序列化
序列化完毕
流已经关闭
---------------------a.txt---------------------
 sr com.dkx.ecpetion.User呂迼7?? L passwordt Ljava/lang/String;L phoneNumq ~ L userNameq ~ xpt 123t 11332233t 鍒樻
```

乱码但是数据确实写出了

### ObjectInputStream类(重点):palm_tree: 

**1. 基本概念** 

- java.io.ObjectInputStream类主要用于从输入流中一次性将对象整体读取出来

- 所谓反序列化主要指将有效组织的字节序列恢复为一个对象及相关信息的转化过程

**2. 常用的方法** 

| 方法声明                          | 功能介绍                                                                      |
|-----------------------------------|-------------------------------------------------------------------------------|
| ObjectInputStream(InputStream in) | 根据参数指定的引用来构造对象                                                  |
| Object readObject()               | 主要用于从输入流中读取一个对象并返回,无法通过返回值来判断是否读取到文件的末尾 |
| void close()                      | 用于关闭输入流并释放有关的资源                                                |

```java
@SuppressWarnings("all")
public class ObjectInputStreamTest {
    public static void main(String[]args){
        //定义对象输入流变量扩大适用范围将其提炼出来并先赋值为null
        ObjectInputStream objin = null;
        try{
            //通过对象输入流变量实例化对象构造器新建字节输入流并指定文件位置进行绑定
            objin = new ObjectInputStream(new FileInputStream("D:\\a.txt"));
            //readObject读取一个对象并返回,无法判断是否读取到文件末尾
            Object o = objin.readObject();
            System.out.println(o);
        }catch(IOException | ClassNotFoundException e){
            e.printStackTrace();
        }finally{
            //1.避免异常  2.捕获close自身携带的异常
            if(null != objin){
                try{
                    objin.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
---------------------------RUN---------------------------
User{userName='刘桑', password='123', phoneNum='11332233'}
```

**3. 序列化版本号** 

- 序列化机制是通过在运行时判断类的SerialVersionUID来验证版本一致性的,在进行反序列化时,JVM会把传来的字节流中的SerialVersionUID与本地响应实体类的SerialVersionUID进行比较,如果相同就认为是一致的,可以进行反序列化,否则就会出现序列化版本不一致的异常(InvalidCastException)

```java
@SuppressWarnings("all")
//实现序列化接口
public class User implements Serializable {
//设定序列化的版本号
    private static final Long SerialVersionUID = 1L;
    private String userName;//用户名
    private String password;//用户密码
    private String phoneNum;//用户手机号

    public User(String userName, String password, String phoneNum) {
        this.userName = userName;
        this.password = password;
        this.phoneNum = phoneNum;
    }

    public User() {
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPhoneNum() {
        return phoneNum;
    }

    public void setPhoneNum(String phoneNum) {
        this.phoneNum = phoneNum;
    }

    @Override
    public String toString() {
        return "User{" +
                "userName='" + userName + '\'' +
                ", password='" + password + '\'' +
                ", phoneNum='" + phoneNum + '\'' +
                '}';
    }
}
```

**4. transient关键字** 

- transient是java语言的关键字,用来表示一个域不是该对象串行化的一部分,当一个对象被串行化的时候,transient型变量的值不包括在串行化的表示中,然而非transient型的变量是被包括进去的

User实体类

```java
    private transient String phoneNum;//用户手机号 使用transient修饰的变量不参与序列化
```

ObjectOutputStream

```java
@SuppressWarnings("all")
public class User implements Serializable {
    private static final Long SerialVersionUID = 1L;
    private String userName;//用户名
    private String password;//用户密码
    private transient String phoneNum;//用户手机号 使用transient修饰的变量不参与序列化

    public User(String userName, String password, String phoneNum) {
        this.userName = userName;
        this.password = password;
        this.phoneNum = phoneNum;
    }

    public User() {
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPhoneNum() {
        return phoneNum;
    }

    public void setPhoneNum(String phoneNum) {
        this.phoneNum = phoneNum;
    }

    @Override
    public String toString() {
        return "User{" +
                "userName='" + userName + '\'' +
                ", password='" + password + '\'' +
                ", phoneNum='" + phoneNum + '\'' +
                '}';
    }
}
-------------------RUN-------------------
正在进行对象序列化
序列化完毕
流已经关闭
-------------------a.txt-------------------
 sr com.dkx.ecpetion.User?联豩9 L passwordt Ljava/lang/String;L userNameq ~ xpt 123t 鍒樻
```

ObjectInputStream

```java
@SuppressWarnings("all")
public class ObjectInputStreamTest {
    public static void main(String[]args){
        //定义对象输入流变量扩大适用范围将其提炼出来并先赋值为null
        ObjectInputStream objin = null;
        try{
            //通过对象输入流变量实例化对象构造器新建字节输入流并指定文件位置进行绑定
            objin = new ObjectInputStream(new FileInputStream("D:\\a.txt"));
            //readObject读取一个对象并返回,无法判断是否读取到文件末尾
            Object o = objin.readObject();
            System.out.println(o);
        }catch(IOException | ClassNotFoundException e){
            e.printStackTrace();
        }finally{
            //1.避免异常  2.捕获close自身携带的异常
            if(null != objin){
                try{
                    objin.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
-------------------RUN-------------------
User{userName='刘桑', password='123', phoneNum='null'}
```

phoneNum没有参与序列化

**5. 经验的分享** 

- 当希望将多个对象写入文件时,通常建议将多个对象放入一个集合中,然后将集合这个整体看做一个对象写入输出流中,此时只需要调用一次readObject方法就可以将整个集合的数据读取出来,从而避免了用过返回值进行是否达到文件末尾的判断

![image_2023-03-20-10-52-24](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410496.png)
实体类

User

```java
@SuppressWarnings("all")
public class User implements Serializable {
    private static final Long SerialVersionUID = 1L;
    private String userName;//用户名
    private String password;//用户密码
    private transient String phoneNum;//用户手机号 使用transient修饰的变量不参与序列化

    public User(String userName, String password, String phoneNum) {
        this.userName = userName;
        this.password = password;
        this.phoneNum = phoneNum;
    }

    public User() {
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPhoneNum() {
        return phoneNum;
    }

    public void setPhoneNum(String phoneNum) {
        this.phoneNum = phoneNum;
    }

    @Override
    public String toString() {
        return "User{" +
                "userName='" + userName + '\'' +
                ", password='" + password + '\'' +
                ", phoneNum='" + phoneNum + '\'' +
                '}';
    }
}
```

User1

```java
@SuppressWarnings("all")
public class User1 implements Serializable {
    private String userName;
    private String password;
    private String phone;

    public User1() {
    }

    public User1(String userName, String password, String phone) {
        this.userName = userName;
        this.password = password;
        this.phone = phone;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    @Override
    public String toString() {
        return "User1{" +
                "userName='" + userName + '\'' +
                ", password='" + password + '\'' +
                ", phone='" + phone + '\'' +
                '}';
    }
}
```

User2

```java
@SuppressWarnings("all")
public class User2 implements Serializable {
    private String userName;
    private String password;
    private String phone;

    public User2() {
    }

    public User2(String userName, String password, String phone) {
        this.userName = userName;
        this.password = password;
        this.phone = phone;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    @Override
    public String toString() {
        return "User2{" +
                "userName='" + userName + '\'' +
                ", password='" + password + '\'' +
                ", phone='" + phone + '\'' +
                '}';
    }
}
```

ObjectOutputStream

```java
@SuppressWarnings("all")
public class ObjectOutputStreamTest {
    public static void main(String[]args){
        //定义对象输出流变量扩大使用范围将其提炼出来并先赋值为null
        ObjectOutputStream objout = null;
        try{
            System.out.println("正在进行对象序列化");
            //通过对象输出流变量实例化对象构造器新建字节输出流并指定文件写出位置
            objout = new ObjectOutputStream(new FileOutputStream("D:\\a.txt"));
            //实例化要序列化的对象构造器传入对象的属性
            User u = new User("刘桑","123","11332233");
            User1 u1 = new User1("张三","321","1231237");
            User2 u2 = new User2("小日子","456","123176");
            List<Object> list = new ArrayList<>();
            list.add(u);
            list.add(u1);
            list.add(u2);
            //writeObject将对象的信息写出
            objout.writeObject(list);
            System.out.println("序列化完毕");
        }catch(IOException e){
            e.printStackTrace();
        }finally{
            //1.避免异常  2.捕获close自身携带的异常
            if(null != objout){
                try{
                    objout.close();
                    System.out.println("流已经关闭");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
-------------------------------RUN-------------------------------
正在进行对象序列化
序列化完毕
流已经关闭
-------------------------------a.txt-------------------------------
 sr java.util.ArrayListx佉櫱a? I sizexp   w   sr com.dkx.ecpetion.User?联豩9 L passwordt Ljava/lang/String;L userNameq ~ xpt 123t 鍒樻sr com.dkx.ecpetion.User1m^U?c7 L passwordq ~ L phoneq ~ L userNameq ~ xpt 321t 1231237t 寮犱笁sr com.dkx.ecpetion.User2}y扖鰞B L passwordq ~ L phoneq ~ L userNameq ~ xpt 456t 123176t 	灏忔棩瀛恱
```

ObjectInputStream

```java
@SuppressWarnings("all")
public class ObjectInputStreamTest {
    public static void main(String[]args){
        //定义对象输入流变量扩大适用范围将其提炼出来并先赋值为null
        ObjectInputStream objin = null;
        try{
            //通过对象输入流变量实例化对象构造器新建字节输入流并指定文件位置进行绑定
            objin = new ObjectInputStream(new FileInputStream("D:\\a.txt"));
            //readObject读取一个对象并返回,无法判断是否读取到文件末尾
            Object o = objin.readObject();
            System.out.println(o);
        }catch(IOException | ClassNotFoundException e){
            e.printStackTrace();
        }finally{
            //1.避免异常  2.捕获close自身携带的异常
            if(null != objin){
                try{
                    objin.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
-------------------------------RUN-------------------------------
[User{userName='刘桑', password='123', phoneNum='null'}, User1{userName='张三', password='321', phone='1231237'}, User2{userName='小日子', password='456', phone='123176'}]
```

### RandomAccessFile类:palm_tree: 

**1. 基本概念** 

- java.io.RandomAccessFile类主要==支持对随机访问==文件的读写操作

- 以上我们学习的流都是以文件的开头读取或者写出,而且读取一个字节数据后光标就要往后移写一个字节数据后光标往后移

- 我们要进行跳跃性的对文件内容的读写操作的时候我们可以使用RandomAccessFile类

**2. 常用的方法** 

| 方法声明                                  | 功能介绍                                                                                                                                                                    |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| RandomAccessFile(String name,String mode) | 根据参数指定的名称和模式构造对象<br>r:只读方式打开<br>rw:打开以便读和写入<br>rwd:打开以便读取和写入,同步文件内容的更新<br>rws:打开以便读取和写入,同步文件内容和元数据的更新 |
| int read()                                | 读取单个字节(8位)的数据                                                                                                                                                     |
| void seek(long pos)                          | 用于设置从此文件按的开头开始测量的文件指针偏移量                                                                                                                                                                        |
| void write(int b)                                      | 将参数指定的单个字节写入                                                                                                                                                                        |
| void close()                                      | 用于关闭并释放有关的资源                                                                                                                                                                        |

![image_2023-03-20-11-52-32](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081410052.png)

# 多线程:deciduous_tree: 

## 基本概念:evergreen_tree: 

**程序和进程的概念**  

- 程序 - 数据结构 + 算法,主要指存放在硬盘上的可执行文件

- 进程 - 主要指运行在内存中的可执行文件

- 可执行文件老老实实在硬盘里放着我们称为==程序==,如果可执行文件双击打开跑到内存中了那就称为==进程==,<font style="color:red">**进程也就是运行起来的程序**</font> 


- 目前主流的操作系统都支持多线程,为了让操作系统同时可以执行多个任务,但进程是重量级的,也就是新建一个进程会消耗CPU和内存空间等系统资源,因此进程的数量比较局限

**线程的概念** 

- 为了解决上述问题就提出线程的概念,线程就是进程内的程序流,也就是说操作系统部内支持多进程的,而每个进程的内部又是支持多线程的,线程是轻量的,新建线程会共享所在进程的系统资源,因此目前主流的开发都是采用多线程

- 多线程是采用时间片轮转法来保证多个线程的并发执行,所谓并发就是指宏观并行(表面看同时执行)微观串行(底层实际上是一个一个在执行)的机制

## 线程的创建:evergreen_tree: 

**Thread类的概念** 

- java.lang.Thread类代表线程,任何线程对象都是Thread类(子类)的实例

- Thread类是线程的模板,封装了复杂的线程开启等操作,封装了操作系统的差异性

**创建方式** 

- 自定义类继承Thread类并重写run方法,然后创建该类的对象调用start方法

- 自定义类实现Runnable接口并重写run方法,创建该类的对象作为实参来构造Thread类型的对象,然后使用Thread类型的对象调用start方法

**相关的方法** 

| 方法声明                            | 功能介绍                                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| Thread()                            | 使用无参的方式构造对象                                       |
| Thread(String name)                 | 根据参数指定的名称来构造对象                                 |
| Thread(Runnable target)             | 根据参数指定的引用来构造对象,其中Runnable是个接口类型        |
| Thread(Runnable target,String name) | 根据参数指定引用和名称来构造对象                             |
| void run()                          | 若使用Runnable引用构造了线程对象,调用该方法时最终调用接口中的版本<br>若没有使用Runnable引用构造线程对象,调用该方法时则啥也不做 |
| void start()                        | 用于启动线程,Java虚拟机会自动调用该线程的run方法             |
|创建继承Thread类的实体类||

```java
@SuppressWarnings("all")
public class SubThreadRun extends Thread{
    public void run(){
        System.out.println("SubThreadRun run runing...");
        for(int i = 0;i < 20;i ++){
            System.out.println("run method: i =" + (i+1));
        }
    }
}
```

创建测试类来验证调用run与调用start方法的区别

```java
@SuppressWarnings("all")
public class SubThreadRunTest {
    public static void main(String[]args){
        //声明Thread类型的引用执行子类类型的对象
        Thread thread = new SubThreadRun();
        //调用run方法测试,本质上就是相当于对普通成员方法的调用,因此执行流程就是先执行完run方法后再执行main方法
        //从上到下依次执行
        //thread.run();
        //用于启动线程,Java虚拟机会自动调用该线程类中的run方法
        //相当于又启动了一个线程,加上main方法的线程是两个线程
        //一个线程执行SubThreadRun中的for一个线程执行main中的for两个线程(任务同时执行)就产生了交替执行的现象
        thread.start();
        for(int i = 0;i < 20;i ++){
            System.out.println("main method: i=" + (i+1));
        }
    }
}
---------------RUN---------------
SubThreadRun run runing...
run method: i =1
main method: i=1
run method: i =2
main method: i=2
run method: i =3
main method: i=3
run method: i =4
main method: i=4
run method: i =5
main method: i=5
run method: i =6
main method: i=6
run method: i =7
main method: i=7
run method: i =8
main method: i=8
run method: i =9
main method: i=9
run method: i =10
main method: i=10
run method: i =11
main method: i=11
run method: i =12
main method: i=12
run method: i =13
main method: i=13
run method: i =14
run method: i =15
run method: i =16
main method: i=14
run method: i =17
main method: i=15
run method: i =18
main method: i=16
run method: i =19
main method: i=17
run method: i =20
main method: i=18
main method: i=19
main method: i=20
```

SubRunnableImpl

```java
@SuppressWarnings("all")
public class SubRunnableImpl implements Runnable{
    public void run(){
        for(int i = 0;i < 20;i ++){
            System.out.println("run method SubRunnableImpl i ="+ (i+1));
        }
    }
}
```

SubRunnableTest

```java
@SuppressWarnings("all")
public class SubRunnableTest {
    public static void main(String[]args){
        //创建自定义类型对象,父类引用指向子类类型对象SubRunnableImpl为Runnable接口的实现类
        //Runnable接口中没有start方法所以需要实例化Thread对象来调用start方法开启线程
        Runnable runnable = new SubRunnableImpl();
        //实例化Thread对象构造器中传入runnable实例对象
        //实例化Thread用于调用start开启线程
        //若使用Runnable引用构造了线程对象,调用该方法(run)时最终调用接口中的版本
        //有源码可知:经过狗杂走方法的调用之后Thread类中的成员变量target的数值为runnable
        Thread thread = new Thread(runnable);
        /**
         由run方法的源码可知:
         @Override
         public void run() {
            if (target != null) {
                target.run();
            }
         }
         此时target的数值不为null条件成立,执行target.run(),也就是runnable.run()
        */
        thread.start();
        //runnable.srart() Error
        for(int i = 0;i < 20;i ++){
            System.out.println("main method i ="+(i+1));
        }
    }
}
------------------------------RUN------------------------------
run method SubRunnableImpl i =1
main method i =1
run method SubRunnableImpl i =2
main method i =2
run method SubRunnableImpl i =3
main method i =3
run method SubRunnableImpl i =4
main method i =4
run method SubRunnableImpl i =5
main method i =5
run method SubRunnableImpl i =6
main method i =6
run method SubRunnableImpl i =7
main method i =7
run method SubRunnableImpl i =8
main method i =8
run method SubRunnableImpl i =9
main method i =9
run method SubRunnableImpl i =10
main method i =10
run method SubRunnableImpl i =11
main method i =11
run method SubRunnableImpl i =12
main method i =12
run method SubRunnableImpl i =13
main method i =13
run method SubRunnableImpl i =14
main method i =14
run method SubRunnableImpl i =15
main method i =15
run method SubRunnableImpl i =16
main method i =16
run method SubRunnableImpl i =17
main method i =17
run method SubRunnableImpl i =18
main method i =18
run method SubRunnableImpl i =19
main method i =19
run method SubRunnableImpl i =20
main method i =20
```

调用run则相当于调用了普通成员方法,程序为一个线程为主线程当执行完SubThreadRun类中的run方法后再执行main中的for,执行流程为 依次执行的

调用start则相当于又开启了一个线程,程序为两个线程一个线程执行SubThreadRun类中的run方法一个线程执行main中的for达到交替执行的效果,而谁前执行谁后执行没有明确的规定因此是通过操作系统调度算法来决定的

**执行流程** 

- 执行main方法的线程叫做主线程,执行run方法的线程叫做==新线程/子线程== 

- main方法是程序的入口,对于start方法之前的代码,由主线程执行一次,当start方法调用成功后线程的个数由1变成了2个,新启动的线程去执行run方法的代码,主线程继续往下执行,两个线程各自独立运行互不影响

- 当run方法执行完毕后子线程结束,当main方法执行完毕后主线程结束

- 两个线程执行没有明确的先后执行次序,由操作系统调度算法来决定

**方式的比较** 

- 继承Thread类的方式代码简单,但是若该类继承Thread类后则无法继承其它类,而实现Runnable接口的方式代码复杂,但不影响该类继承其它类以及实现其它接口,因此以后的开发中推荐使用第二种方式

## 匿名内部类的方式:evergreen_tree: 

- 使用匿名内部类的方式来创建和启动线程

```java
@SuppressWarnings("all")
public class ThreadJoinTest {
    public static void main(String[]args){
        //使用匿名内部类实例化Thread构造器中新建Runnable接口并指定线程的名称
        Thread thread = new Thread(new Runnable(){
            //重写run
            @Override
            public void run(){
                Thread t = Thread.currentThread();
                for(int i = 0;i < 20;i ++){
                    try{
                        //线程睡眠
                        t.sleep(1000);
                    }catch(InterruptedException e){
                        e.printStackTrace();
                    }
                    System.out.println(t.getName() + " [" + i + "]");
                }
            }
        },"刘桑");
        //启动线程
        thread.start();
        var t = new Thread().currentThread();
        int i = 0;
        for(;;){
            i ++;
            System.out.println(t.getName()+" ["+i+"]");
            try{
                t.sleep(1000);
            }catch(InterruptedException e){
                e.printStackTrace();
            }
            if(i == 10){
                try{
                    //线程插队,让插队的线程执行完毕后在执行其它线程
                    thread.join();
                }catch(InterruptedException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
---------------------------------------------RUN---------------------------------------------
main [8]
刘桑 [7]
main [9]
刘桑 [8]
main [10]
刘桑 [9]
刘桑 [10]
刘桑 [11]
刘桑 [12]
```

## 使用Lambda格式创建多线程:evergreen_tree: 

```java
@SuppressWarnings("all")
public class LambdaThreadTest {
    public static void main(String[]args){
        //使用Lambda格式创建线程
        new Thread(()->{
            Thread t = Thread.currentThread();
            for(int i = 0;i < 20;i ++){
                try{
                    t.sleep(1000);
                }catch(InterruptedException e){
                    e.printStackTrace();
                }
                System.out.println(t.getName()+" ["+i+"]");
            }
            //设置线程名称并启动线程
        },"刘桑").start();
        Thread t = new Thread().currentThread();
        for(int i = 0;i < 20;i ++){
            try{
                t.sleep(1000);
            }catch(InterruptedException e){
                e.printStackTrace();
            }
            System.out.println(t.getName()+" ["+i+"]");
        }
    }
}
---------------------------------------RUN---------------------------------------
main [0]
刘桑 [0]
main [1]
刘桑 [1]
main [2]
刘桑 [2]
main [3]
刘桑 [3]
main [4]
刘桑 [4]
刘桑 [5]
main [5]
```

## 使用类名引用静态方法方式:evergreen_tree: 

```java
@SuppressWarnings("all")
public class ThreadDaemonTest {
    public static void runar(){
        var t = Thread.currentThread();
        //当子线程不是守护线程时,虽然主线程结束了但不影响子线程继续完成它的使命
        //当子线程为守护线程时,主线程一旦结束了,子线程将会随之结束,并不是主线程一结束子线程就得立马结束,会有一段反应期
        //如果子线程先执行了很多次在执行的主线程那么就会发生子线程可以循环到120多次才结束而主线程循环5次
        System.out.println(t.isDaemon() ? "子线程为守护线程":"子线程为非守护线程");//默认不是守护线程
        int i = 0;
        for(;i < 2000;){
            System.out.println(t.getName()+" - ["+(i+1)+"]");
            i++;
        }
    }
    public static void main(String[]args){
        //通过类名引用静态方法创建线程
        var thread = new Thread(ThreadDaemonTest::runar,"刘桑");
        //开启后台线程,需要在调用start启动线程之前将其设置为后台线程否则不好使
        thread.setDaemon(true);
        thread.start();
        var t = new Thread().currentThread();
        int i = 0;
        for(;i < 5;){
            System.out.println("--------------"+t.getName()+" - ["+(i+1)+"]");
            i++;
        }
    }
}
---------------------------------------RUN---------------------------------------
子线程为守护线程
刘桑 - [1]
刘桑 - [2]
刘桑 - [3]
刘桑 - [4]
--------------main - [1]
--------------main - [2]
刘桑 - [5]
--------------main - [3]
刘桑 - [6]
--------------main - [4]
刘桑 - [7]
--------------main - [5]
刘桑 - [8]
刘桑 - [9]
刘桑 - [10]
刘桑 - [11]
刘桑 - [12]
```

## 线程的生命周期:evergreen_tree: 

![ixiancheng2](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081411597.png)

- `new`:新建状态  -  在调用`start()`之前的状态

- `runnable`: 在Java虚拟机中执行的线程处于这个状态
   - 可运行状态,是个复合状态,可分为两类:
      -  1.`ready`: 就绪状态
      -  2.`running`: 该线程正在执行状态
   - <font style="color:red">通过`yield()`,完成放弃CPU资源,使该线程从`running`状态到`ready`然后如果被程序调度器选中,又从`ready`状态转到`runing`状态</font> 

- `blocked`: 线程阻塞状态,被阻塞等待监听器锁定的线程处于这个状态
   - <font style="color:red">阻塞状态:当线程发起阻塞的IO操作,或者申请其它线程独占的资源,线程会转换为`blocked`阻塞状态,处于阻塞状态的线程不占用CPU资源,当阻塞IO操作完成时,或者线程获得了其它资源,线程可以转换为`running`状态</font> 

- `waiting`: 正在等待另一个线程执行特定的动作的线程处于这个状态(<font style="color:red">无限时等待直到另一个线程执行完毕</font>)
   - <font style="color:red">线程执行了`wait()`或`join()`会把线程转换为`waiting`状态,执行了`notify`或加入的线程执行完毕,就会转换为`runnable`状态</font> 

- `timed_waiting`: 正在等待另一个线程执行特定动作达到指定等待时间的线程处于这个状态(<font style="color:red">限时等待如果超时不会等待另一个线程执行完毕</font>)
   - <font style="color:red">线程不会无限等待,如果在指定时间内完成了指定的操作,就会转换为`runnable`状态</font> 

- `terminated`: 已退出/消亡的线程处于此状态
   - <font style="color:red">线程已经退出或者消亡状态</font> 


## 线程的编程和名称:evergreen_tree: 

| 方法声明                      | 功能介绍                            |
|-------------------------------|-------------------------------------|
| long getId()                  | 获取调用对象所表示线程的编号        |
| String getName()              | 获取调用对象所表示线程的名称        |
| void setName(String name)     | 设置/修改线程的名称为参数指定的数值 |
| static Thread currentThread() | 获取当前正在执行线程的引用          |

- 案例题目

自定义类继承Thread类并重写run方法,在run方法中先打印当前线程的编号和名称,然后将线程的名称修改为"zhangfei",后再次打印编号和名称

要求在main方法中也要打印主线程的编号和名称

```java
@SuppressWarnings("all")
public class ThreadNameTest extends Thread{
    //定义有参构造方法
    public ThreadNameTest(String name){
        //使用super关键字将传递的name变量的值传递给父类Thread的有参构造器中,完成给其赋值线程名称
        super(name);
    }
    @Override
    public void run(){
        var t = new Thread().currentThread();
        System.out.println("线程的Id:"+t.getId()+" - "+"线程的名称:"+
                    t.getName());
        System.out.println("----------------after----------------");
        //调用setName修改线程名称
        Thread.currentThread().setName("张飞");
        System.out.println("修改后的线程的Id:"+t.getId()+" - "+"线程的名称:"+
                t.getName());
    }
    public static void main(String[]args){
        //父类引用指向子类对象,构造器传入name赋值
        Thread thread = new ThreadNameTest("刘桑");
        //启动线程
        thread.start();
        //定义当前线程的currentThread实例来对该线程进行设置
        var t1 = new Thread().currentThread();
        System.out.println("线程的Id:"+t1.getId()+" - "+"线程的名称:"+
                    t1.getName());
    }
}
---------------RUN---------------
线程的Id:1 - 线程的名称:main
线程的Id:14 - 线程的名称:刘桑
----------------after----------------
修改后的线程的Id:14 - 线程的名称:张飞
```

```java
@SuppressWarnings("all")
public class RunnableNameTest implements Runnable{
    @Override
    public void run(){
        var t = Thread.currentThread();
        System.out.println("子线程的Id: ["+t.getId()+"] "+"子线程的名称为: ["+t.getName()+"]");
        t.setName("张飞");
        System.out.println("修改后的子线程的Id: ["+t.getId()+"子线程的名称为: ["+t.getName()+"]");
    }
    public static void main(String[]args){
        var runnable = new RunnableNameTest();
        var thread = new Thread(runnable,"刘桑");
        thread.start();
        var t = Thread.currentThread();
        System.out.println("主线程的Id: ["+t.getId()+"] "+"主线程的名称为: ["+t.getName()+"]");
    }
}
---------------RUN---------------
主线程的Id: [1] 主线程的名称为: [main]
子线程的Id: [14] 子线程的名称为: [刘桑]
修改后的子线程的Id: [14子线程的名称为: [张飞]
```

**常用方法** 

| 方法声明                          | 功能介绍                                                                                                                                     |
|-----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| static void yield()               | 当前线程让出处理器(离开Running状态),使当前线程进入Runnable状态等待                                                                           |
| static void sleep(times)          | 使当前线程从Running放弃处理器进入Block状态,休眠times毫秒,再返回到Runnable如果其它线程打断当前线程的Block(sleep),就会发生InterruptedException |
| int getprioriry()                 | 获取线程的优先级                                                                                                                             |
| void setPriority(int newPriority) | 修改线程的优先级<br>优先级越高的线程不一定先执行,但该线程获取到时间片的机会会更多一些                                                        |
| void join()                       | 等待该线程终止                                                                                                                               |
| void join(long millis)            | 等待参数指定的毫秒值                                                                                                                         |
| boolean isDaemon()                              | 用于判断是否为守护线程                                                                                                                                         |
| void setDaemon(boolean on)                              | 用于设置线程为守护线程                                                                                                                                         |
- 案例题目

编程创建两个线程,线程一负责打印1`~`100之间的所有奇数,其中线程而负责打印1`~`100之间的所有的偶数,在main方法启动上述两个线程同时执行等待两个线程终止

通过join实现

```java
//Runanble实现类,打印奇数
@SuppressWarnings("all")
public class ThreadJOTest1 implements Runnable{
    @Override
    public void run(){
        for(int i = 1;i < 100;i += 2){
            System.out.println("---------子线程一中:"+i);
        }
    }
}
//Runnable实现类,打印偶数
@SuppressWarnings("all")
public class ThreadJOTest implements Runnable{
    @Override
    public void run(){
        for(int i = 2;i < 100;i += 2){
            System.out.println("子线程二中:"+i);
        }
    }
}
//测试类
@SuppressWarnings("all")
public class ThreadRunnableJOTest {
    public static void main(String[]args){
        ThreadJOTest t = new ThreadJOTest();
        ThreadJOTest1 t1 = new ThreadJOTest1();
        Thread art = new Thread(t);
        Thread art1 = new Thread(t1);
        art.start();
        art1.start();
        System.out.println("主线程进入等待状态");
        try{
            art.join();
            art1.join();
        }catch(InterruptedException e){
            e.printStackTrace();
        }
    }
}
--------------------------------------RUN--------------------------------------
主线程进入等待状态
子线程二中:2
---------子线程一中:1
子线程二中:4
---------子线程一中:3
子线程二中:6
---------子线程一中:5
子线程二中:8
---------子线程一中:7
子线程二中:10
---------子线程一中:9
子线程二中:12
---------子线程一中:11
子线程二中:14
---------子线程一中:13
子线程二中:16
---------子线程一中:15
子线程二中:18
---------子线程一中:17
子线程二中:20
---------子线程一中:19
子线程二中:22
---------子线程一中:21
子线程二中:24
---------子线程一中:23
子线程二中:26
---------子线程一中:25
子线程二中:28
子线程二中:30
---------子线程一中:27
子线程二中:32
---------子线程一中:29
子线程二中:34
---------子线程一中:31
子线程二中:36
---------子线程一中:33
子线程二中:38
---------子线程一中:35
子线程二中:40
---------子线程一中:37
子线程二中:42
---------子线程一中:39
子线程二中:44
---------子线程一中:41
子线程二中:46
---------子线程一中:43
子线程二中:48
---------子线程一中:45
子线程二中:50
---------子线程一中:47
子线程二中:52
---------子线程一中:49
子线程二中:54
子线程二中:56
---------子线程一中:51
子线程二中:58
---------子线程一中:53
子线程二中:60
---------子线程一中:55
子线程二中:62
---------子线程一中:57
子线程二中:64
---------子线程一中:59
子线程二中:66
---------子线程一中:61
子线程二中:68
子线程二中:70
子线程二中:72
---------子线程一中:63
子线程二中:74
---------子线程一中:65
子线程二中:76
子线程二中:78
---------子线程一中:67
子线程二中:80
---------子线程一中:69
子线程二中:82
子线程二中:84
子线程二中:86
---------子线程一中:71
子线程二中:88
---------子线程一中:73
子线程二中:90
---------子线程一中:75
子线程二中:92
---------子线程一中:77
子线程二中:94
---------子线程一中:79
子线程二中:96
---------子线程一中:81
子线程二中:98
---------子线程一中:83
---------子线程一中:85
---------子线程一中:87
---------子线程一中:89
---------子线程一中:91
---------子线程一中:93
---------子线程一中:95
---------子线程一中:97
---------子线程一中:99
```

通过wait,notify实现

```java
//Thread继承多线程类
@SuppressWarnings("all")
public class ThreadWaitNotifyTest extends Thread{
    private static boolean f = true;
    private static int i = 1;
    public synchronized void f(){
            try{
                while(f != true){
                    this.wait();
                }
            }catch(InterruptedException e){
                e.printStackTrace();
            }
            f = false;
            System.out.println(Thread.currentThread().getName()+" - ["+(i ++) +"]");
            this.notify();
    }
    public synchronized void f1(){
            try{
                while(f != false){
                    this.wait();
                }
            }catch(InterruptedException e){
                e.printStackTrace();
            }
            f = true;
            System.out.println(Thread.currentThread().getName()+" - ["+(i ++) +"]");
            this.notify();
    }
}
//测试类
@SuppressWarnings("all")
public class SubThreadTest {
    public static void main(String[] args) {
        ThreadWaitNotifyTest t = new ThreadWaitNotifyTest();
        var t1 = new Thread().currentThread();
        int i = 0;
        for(;i  < 50;){
            new Thread(t::f,"线程1号").start();
            new Thread(t::f1,"线程2号").start();
            i++;
        }
    }
}
----------------------------------------RUN----------------------------------------
线程1号 - [1]
线程2号 - [2]
线程1号 - [3]
线程2号 - [4]
线程1号 - [5]
线程2号 - [6]
线程1号 - [7]
线程2号 - [8]
线程1号 - [9]
线程2号 - [10]
线程1号 - [11]
线程2号 - [12]
线程1号 - [13]
线程2号 - [14]
线程1号 - [15]
线程2号 - [16]
线程1号 - [17]
线程2号 - [18]
线程1号 - [19]
线程2号 - [20]
线程1号 - [21]
线程2号 - [22]
线程1号 - [23]
线程2号 - [24]
线程1号 - [25]
线程2号 - [26]
线程1号 - [27]
线程2号 - [28]
线程1号 - [29]
线程2号 - [30]
线程1号 - [31]
线程2号 - [32]
线程1号 - [33]
线程2号 - [34]
线程1号 - [35]
线程2号 - [36]
线程1号 - [37]
线程2号 - [38]
线程1号 - [39]
线程2号 - [40]
线程1号 - [41]
线程2号 - [42]
线程1号 - [43]
线程2号 - [44]
线程1号 - [45]
线程2号 - [46]
线程1号 - [47]
线程2号 - [48]
线程1号 - [49]
线程2号 - [50]
线程1号 - [51]
线程2号 - [52]
线程1号 - [53]
线程2号 - [54]
线程1号 - [55]
线程2号 - [56]
线程1号 - [57]
线程2号 - [58]
线程1号 - [59]
线程2号 - [60]
线程1号 - [61]
线程2号 - [62]
线程1号 - [63]
线程2号 - [64]
线程1号 - [65]
线程2号 - [66]
线程1号 - [67]
线程2号 - [68]
线程1号 - [69]
线程2号 - [70]
线程1号 - [71]
线程2号 - [72]
线程1号 - [73]
线程2号 - [74]
线程1号 - [75]
线程2号 - [76]
线程1号 - [77]
线程2号 - [78]
线程1号 - [79]
线程2号 - [80]
线程1号 - [81]
线程2号 - [82]
线程1号 - [83]
线程2号 - [84]
线程1号 - [85]
线程2号 - [86]
线程1号 - [87]
线程2号 - [88]
线程1号 - [89]
线程2号 - [90]
线程1号 - [91]
线程2号 - [92]
线程1号 - [93]
线程2号 - [94]
线程1号 - [95]
线程2号 - [96]
线程1号 - [97]
线程2号 - [98]
线程1号 - [99]
线程2号 - [100]
```

## 线程同步机制(重点):evergreen_tree: 

**基本概念** 

- 当多个线程同时访问一种共享资源时,可能会造成数据的覆盖等不一致性问题,此时就需要对线程之间进行通信和协调,该机制就叫做线程的同步机制

- 多个线程并发读写一个临界资源时会发生线程并发问题

- 异步操作:多线程并发的操作,各自独立运行

- 同步操作:多线程串行的操作,先后执行的顺序

**解决方案** 

- 由程序结果可知:当两个线程同时对一个账户进行取款时,导致最终的账户余额不合理

- 引发原因:线程一执行取款时还没来得及将取款后的余额写入后台,线程二就已经开始取款

- 解决方案:让线程一执行完毕取款操作后,再让线程二执行即可,将线程的并发操作改为串行操作

- 经验分享:在以后开发尽量减少换行操作的范围,从而提高效率

**实现方式** 

- 在java语言种使用synchronized关键字来实现同步/对象锁机制从而保证线程执行的原子性(最小),具体方式如下:

- 使用同步代码块的方式实现部分代码的锁定,格式如下:
  
```java
synchronized (类 类型的引用) {
    编写所有需要锁定的代码;
}
```

- 使用同步方法的方式实现所有代码的锁定

直接使用synchronized关键字来修饰整个方法即可

该方式等价于:

```java
synchronized (this) {整个方法体的代码}
```

其中`synchronized()`中的this必须是同一个对象不然会发生不一致性

**静态方法的锁定** 

- 当我们对一个静态方法加锁,如:

```java
public synchronized static void xxx(){...}
```

- 那么该方法锁的对象是类对象,每个类都有唯一的一个类对象,获取类对象的方式: 类名.class

- 静态方法与非静态方法同时使用了synchronized后它们之间是非互斥关系的

- 原因在于: 静态方法锁的是类对象而非静态方法锁的是当前方法所属对象

<font style="color:red">**代码案例**</font> 

**Thread** 

```java
@SuppressWarnings("all")
public class ThreadDemoTest extends Thread {
    private static int monery;
    //private Demo dm = new Demo();//在主方法中不能实例化两个对象因为每实例一个就是一个对象会创建两个Demo成员变量
    private static Demo dm = new Demo();//使用static修饰成员变量,隶属于类层级,所有对象共享同一个
    public int getMonery() {return monery;}
    public void setMonery(int monery) {this.monery = monery;}
    public ThreadDemoTest() {}
    public ThreadDemoTest(int monery) {this.monery = monery;}
    @Override//必须为同一个对象
    public synchronized void run() {
        //等价于synchronized(this){}
        //synchronized(dm){//锁获取对象必须为同一个对象否则会导致不一致性
        //不写在锁中,因为要直接打印两个线程的名称,写在锁中则会一个一个来
        //主要查看两个线程是否开启
        System.out.println(Thread.currentThread().getName()+"线程已经开启");
            int tem = getMonery();
            if(tem >= 200){
                tem -= 200;
                try{
                    Thread.sleep(2000);
                }catch(InterruptedException e){
                    e.printStackTrace();
                }
                System.out.println("取款成功");
            }else{
                System.out.println("抱歉您的余额不足");
            }
            //将取出钱后的余额状态赋值到当前状态中
            setMonery(tem);
        //}
        //实验静态同步方法时开打
        //test();
    }//相当于synchronized(类名.class),隶属于类层级,所有对象共享同一个
    public synchronized static void test(){
        //等价于
        //synchronized(ThreadDemoTest.class){
            //不写在锁中,因为要直接打印两个线程的名称,写在锁中则会一个一个来
            //主要查看两个线程是否开启
            System.out.println(Thread.currentThread().getName()+"线程已经开启");
            //synchronized(dm){//锁获取对象必须为同一个对象否则会导致不一致性
            int tem = monery;
            if(tem >= 200){
                tem -= 200;
                try{
                    Thread.sleep(2000);
                }catch(InterruptedException e){
                    e.printStackTrace();
                }
                System.out.println("取款成功");
            }else{
                System.out.println("抱歉您的余额不足");
            }
            //将取出钱后的余额状态赋值到当前状态中
            monery  = tem;
        //}
    }
    public static void main(String[] args) {
        //实例化对象,实例一个对象时开打,两个对象将此对象注释掉打开下面两个对象
        ThreadDemoTest tr = new ThreadDemoTest(200);
        //实例化对象实验,仅可锁主的方法 静态方法同步锁 public synchronized static void test(){}
        //ThreadDemoTest t = new ThreadDemoTest(200);
        //ThreadDemoTest t1 = new ThreadDemoTest(200);
        //实例化一个对象实验,仅可锁主的方法,同步方法 public synchronized void run(){}
        Thread t = new Thread(tr);
        //开启线程
        t.start();
        Thread t1 = new Thread(tr);
        //开启线程
        t1.start();
        System.out.println("主线程等待子线程执行中...");
        try{
            //线程插队
            t.join();
            //线程插队
            t1.join();
        }catch(InterruptedException e){
            e.printStackTrace();
        }
        System.out.println("最终余额:"+tr.getMonery());
    }
}
//定义Demo类在测试类中定义为成员变量为实验锁来使用
class Demo{}
--------------------------------------RUN--------------------------------------
主线程等待子线程执行中...
Thread-1线程已经开启
取款成功
Thread-2线程已经开启
抱歉您的余额不足
最终余额:0
```

**Runnable** 

```java
@SuppressWarnings("all")
public class AccountRunnableTest implements Runnable {
    private static int balance;//描述账户的余额,使用静态成员变量,隶属于类层级,所有对象共享同一个
    //private Demo demo = new Demo();//主方法中实例化两个对象时该成员变量为两个使用同步锁会导致不一致性的问题
    private static Demo demo = new Demo();//使用static修饰的成员变量,隶属于类层级,所有对象共享同一个
    @Override
    public void run() {
        synchronized(demo){//demo对象为静态的属于类层级,所有对象共享同一个
            System.out.println(Thread.currentThread().getName()+"线程已经开启");
            //synchronized(new Demo()){//锁不住,因为不是同一个对象
            Scanner sc = new Scanner(System.in);
            int temp = getBalance();
            if (temp >= 200) {
                System.out.println("正在出钞...");
                temp -= 200;
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("钞票取出完毕,您当前的余额为:" + temp + ",请带好您的钞票");
            } else {
                System.out.println("抱歉,您的账户不足200元,您当前的余额为:" + getBalance());
            }
            //将取出钱后的状态赋值到当前状态中
            setBalance(temp);
        }
    }

    public AccountRunnableTest() {}
    public AccountRunnableTest(int balance) {this.balance = balance;}
    public int getBalance() {return balance;}
    public void setBalance(int balance) {this.balance = balance;}

    public static void main(String[] args) {
        //实例化对象
        AccountRunnableTest account1 = new AccountRunnableTest(200);
        AccountRunnableTest account2 = new AccountRunnableTest(200);
        var t = new Thread(account1);
        //开启线程
        t.start();
        var t1 = new Thread(account1);
        //开启线程
        t1.start();
        System.out.println("主线程开始等待...");
        try {
            //线程插队
            t.join();
            t1.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("最终余额为:" + account1.getBalance());
    }
}
//定义Demo类供AccountRunnableTest与ThreadDemoTest定义成员变量
class Demo{}
----------------------------RUN----------------------------
主线程开始等待...
Thread-0线程已经开启
正在出钞...
钞票取出完毕,您当前的余额为:0,请带好您的钞票
Thread-1线程已经开启
抱歉,您的账户不足200元,您当前的余额为:0
最终余额为:0
```

**注意事项:** 

- 使用synchronized保证线程同步应当注意:
    - 多个需要同步的线程在访问同步块时,看到的应该是同一个锁对象引用
    - 在使用同步块时应当尽量减少同步范围以提高并发的执行效率

**线程安全类和不安全类** 

- StringBuffer类是线程安全的类,但StringBuilder类不是线程安全的类

- Vector类和HashTable类是线程安全的类,但ArrayList和HasMap类不是线程安全的类
    - Vector和HashTable两个已经过时了,使用Collections.synchronizedList和Collections.synchronizedMap可以实现将非线程安全的类转换为线程安全的类

- Collections.synchronizedList()和Collections.synchronizedMap()等方法实现安全

**死锁的概念** 

- 线程一执行的代码:

```java
public void run(){
    synchronized(a){//持有对象锁a,等待对象锁b
        synchronized(b){
            编写锁定的代码;
        }
    }
}
```

- 线程二执行的代码:

```java
public void run(){
    synchronized(b){//持有对象锁b,等待对象锁a
        synchronized(a){
            编写锁定的代码;
        }
    }
}
```

- 注意:

在以后的开发中尽量减少同步的资源,减少同步代码块的嵌套结构的使用

## 使用Lock(锁)实现线程同步:evergreen_tree: 

**基本概念** 

- 从java5开始提供了更强大的线程同步机制--使用显式定义的同步锁对象来实现

- java.util.concurrent.locks.Lock接口是控制多个线程对共享资源进行访问的工具

- 该接口的主要实现类是ReentrantLock类,该类拥有与synchronized相同的并发性,在以后的线程安全控制中,经常使用ReentrantLock类显式加锁和释放锁

**常用的方法** 

| 方法声明        | 功能介绍             |
|-----------------|----------------------|
| ReentrantLock() | 使用无参方式构造对象 |
| void lock()     | 获取锁               |
| void unlock()   | 释放锁               |

**与synchronized方式的比较** 

- Lock是显式锁,需要手动实现开启和关闭操作,而synchronized是隐式锁,执行锁定代码后自动释放

- Lock只有同步代码块方式的锁,而synchronized有同步代码块方式和同步方法两种锁

- 使用Lock锁方式时,java虚拟机将花费较少的时间来调度线程,因此性能更好

**Object类常用的方法** 

| 方法声明                | 功能介绍                                                            |
|-------------------------|---------------------------------------------------------------------|
| void wait()             | 用于使得线程进入等待状态,直到其它线程调用notify()或notifyAll()方法  |
| void wait(long timeout) | 用于进入等待状态,直到其它线程调用方法或参数指定的毫秒数已经过去为止 |
| void notify()           | 用于唤醒等待的单个线程                                              |
| void notifyAll()        | 用于唤醒等待的所有线程                                              |

```java
@SuppressWarnings("all")
public class ThreadLockTest extends Thread{
    private static int monery;
    //private Lock lock = new ReentrantLock();必须为同一个对象
    private static Lock lock = new ReentrantLock();//静态成员变量隶属于类层级,所有对象共享同一个
    @Override
    public void run(){
        lock.lock();//将以下的代码锁主
        //打印出启动了的线程
        System.out.println(Thread.currentThread().getName()+"线程已经启动");
        int temp = getMonery();
        if(temp >= 200){
            temp -= 200;
            try{
                Thread.sleep(1000);
            }catch(InterruptedException e){
                e.printStackTrace();
            }
            System.out.println("取款成功");
        }else{
            System.out.println("抱歉您的余额不足");
        }
        //赋值取款后的余额状态
        setMonery(temp);
        lock.unlock();//释放锁
    }
    public ThreadLockTest (int monery) {this.monery = monery;}
    public void setMonery(int monery){this.monery = monery;}
    public int getMonery(){return monery;}
    public static void main(String[]args){
        //实例化对象
        ThreadLockTest t = new ThreadLockTest(200);
        ThreadLockTest t1 = new ThreadLockTest(200);
        //开启线程
        t.start();
        t1.start();
        try{
            //线程插队
            t.join();
            t1.join();
        }catch(InterruptedException e){
            e.printStackTrace();
        }
        System.out.println("最终余额:"+t.getMonery());
    }
}
----------------------------RUN----------------------------
Thread-1线程已经启动
取款成功
Thread-0线程已经启动
抱歉您的余额不足
最终余额:0
```

- 消费者与生产者案例

[查看代码 ](https://pigpigletsgo.github.io/computer-science/java/%E5%9F%BA%E7%A1%80-%E6%A0%B8%E5%BF%83%E7%B1%BB%E5%BA%93/%E6%B6%88%E8%B4%B9%E8%80%85%E4%B8%8E%E7%94%9F%E4%BA%A7%E8%80%85/) 

## 线程池(熟悉):evergreen_tree: 

**实现Callable接口** 

- 从java5开始新增加创建线程的第三种方式为实现java.util.concurrent.Callable接口

- 常用方法如下:

| 方法声明 | 功能介绍       |
|----------|----------------|
| V call() | 计算结果并返回 |

**FutureTask类** 

- java.util.concurrent.FutureTask类用于描述可取消的异步计算,该类提供了Future接口的基本实现,包括启动和取消计算,查询计算是否完成以检索计算结果的方法,也可以用于获取方法调用后的返回结果

- 常用的方法如下:

| 方法声明                         | 功能介绍                             |
|----------------------------------|--------------------------------------|
| FutureTask(Callable<V> callable) | 根据参数指定的引用来创建一个未来任务 |
| V get()                          | 获取call方法计算的结果               |

**线程池的由来** 

- 在服务器编程模型的原理,每一个客户端连接用一个单独的线程为之服务,当与客户端的会话结束时,线程也就结束了,即每来一个客户端连接,服务器端就要创建一个线程

- 如果访问服务器的客户端很多,那么服务器端要不断的创建和销毁线程,这将严重影响服务器的性能

**概念和原理** 

- 线程池的概念:首先创建一些线程,它们的集合称为线程池,当服务器接收到一个客户请求后,就从线程池中取出一个空闲的线程为之服务,服务完后不关闭该线程,而是将该线程还回到线程池中

- 在线程池的编程模式下,任务是提交给整个线程池,而不是直接交给某个线程,线程池在拿到任务后,它就在内部找有无空闲的线程,再把任务交给内部某个空闲的线程,任务是提交给整个线程池,一个线程同时只能执行一个任务,但可以同时向一个线程池提交多个任务

**相关类和方法** 

- 从java5开始提供了线程池的相关类和接口: java.util.concurrent.Executors类和java.util.concurrent.ExecutorService接口

- 其中Executors是个工具类和线程池的工厂类,可以创建并返回不同类型的线程池,常用方法如下:

| 方法声明                                                  | 功能介绍                             |
|-----------------------------------------------------------|--------------------------------------|
| static ExecutorService newCachedThreadPool()              | 创建一个可根据需要创建新线程的线程池 |
| static ExecutorService newFixedThreadPool(int n,Thread s) | 创建一个可重用固定线程数的线程池     |
| static ExecutorService newSingleThreadExecutor()          | 创建一个只有一个线程的线程池         |

- 其中ExecutorService接口是真正的线程池接口,主要实现类是ThreadPoolExecutor,常用方法如下:

| 方法声明                               | 功能介绍                            |
|----------------------------------------|-------------------------------------|
| void execute(Runnable command)         | 执行任务和命令,通常用于执行Runnable |
| <T> Future<T> submit(Callable<T> task) | 执行任务和命令,通常用于执行Callable                                |
| void shutdown()                                   | 启动有序关闭                                |

-  代码案例

```java
@SuppressWarnings("all")
//实现Callable<T>多线程接口
public class ThreadCallableTest implements Callable<Integer> {
    //定义成员变量记录数据的累加和
    private int num = 0;
    //重写Callable接口中的call方法返回值为泛型类型
    @Override
    public Integer call(){
        for(int i = 1;i <= 10000;i ++){
            num += i;
        }
        System.out.println("计算出来的累加和为 - "+num);
        //返回值
        return num;
    }
    public static void main(String[]args){
        //实例化实现多线程接口类对象
        ThreadCallableTest t = new ThreadCallableTest();
        //实例化FutureTask实现类并在构造器中传入实现多线程接口类对象
        FutureTask t1 = new FutureTask(t);
        //实例多线程类构造器传入简介实现了Runnable的实现类,并启动线程
        new Thread(t1).start();
        try {
            //获取返回值
            Object o = t1.get();
            System.out.println(o);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
------------------------------RUN------------------------------
计算出来的累加和为 - 50005000
50005000
```

```java
@SuppressWarnings("all")
public class ThreadPoolTest {
    public static void main(String[]args){
        //创建一个线程池
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        //向线程池中布置任务
        executorService.submit(new ThreadCallableTest());
        //关闭线程池
        executorService.shutdown();
    }
}
------------------------------RUN------------------------------
计算出来的累加和为 - 50005000
50005000
```

# 网络编程:deciduous_tree: 

**网络编程的常识** 

- 目前主流的网络编程通讯软件:微信,QQ,阿里旺旺,陌陌,探探, ...

**七层网络模型** 

- oSI(Open System Interconnect),即开放式系统互联,是ISO(国际标准化组织),组织在1985年研究的网络互联模型

- OSI七层模型和TCP/IP五层模型的划分如下:

![image_2023-03-24-08-22-42](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081411419.png)

- 当发送数据时,需要对发送的内容按照上述七层模型进行层层加包后发送出去

- 当接收数据时,需要对接收的内容按照上述七层模型相反的次序层层拆包并显示出来

**相关的协议** (笔试题)

**协议的概念** 

- 计算机在网络中实现通信就必须有一些约定或者规则,这种约定和规则就叫做通信协议,通信协议可以对速率,传输代码,代码结构,传输控制步骤,出错控制等制定统一的标准

**TCP协议** 

- 传输控制协议(Transmission COntrol Protocol),是一种面向连接的协议,类似于打电话
    - 建立连接 ==> 进行通信 ==> 断开连接
    - 在传输前采用"三次握手"方式

- 三次握手

![image_2023-03-24-09-51-06](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081411276.png)

- 四次挥手

![image_2023-03-24-10-15-36](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081411863.png)

**UDP协议** 

- 用户数据报协议(User Datagram Protocol),是一种非面向连接的协议,类似于写信
    - 在通信的整个过程中不需要保持连接,其实是不需要建立连接
    - 不保证数据传输的可靠性和有序性
    - 是一种全双工的数据报通信方式,每个数据报的大小限制在64K内
    - 发送数据报完毕后无需释放资源,开销小,发送数据的效率比较高,速度快

![image_2023-03-24-10-11-40](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081411128.png)

**IP地址** (重点)

- 192.186.1.1 - 是绝大多数路由器的登录地址,主要配置用户名和密码以及Mac过滤

- IP地址是互联网中的唯一地址标识,本质上是由32位二进制组成的整数,叫做IPv4,当然也有128位二进制组成的整数,叫做IPv6,目前主流还是IPv4

- 日常生活中采用点分十进制表示法来进行IP地址的描述,将每个字节的二进制转化为一个十进制整数,不同的整数之间采用小数点隔开

如:

0x01020304 => 1.2.3.4

- 查看IP地址的方式

windows: dos中输入ipconfig或ipconfig/all命令

unix/linux: Terminal中输入ifconfig或/sbin/ifconfig命令

- 特殊的地址

本地地址(hostAddress): 127.0.0.1 主机名(HostName):localhost

**端口号** (重点)

- IP地址: 可以定位到具体的一台设备

- 端口号: 可以定位到具体某一个进程

- 端口号本质上是16位二进制组成的整数,表示范围是0~65515,其中0~1024之间的端口号通常被系统占用,建议编程从1025开始使用

- 网络编程需要提供:IP地址+端口号 ,组合在一起叫做网络套接字:Socket

### 基于tcp协议的编程模型(重点):evergreen_tree: 

**C/S架构的简介** 

服务器(Server) , 客户端(Client) : 简写就是 S/C

- 在C/S模式下客户向服务器发出服务请求,服务器接收请求后提供服务

- 例如:在一个酒店中,顾客向服务员点菜,服务员把点菜单通知厨师,厨师按点菜单做好菜后让服务员送给客户,这就是一种C/S工作方式,如果把酒店看作一个系统,服务员就是客户端,厨师就是服务器,这种系统分工和协同工作的方式就是C/S的工作方式

- 客户端部分: 为每一个用户所专有的,负责执行前台功能

- 服务器部分: 由多个用户共享的信息与功能,招待后台服务

**编程模型** 

- 服务器

1. 创建ServerSocket类型的对象并提供端口号

2. 等待客户端的连接请求,使用accept()方法

3. 使用输入流进行通信

4. 关闭Socket

- 客户端:

1. 创建Socket类型的对象并提供服务器的IP地址和端口号

2. 使用输入输出流进行通信

3. 关闭Socket

![image_2023-03-24-11-08-19](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081411511.png)

**相关类和方法的解析** 

**1. ServerSocket类** 

- java.net.ServerSocket类主要用于描述服务器套接字信息(大插排)

- 常用的方法如下:

| 方法声明 | 功能介绍 |
|----------|----------|
| ServerSocket(int port)     | 根据参数指定的端口号来构造对象     |
| Socket accept()     | 侦听并接收到此套接字的连接请求     |
| void close()     | 用于关闭套接字     |

**2. Socket类** 

- java.net.Socket类主要用于描述客户端套接字,是两台机器间通信的端点(小插排)

- 常用的方法如下:

| 方法声明                       | 功能介绍                         |
|--------------------------------|----------------------------------|
| Socket(String host,int port)   | 根据指定主机名和端口号来构造对象 |
| InputStream getInputStream()   | 用于获取当前套接字的输入流       |
| OutputStream getOutputStream() | 用于获取当前套接字的输出流       |
| void close()                   | 用于关闭套接字                   |

**3. 注意事项** 

- 客户端Socket与服务器端Socket对应,都包含输入和输出流

- 客户端socket.getInputStream()连接于服务器socket.getOutputStream()

- 客户端的socket.getOutputStream连接于服务器socket.getInputStream()

服务端

```java
@SuppressWarnings("all")
public class ServerSocketTest {
    public static void main(String[]args) {
        Scanner sc = new Scanner(System.in);
        ServerSocket serversocket = null;
        BufferedWriter writer = null;
        Socket socket = null;
        InputStream in = null;
        try{
            serversocket = new ServerSocket(2333);
            System.out.println("等待连接中...");
            socket = serversocket.accept();
            System.out.println("连接成功");

            //------------------write------------------
            writer = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
            System.out.println("请输入您要发送的消息");
            String s = sc.nextLine();
            writer.write(s);

            System.out.println("正在读取数据中...");
            in = socket.getInputStream();
            int i = 0;
            byte[] by = new byte[1024];
            while((i = in.read(by)) != -1){
                System.out.println("读取到的数据为 :"+new String(by,0,i));
            }
        }catch(IOException e){
            e.printStackTrace();
        }finally{
            //1.避免异常  2.捕获自身携带的异常
            if(null != writer){
                try{
                    writer.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }

            if(null != in){
                try{
                    in.close();
                    System.out.println("inputStream流已经关闭");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }

        //关闭套接字
        if(null != socket){
            try{
                socket.close();
                System.out.println("套接字已关闭");
            }catch(IOException e){
                e.printStackTrace();
            }
        }

        if(null != serversocket){
            try{
                serversocket.close();
                System.out.println("服务器套接字已关闭");
            }catch(IOException e){
                e.printStackTrace();
            }
        }
    }
}
```

客户端

```java
@SuppressWarnings("all")
public class ClientSocketTest {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        InetAddress localhost = null;
        Socket socket = null;
        //PrintStream out = null;
        //OutputStream out = null;
        BufferedWriter out = null;
        //读取
        BufferedReader reader = null;
        try {
            localhost = InetAddress.getLocalHost();
            socket = new Socket(localhost, 2333);

            //out = new PrintStream(socket.getOutputStream());
            //out = socket.getOutputStream();
            out = new BufferedWriter(new OutputStreamWriter(socket.getOutputStream()));
            System.out.println("请输入要发送的消息");
            String s = sc.nextLine();
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            out.write(s);
            System.out.println("消息发送完毕");
            //需要刷出数据否则报错Cannot send after socket shutdown: socket write error
            out.flush();

            //-------------------write-------------------
            socket.shutdownOutput();
            reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            String str = null;
            System.out.println("正在读取中...");
            while ((str = reader.readLine()) != null) {
                System.out.println(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally{
            //1.避免异常  2.捕获自身携带的异常
            if (null != out) {
                try {
                    out.close();
                    System.out.println("流已经关闭完毕");
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if(null != reader){
                try{
                    reader.close();
                    System.out.println("读取流已关闭");
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }

        //使用PrintStream来进行写出时打开它的close不需要抛出异常
//        if (null != out) {
            //out.println(s);
            //out.close();
            //System.out.println("消息发送完毕");
//        }

        //关闭套接字
        if (null != socket) {
            try {
                socket.close();
                System.out.println("套接字流已关闭");
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

RUN

```
------------------服务端
等待连接中...
连接成功
请输入您要发送的消息
好的我知道了
正在读取数据中...
读取到的数据为 :你好我是刘桑
inputStream流已经关闭
套接字已关闭
服务器套接字已关闭
------------------客户端
请输入要发送的消息
你好我是刘桑
消息发送完毕
正在读取中...
好的我知道了
流已经关闭完毕
读取流已关闭
套接字流已关闭
```

### 网络编程与多线程实现多对一:evergreen_tree: 

服务端(接收端)

```java
@SuppressWarnings("all")
public class ServerSocketThreadTest {
    public static void main(String[]args){
        //定义服务器套接字(大插排)扩大使用范围将其提炼出来并先赋值为null
        ServerSocket serverSocket = null;
        //定义套接字(小插排)扩大使用范围将其提炼出来并先赋值为Null
        Socket socket = null;
        //定义变量记录连接的数量
        int lin = 1;
        try{
            //实例化服务器套接字
                serverSocket = new ServerSocket(2333);
                //循环创建线程来连接服务器套接字
            while(true){
                System.out.println("等待客户端连接中...");
                //通过accept实例化执行到这里会将程序阻塞等待客户端的连接
                socket = serverSocket.accept();
                System.out.println("连接成功");
                //实例化多线程类将连接后的套接字传入构造器中并启动线程
                new ServerThreadTest(socket).start();
                System.out.println("当前连接数: "+lin);
                lin ++;
            }
        }catch(IOException e){
            e.printStackTrace();
        }
        if(null != socket){
            try{
                socket.close();
            }catch(IOException e){
                e.printStackTrace();
            }
        }
        if(null != serverSocket){
            try{
                serverSocket.close();
            }catch(IOException e){
                e.printStackTrace();
            }
        }
    }
}
```

客户端(发送端)

```java
@SuppressWarnings("all")
public class ClientThreadTest {
    public static void main(String[]args){
        //实例化键盘输入对象
        Scanner sc = new Scanner(System.in);
        //定义套接字变量扩大使用范围将其提炼出来并先赋值为null
        Socket socket = null;
        //定义字符输入流缓冲流变量扩大使用范围将其提炼出来并先赋值为null
        BufferedReader reader = null;
        //定义打印流变量扩大使用范围将其提炼出来并先赋值为null
        PrintStream print = null;
        try{
            //实例化套接字构造器中 传入目标的IP地址和端口号
            socket = new Socket(InetAddress.getLocalHost(),2333);
            //实例化字符输入流
            reader = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            //实例化打印流
            print = new PrintStream(socket.getOutputStream());
            //无限循环
            while(true){
                System.out.println("请输入要发送的内容:");
                String s = sc.next();
                //将键盘输入的内容传入打印流
                print.println(s);
                if("bye".equals(s)){
                    System.out.println("程序已退出");
                    break;
                }
                System.out.println("客户端发送数据成功");
                //读取服务器返回来的数据
                String str = reader.readLine();
                System.out.println("服务器返回的消息为: "+str);
            }
        }catch(IOException e) {
            e.printStackTrace();
        }finally{
            if(null != reader){
                try{
                    reader.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
            if(null != print){
                print.close();
            }
        }

        if(null != socket){
            try{
                socket.close();
            }catch(IOException e){
                e.printStackTrace();
            }
        }
    }
}
```

线程类(Thread)

```java
@SuppressWarnings("all")
public class ServerThreadTest extends Thread{
    //定义套接字成员变量,合成复用原则,多线程实现多个对象客户端连接到该套接字
    private Socket s;
    //定义有参构造器将套接字成员变量作为参数
    public ServerThreadTest(Socket s){
        //将传入的套接字连入的对象传入成员变量
        this.s = s;
    }

    @Override
    public void run(){
        //定义字符输入流变量扩大使用范围将其提炼出来并先赋值为null
        BufferedReader reader = null;
        //定义打印流变量扩大使用范围将其提炼出来并先赋值为null
        PrintStream print = null;
        try{
            //实例化字符输入流
            reader = new BufferedReader(new InputStreamReader(s.getInputStream()));
            //实例化打印流
            print = new PrintStream(s.getOutputStream());
            //无限循环
            while(true){
                //读取客户端发送过来的数据
                String re = reader.readLine();
                System.out.println("客户端发送的消息为: "+re);
                //结束的判断条件
                if("bye".equals(re)){
                    System.out.println("程序已结束");
                    break;
                }
                //打印出服务器读取到客户端的数据
                print.println(re);
                System.out.println("数据读取成功");
            }
        }catch(IOException e){
            e.printStackTrace();
        }finally{
            if(null != reader){
                try{
                    reader.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
            if(null != print){
                print.close();
            }
        }
    }
}
```

RUN

客户端

![!in](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081412204.png)

服务端 , 依旧保持可被连接的状态可连接多个客户端对象

![wdawd](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081412238.png)

### 基于UDP协议的编程模型(熟悉):evergreen_tree: 

**编程模型** 

- 接收放

1. 创建DatagramSocket类型的对象并提供端口号

2. 创建DatagramPacket类型的对象并提供缓冲区

3. 通过Socket接收数据内容存放到Packet中,调用receive方法(接收数据报)

4. 关闭Socket

- 发送方:

1. 创建DatagramSocket类型的对象

2. 创建DatagramPacket类型的对象并提供接收方的通信地址

3. 通过Socket将Packet中的数据内容发送出去,调用send方法(发送数据报)

4. 关闭Socket

**相关类和方法的解析** 

**1. DatagramSocket类** 

- java.net.DatagramSocket类,主要用于描述发送和接收数据报的套接字(邮局),换句话说,该类就是包囊投递服务的发送或接受点

- 常用的方法如下:

| 方法声明                       | 功能介绍                           |
|--------------------------------|------------------------------------|
| DatagramSocket()               | 使用无参的方式构造对象             |
| DatagramSocket(int port)       | 根据指定的端口来构造对象           |
| void receive(DatagramPacket p) | 用于接收数据报存放到参数指定的位置 |
| void send(DatagramPacket p)    | 用于将参数指定的数据报发送出去     |
| voiid close()                  | 关闭socket并释放相关资源           |

**DatagramPacket类** 

- java.net.DatagramPcaket类主要用于描述数据报,数据报用来实现无连接包裹投递服务

- 常用的方法如下:

| 方法声明                                                           | 功能介绍                                                   |
|--------------------------------------------------------------------|------------------------------------------------------------|
| DatagramPacket(byte[] buf,int length)                              | 根据参数指定的数组来构造对象,用于接收长度为Length的数据报m |
| DatagramPacket(byte[] buf,int length,InetAddress address,int port) | 根据参数指定数组来构造对象,将数据报发送到指定地址和端口    |
| InetAddress getAddress()                                           | 用于获取发送或接收方的通信地址                             |
| int getPort()                                                      | 用于获取发送方或接收方的端口号                             |
| int  getLength()                                                   | 用于获取发送数据报或接收数据报的长度                       |

**InetAddress类** 

- java.net.InetAddress类主要用于描述互联网通信地址信息

- 常用的方法如下:

| 方法声明                                  | 功能介绍                         |
|-------------------------------------------|----------------------------------|
| static InetAddress getLcoalHost()         | 用于获取当前主机的通信地址       |
| static InetAddress getByName(String host) | 根据参数指定的主机名获取通信地址 |

```java
@SuppressWarnings("all")
public class UDPServer {
    public static void main(String[]args){
        //定义数据换缓冲区
        byte[] by = new byte[1024];
        DatagramPacket packet = null;
        DatagramSocket socket = null;
        DatagramPacket packet1 = null;
        DatagramSocket socket1 = null;
        try{
            //实例化集装箱构造器接收缓冲区大小的字节数据
            packet = new DatagramPacket(by,by.length);
            //实例化码头对象并指定端口号
            socket = new DatagramSocket(2333);
            //接收数据报并存放集装箱中
            System.out.println("等待接收发送方数据报");
            socket.receive(packet);
            System.out.println("接收完毕");
            String str = "对不起,我不好";
            //将字符串转换为byte字节数据
            byte[] by1 = str.getBytes();
            //实例化集装箱构造器传入字节数组的整体和长度,指定IP地址和端口号,通过packet.getPort获取发送方的端口号
            packet1 = new DatagramPacket(by1,by1.length,InetAddress.getLocalHost(),packet.getPort());
            //发送数据报
            socket.send(packet1);
        }catch(IOException e){
            e.printStackTrace();
        }
        //获取发送方的IP地址
        InetAddress address = packet.getAddress();
        //获取发送数据报的长度
        int i  = packet.getLength();
        //获取发送数据报的字节数据
        byte[] by1 = packet.getData();
        //将数据报转换为十进制
        String str = new String(by1,0,i);
        System.out.println("主机地址为: "+address + " 发送的消息: "+str);
        if(null != socket){
            //关闭码头对象,释放有关的资源
            socket.close();
        }
    }
}
```

```java
@SuppressWarnings("all")
public class UDPClient {
    public static void main(String[]args){
        String str = "你好,兄弟";
        //将字符串转换为字节数据
        byte[] by = str.getBytes();
        DatagramPacket packet = null;
        DatagramSocket socket = null;
        DatagramPacket packet1 = null;
        DatagramSocket socket1 = null;
        try{
            //实例化集装箱构造器中传入整个数组和数组长度,指定对方IP和端口号,用于封装通信中的数据报
            packet = new DatagramPacket(by,by.length,InetAddress.getLocalHost(),2333);
            //实例化码头,用于存放通信中的数据报
            socket = new DatagramSocket();
            //发送数据报
            socket.send(packet);
            //定义字节数组缓冲区
            byte[] by1 = new byte[1024];
            //实例化集装箱构造器传入接收字节数组缓冲区大小的数据报,用于封装通信中的数据报
            packet1 = new DatagramPacket(by1,by1.length);
            //实例化码头，用于存放通信中的数据报,端口号不能一致否则报：Address already in use: Cannot bind
            //socket1 = new DatagramSocket(2334);
            System.out.println("等待接收发送方数据报");
            //接收发送方的数据报存放到集装箱中，执行到此方法会阻塞等待数据报
            socket.receive(packet1);
            System.out.println("接收完毕");
        }catch(IOException e){
            e.printStackTrace();
        }
        //获取发送发的IP
        InetAddress address = packet1.getAddress();
        //获取数据报的长度
        int i = packet1.getLength();
        //获取数据报的字节数据
        byte[] by2 = packet1.getData();
        //将数据报转换为十进制
        String str2 = new String(by2,0,i);
        System.out.println("主机名: "+address+" 发送过来消息: "+str2);
        //避免异常
        if(null != socket){
            //关闭码头，释放有关的资源
            socket.close();
        }
    }
}
```

RUN

```
ClientTest
等待接收发送方数据报
接收完毕
主机名: /192.168.0.39 发送过来消息: 对不起,我不好
ServerTest
等待接收发送方数据报
接收完毕
主机地址为: /192.168.0.39 发送的消息: 你好,兄弟
```

### URL类（熟悉）:evergreen_tree: 

**基本概念** 

- java.net.URL(Uniform Resource Identifier) 类主要用于表示统一的资源定位器，也就是指向万维网上"资源"的指针，这个资源可以是简单的文件或目录，也可以是对复杂对象的引用，例如对数据库或搜索引擎的查询等

- 通过URL可以访问万维网上的网络资源，最常见的就是www和ftp站点，浏览器通过解析给定的URL可以在网络上查找相应的资源

- URL的基本结构如下：

<传输协议>://<主机名>:<端口号>/<资源地址>

**常用的方法** 

| 方法声明                        | 功能介绍                         |
|---------------------------------|----------------------------------|
| URL(String spec)                | 根据参数指定的字符串信息构造对象 |
| String getProtocol()            | 获取协议名称                     |
| String getHost()                | 获取主机名称                     |
| int getPort()                   | 获取端口号                       |
| int getPath()                   | 获取路径信息                     |
| String getFile()                | 获取文件名                       |
| URL Connection openConnection() | 获取URLConnection类的实例        |

**URLConnection类** 


**基本概念** 

- java.net.URLConnectiion类是个抽象类，该类表示应用程序和URL之间的通信连接的所有类的超类，主要使用类有支持HTTP特有功能的HttpURLCOnnection类

**HttpURLConnection类的常用的方法** 

| 方法声明 | 功能介绍 |
|----------|----------|
| InputStream getInputStream()     | 获取输入流     |
| void disconnect()     | 断开连接     |

```java
@SuppressWarnings("all")
public class URLTest {
    public static void main(String[]args){
        URL url = null;
        try{
            url = new URL("https://www.lagou.com/");
        }catch(IOException e){
            e.printStackTrace();
        }
        System.out.println("地址的协议为: "+url.getProtocol());
        System.out.println("地址的主机名为: "+url.getHost());
        //如果端口号为-1代表获取失败
        System.out.println("地址的端口号为: "+url.getPort());
        System.out.println("地址的路径信息为: "+url.getPath());
        System.out.println("地址的文件名称为: "+url.getFile());
        HttpURLConnection httpurlConnection = null;
        InputStream input = null;
        BufferedReader reader = null;
        try{
            //通过openConnection获取httpurlConnection实例对象
            httpurlConnection = (HttpURLConnection) url.openConnection();
            //获取输入流
            input = httpurlConnection.getInputStream();
            //字节流经过转换流为字符缓冲流
            reader = new BufferedReader(new InputStreamReader(input));
            String  str = null;
            while((str = reader.readLine()) != null){
                System.out.println(str);
            }
        }catch(IOException e){
            e.printStackTrace();
        }finally{
            if(null != httpurlConnection){
                //断开连接
                httpurlConnection.disconnect();
            }
            if(null != input){
                try{
                    //关闭流，释放有关的资源
                    input.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
            if(null != reader){
                try{
                    //关闭流，释放有关的资源
                    reader.close();
                }catch(IOException e){
                    e.printStackTrace();
                }
            }
        }
    }
}
-----------------------RUN-----------------------
地址的协议为: https
地址的主机名为: www.lagou.com
地址的端口号为: -1
地址的路径信息为: /
地址的文件名称为: /
<!DOCTYPE html>
<html>
<head>
<!-- meta -->
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<meta name="renderer" content="webkit">
<meta property="qc:admins" content="23635710066417756375" />
<meta name="baidu-site-verification" content="QIQ6KC1oZ6" />
<meta content="拉勾招聘,互联网招聘,求职,找工作,招聘,招聘网站" name="keywords">
<meta content="拉勾招聘是专业的互联网求职招聘网站。致力于提供真实可靠的互联网岗位求职招聘找工作信息，拥有海量的互联网人才储备，互联网行业找工作就上拉勾招聘，值得信赖的求职招聘网站。" name="description">
<meta name="applicable-device" content="pc">
<title>互联网求职招聘找工作-上拉勾招聘-专业的互联网求职招聘网站</title>
```

-  因为这个站点只是个首页站点所以地址路径和地址文件名都是 / 

# 反射机制:deciduous_tree: 

**基本概念** 

- 通常情况下编写代码都是==固定==的，无论运行多少次执行的结果也都是固定的，在某些特殊场合中编写代码时不确定要创建什么类型的对象，也不确定要调用什么样的方法，这些都希望通过==运行时传递的参数来决定==，该机制叫做==动态编程==技术，也就是==反射机制== 

- 通俗来说，反射机制就是用于==动态创建对象==并且==动态调用方法==的机制

- 目前主流的框架底层都是采用反射机制实现的

- 如：

```java
Person p = new Person();表示声明Person类型的引用指向Person类型的对象
p.show();表示调用Person类中的成员方法show()
```

### Class类:evergreen_tree: 

**基本概念** 

- java.lang.Class类的实例可以用于描述java应用程序中的接口，也就是一种==数据类型== 

- 该类没有公共构造方法，该类的实例由==java虚拟机==和==类加载器==自动构造完成，本质上就是==加载到内存中的运行时类== 

**区别** 

>  Class这个类跟以前讲的类不一样的地方在于Class类对象不再是堆区中的内存空间了,而是代表加载到方法区中的正在运行的这个类

>  只要是一个类加载到内存中就会有对应的类信息,不同的类就会有不同的类信息,而不同的类信息运行时类对应的是Class类不同的对象

**获取class对象的方式** 

- 使用  [数据类型`.class` ]  的方式可以获取对象对应类型的Class对象 (掌握)

- 使用  [引用/对象`.getClass()`]   的方式可以获取对应类的Class对象

- 使用  [包装类`.TYPE` ]  的方式可以获取对应基本数据类型的Class对象

- 使用  [`Class.forName()` ]  的方式来获取参数指定类型的Class对象 (掌握)

- 使用  [类加载器`ClassLoader`]  的方式获取指定类型的Class对象

```java
@SuppressWarnings("all")
public class ClassTest {
    public static void main(String[]args){
        //使用[数据类型.class]方式获取对应类型的Class对象
        Class cls =  String.class;
        System.out.println(cls);//自动调用toString()方法; class java.lang.String
        Class cls1 = int.class;
        System.out.println(cls1);//int
        Class cls2 = void.class;
        System.out.println(cls2);//viod
        System.out.println("--------------------------");
        //使用[对象.getClass]的方式获取对应的Class对象
        String str = new String("hello");
        cls = str.getClass();
        System.out.println(cls);//class java.lang.String

        Integer in = 20;
        cls = in.getClass();
        System.out.println(cls);//class java.lang.Integer
        System.out.println("--------------------------");
        //使用[包装类.TYPE]的方式获取对应基本数据类型的Class对象
        cls = Integer.TYPE;
        System.out.println(cls);//int
        cls = Character.TYPE;
        System.out.println(cls);//char
        System.out.println("--------------------------");
        //调用Class类中的forName方法来获取对应的Class对象
        try{
            //cls = Class.forName("String");//报错java.lang.ClassNotFoundException: String
            cls1 = Class.forName("java.lang.String");
            cls2 = Class.forName("java.util.Date");
            //Class.forName("int");//不能获取基本数据类型,因为不是类报错java.lang.ClassNotFoundException: int
        }catch(ClassNotFoundException e){
            e.printStackTrace();
        }
        //System.out.println(cls);
        System.out.println(cls1);//class java.lang.String
        System.out.println(cls2);//class java.util.Date
        System.out.println("--------------------------");
        //使用类加载器[对象.getClassLoader]的方式获取Class对象
        ClassLoader c = ClassTest.class.getClassLoader();
        //ClassLoader c = String.class.getClassLoader();//报错 NullPointerException,运行时String并不在内存中所以找不到
        try{
             cls = c.loadClass("java.lang.String");
        }catch(ClassNotFoundException e){
            e.printStackTrace();
        }
        System.out.println(cls);//Class java.lang.String
    }
}
----------------------------------RUN----------------------------------
class java.lang.String
int
void
--------------------------
class java.lang.String
class java.lang.Integer
--------------------------
int
char
--------------------------
class java.lang.String
class java.util.Date
--------------------------
class java.lang.String
```

**常用的方法(掌握)** 

| 方法声明                                 | 功能介绍                                  |
| ---------------------------------------- | ----------------------------------------- |
| static Class<?>forName(String className) | 用于获取参数指定类型对应的Class对象并返回 |
| T newInstance()                          | 用于创建该Class对象所表示类的新实例       |

```java
//调用Class类中的forName方法来获取对应的Class对象
try{
   //cls = Class.forName("String");//报错java.lang.ClassNotFoundException: String
   cls1 = Class.forName("java.lang.String");
   cls2 = Class.forName("java.util.Date");
   //Class.forName("int");//不能获取基本数据类型,因为不是类报错java.lang.ClassNotFoundException: int
}catch(ClassNotFoundException e){
   e.printStackTrace();
}
```

### Constructor类:evergreen_tree: 

**基本概念** 

-  java.lang.reflect.Constructor类主要用于描述获取到的构造方法信息

**Class类的常用方法** 

| 方法声明                                                   | 功能介绍                                              |
| ---------------------------------------------------------- | ----------------------------------------------------- |
| Constructor<T> getConstructor(Class<?> ... parameterTypes) | 用于获取此Class对象所表示类型中参数指定的公共构造方法 |
| Constructor<?>[ ]  getConstructors( )                      | 用于获取此Class对象所表示类型中所有的公共构造方法     |

**Constructor类的常用方法** 

| 方法声明                           | 功能介绍                                                     |
| ---------------------------------- | ------------------------------------------------------------ |
| T newInstance(Object ... Initargs) | 使用此Constructor对象描述的构造方法来构造Class对象代表类型的新实例 |
| int getModifiers()                 | 获取方法的访问修饰符                                         |
| String getName()                   | 获取方法的名称                                               |
| Class<?>[ ] getParameterTypes()    | 获取方法所有参数的类型                                       |

**代码演示** 

##### 实体类:palm_tree: 

```java
public class Person {
    public String name;
    public int age;

    public Person(String name, int age) {this.name = name;this.age = age;}
    public Person() {}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public int getAge() {return age;}
    public void setAge(int age) {this.age = age;}

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```



```java
@SuppressWarnings("all")
public class PersonConstructorTest {
    public static void main(String[] args) {
        //使用原始方式以无参形式构造Person类的对象并打印
        Person p = new Person();
        System.out.println("无参方式创建的对象是 : " + p);
        System.out.println("----------------reflect----------------");
        Class clazz = null;
        Constructor constructor = null;
        Properties pr = new Properties();
        InputStream in = PersonConstructorTest.class.getClassLoader().getResourceAsStream("person.properties");
        try {
            pr.load(in);
            clazz = Class.forName("com.dkx.classar.Person");
            System.out.println("无参方式创建的对象是 : " + clazz.newInstance());
            clazz = Class.forName(pr.getProperty("classpath"));
            System.out.println("properties实例化对象: " + clazz.newInstance());
            constructor = clazz.getConstructor();
            System.out.println("无参方式创建的对象是: " + constructor.newInstance());
            //IllegalAccessException:没有权限异常
        } catch (IllegalAccessException e) {
            e.printStackTrace();
            //InstantiationException:实例化异常
        } catch (InstantiationException e) {
            e.printStackTrace();
            //NoSuchMethodException:找不到方法异常
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
            //ClassNotFoundException:找不到类
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        System.out.println("--------------------------------");
        Person p1 = new Person("刘桑", 10);//Person{name='刘桑', age=10}
        System.out.println(p1);
        System.out.println("----------------reflect AllArgsConstructor----------------");

        Constructor clazz1 = null;
        try {
            //使用反射机制以有参构造Person类型的对象并打印
            //获取class对象对应类中的有参构造方法,也就是Person类中的有参构造方法
            clazz1 = clazz.getConstructor(String.class, int.class);
            //使用过获取到的有参构造方法来构造对应类型的对象,也就是Person类型的对象
            //newInstance方法中的实参是用于给有参构造方法的形参车初始化的也就是给name和age初始化的
            Object o = clazz1.newInstance("刘桑", 10);//Person{name='刘桑', age=10}
            System.out.println(o);
        } catch (NoSuchMethodException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }

        System.out.println("----------------reflect AllArgsConstructor 解决不知道构造器中需要传入哪些参数的问题----------------");
        //使用反射机制获取Pseron类中所有的公共构造方法并打印
        Constructor[] constructors = clazz.getConstructors();
        for(Constructor ct : constructors){
            System.out.println("修饰符:"+ct.getModifiers());
            System.out.println("方法名称:"+ct.getName());
            Class[] fieldName = ct.getParameterTypes();
            System.out.print("构造方法中的所有参数是:");
            for(Class i : fieldName){
                System.out.print(i+"   ");
            }
            System.out.println();
            System.out.println("----------------------------");
        }
    }
}
---------------------------------RUN---------------------------------
无参方式创建的对象是 : Person{name='null', age=0}
----------------reflect----------------
无参方式创建的对象是 : Person{name='null', age=0}
properties实例化对象: Person{name='null', age=0}
无参方式创建的对象是: Person{name='null', age=0}
--------------------------------
Person{name='刘桑', age=10}
----------------reflect AllArgsConstructor----------------
Person{name='刘桑', age=10}
----------------reflect AllArgsConstructor 解决不知道构造器中需要传入哪些参数的问题----------------
修饰符:1
方法名称:com.dkx.classar.Person
构造方法中的所有参数是:class java.lang.String - int - 
----------------------------
修饰符:1
方法名称:com.dkx.classar.Person
构造方法中的所有参数是:
----------------------------
```

**动态实例对象(键盘输入)** 

```java
@SuppressWarnings("all")
public class PersonConstructorTest {
    public static void main(String[] args) {
        //键盘输入动态实例化对象
        Scanner sc = new Scanner(System.in);
        Class clazz1 = null;
        try{
            System.out.println("请输入要创建的对象类型的全限定名");
            String s = sc.next();
            clazz1 = Class.forName(s);
        }catch(ClassNotFoundException e){
            e.printStackTrace();
        }
        try{
            System.out.println("使用无参方式创建的对象是 : "+clazz1.newInstance());
        }catch(IllegalAccessException e){
            e.printStackTrace();
        }catch(InstantiationException e){
            e.printStackTrace();
        }
    }
}
---------------------------------RUN---------------------------------
请输入要创建的对象类型的全限定名
java.util.Date
使用无参方式创建的对象是 : Sat Mar 25 16:54:04 CST 2023
```

### Field类:evergreen_tree: 

**基本概念** 

-  java.lang.reflect.Field类主要用于描述获取到的单个成员变量信息

**Class类的常用方法** 

| 方法声明                            | 功能介绍                                                |
| ----------------------------------- | ------------------------------------------------------- |
| Field getDeclaredField(String name) | 用于获取此Class对象所表示类中参数指定的单个成员变量信息 |
| Field[ ] getDeclaredField()         | 用于获取此Class对象所表示类中所有成员变量信息           |

**Field类常用的方法** 

| 方法声明                          | 功能介绍                                                     |
| --------------------------------- | ------------------------------------------------------------ |
| Object get(Object obj)            | 获取参数对象obj中此Field对象所表示成员变量的数值             |
| void set(Object obj,Object value) | 将参数对象obj中此Field对象表示成员变量的数值修改为参数value的数值 |
| void setAccessible(boolean flag)  | 当实参传递true时,则反射对象在使用时应该取消Java语言访问检查  |
| int getModifiers()                | 获取成员变量的访问修饰符                                     |
| Class<?> getType()                | 获取成员变量的数据类型                                       |
| String getName()                  | 获取成员变量的名称                                           |

```java
@SuppressWarnings("all")
public class PersonFieldTest {
    public static void main(String[]args){
        Person p = new Person("李四",10);
        System.out.println("获取到成员变量name的值 : "+p.name);
        System.out.println("--------------------------reflect--------------------------");
        //使用反射机制来构造对象以及获取成员变量的数值并打印
        //获取class对象
        Class clazz = Person.class;
        Constructor constructor = null;
        Object o = null;
        Field field = null;
        try{
            //根据class对象获取对应的有参构造方法
            constructor = clazz.getConstructor(String.class , int.class);
            //使用有参构造方法来得到Person类型的对象
            o = constructor.newInstance("张三",10);
            //根据class对象获取到对应的成员变量信息
            field = clazz.getDeclaredField("name");
            //使用Person类型的对象来获取成员变量的数值并打印
            //获取对象o中名称为field(name)成员变量的数值,也就是成员变量name的数值
            System.out.println("获取到成员变量name的值 : "+field.get(o));
        }catch(NoSuchMethodException e){
            e.printStackTrace();
        }catch(IllegalAccessException e){
            e.printStackTrace();
        }catch(InvocationTargetException e){
            e.printStackTrace();
        }catch(InstantiationException e){
            e.printStackTrace();
        }catch(NoSuchFieldException e){
            e.printStackTrace();
        }
        System.out.println("--------------------------普通方式修改成员变量的值--------------------------");
        //使用原始方式修改指定对象中成员变量的数值后再次打印
        p.name = "关羽";
        System.out.println("修改后的name的值: "+p.name);
        System.out.println("--------------------------reflect修改成员变量的值--------------------------");
        try{
            //使用反射机制修改指定对象中成员变量的数值后再次打印
            //表示修改对象o中名字为field(name)的成员变量的数值为"小日子-刘桑",也就是成员变量name的数值为"小日子-刘桑"
            field.set(o,"小日子-刘桑");
            System.out.println("修改后的name的值: "+field.get(o));
        }catch(IllegalAccessException e){
            e.printStackTrace();
        }
    }
}
--------------------------------------------RUN--------------------------------------------
获取到成员变量name的值 : 李四
--------------------------reflect--------------------------
获取到成员变量name的值 : 张三
--------------------------普通方式修改成员变量的值--------------------------
修改后的name的值: 关羽
--------------------------reflect修改成员变量的值--------------------------
修改后的name的值: 小日子-刘桑
```

<a href="#实体类">**查看实体类**</a> 

==以上是访问public 修饰的成员变量,如果访问private 成员变量则会报错== 

>  <font style="color:red">java.lang.IllegalAccessException: class com.dkx.classar.PersonFieldTest cannot access a member of class com.dkx.classar.Person with modifiers "private"</font> 

<font style="color:green">**解决办法:  使用setAccessible(boolean f) ， 在参数列表中传入true开启有人称开启爆破，有人称关闭语言访问检查(忽略私有检查)** </font> 

实体类的私有变量 , 还是上个实体类只是成员变量改为私有的了

```java
    private String name;
    private int age;
```

```java
@SuppressWarnings("all")
public class PersonFieldTest1 {
    public static void main(String[]args){
        //原始方式只能通过get / set方法来对私有变量的访问和修改
        Person p = new Person("李四",10);
        p.setName("关羽");
        System.out.println("修改后的name的值: "+p.getName());
        System.out.println("--------------------------reflect--------------------------");
        //使用反射机制来构造对象以及获取成员变量的数值并打印
        //获取class对象
        Class clazz = Person.class;
        Constructor constructor = null;
        Object o = null;
        Field field = null;
        try{
            //根据class对象获取对应的有参构造方法
            constructor = clazz.getConstructor(String.class , int.class);
            //使用有参构造方法来得到Person类型的对象
            o = constructor.newInstance("张三",10);
            //根据class对象获取到对应的成员变量信息
            field = clazz.getDeclaredField("name");
            //开启爆破/取消语言访问检查
            field.setAccessible(true);
            //使用Person类型的对象来获取成员变量的数值并打印
            //获取对象o中名称为field(name)成员变量的数值,也就是成员变量name的数值
            System.out.println("获取到成员变量name的值 : "+field.get(o));
        }catch(NoSuchMethodException e){
            e.printStackTrace();
        }catch(IllegalAccessException e){
            e.printStackTrace();
        }catch(InvocationTargetException e){
            e.printStackTrace();
        }catch(InstantiationException e){
            e.printStackTrace();
        }catch(NoSuchFieldException e){
            e.printStackTrace();
        }
    }
}
-------------------------------------------RUN-------------------------------------------
修改后的name的值: 关羽
--------------------------reflect--------------------------
获取到成员变量name的值 : 张三
```

**获取私有的成员变量和构造方法** 

**实体类** 

```java
    private String name;
    private int age;

    private Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
```

```java
@SuppressWarnings("all")
public class PersonFieldTest1 {
    public static void main(String[]args){
        //原始方式只能通过get / set方法来对私有变量的访问和修改
        //Person p = new Person("李四",10);//私有的构造方法,不能为其构造器中赋值
//        p.setName("关羽");
        //System.out.println("修改后的name的值: "+p.getName());
        System.out.println("--------------------------reflect--------------------------");
        //使用反射机制来构造对象以及获取成员变量的数值并打印
        //获取class对象
        Class clazz = Person.class;
        Constructor constructor = null;
        Object o = null;
        Field field = null;
        try{
            //根据class对象获取对应的有参构造方法,使用Declared
            constructor = clazz.getDeclaredConstructor(String.class , int.class);
            //使用有参构造方法来得到Person类型的对象
            constructor.setAccessible(true);
            o = constructor.newInstance("张三",10);
            //根据class对象获取到对应的成员变量信息
            field = clazz.getDeclaredField("name");
            //开启爆破/取消语言访问检查
            field.setAccessible(true);
            //使用Person类型的对象来获取成员变量的数值并打印
            //获取对象o中名称为field(name)成员变量的数值,也就是成员变量name的数值
            System.out.println("获取到成员变量name的值 : "+field.get(o));
        }catch(NoSuchMethodException e){
            e.printStackTrace();
        }catch(IllegalAccessException e){
            e.printStackTrace();
        }catch(InvocationTargetException e){
            e.printStackTrace();
        }catch(InstantiationException e){
            e.printStackTrace();
        }catch(NoSuchFieldException e){
            e.printStackTrace();
        }
        System.out.println("--------------------------reflect遍历有参构造器--------------------------");
        Constructor[] constructors = clazz.getConstructors();
        for(Constructor ct : constructors){
            System.out.println("方法名称: "+ct.getName());
            System.out.println("方法修饰符: "+ct.getModifiers());
            System.out.print("有参构造中的参数: ");
            Class[] parameter = ct.getParameterTypes();
            for(Class i : parameter){
                System.out.print(i+"  ");
            }
            System.out.println();
            System.out.println("----------------------------------");
        }
        System.out.println("--------------------------reflect遍历成员变量--------------------------");
        Field[] fields = clazz.getDeclaredFields();
        for(Field i : fields){
            System.out.println("获取修饰符: "+i.getModifiers());
            System.out.println("获取变量的数据类型: "+i.getType());
            System.out.println("获取变量的名称: "+i.getName());
            System.out.println("---------------------------");
        }
    }
}
-----------------------------RUN-----------------------------
--------------------------reflect--------------------------
获取到成员变量name的值 : 张三
--------------------------reflect遍历有参构造器--------------------------
方法名称: com.dkx.classar.Person
方法修饰符: 1
有参构造中的参数: 
----------------------------------
--------------------------reflect遍历成员变量--------------------------
获取修饰符: 2
获取变量的数据类型: class java.lang.String
获取变量的名称: name
---------------------------
获取修饰符: 2
获取变量的数据类型: int
获取变量的名称: age
---------------------------
```

### Method类:evergreen_tree: 

**基本概念** 

-  java.lang.reflect.Method类主要用于描述获取到的单个成员方法信息

**Class类的常用方法** 

| 方法声明                                                  | 功能介绍                                                     |
| --------------------------------------------------------- | ------------------------------------------------------------ |
| Method getMethod(String name,Class<?> ... parameterTypes) | 用于获取该Class对象表示类中名字为name参数为parameterTypes的指定公共成员方法 |
| Method[ ] getMethods()                                    | 用于获取该Class对象表示类中所有公共成员方法                  |

**Method类的常用方法** 

| 方法声明                                  | 功能介绍                                                   |
| ----------------------------------------- | ---------------------------------------------------------- |
| Object invoke(Object obj,Object ... args) | 使用对象obj来调用此Method对象所表示的成员方法,实参传递args |
| int getModifiers()                        | 获取方法的访问修饰符                                       |
| Class<?> getReturnType()                  | 获取方法的返回值类型                                       |
| String getName()                          | 获取方法的名称                                             |
| Class<?>[ ] getParameterTypes()           | 获取方法所有参数的类型                                     |
| Class<?>[ ] getExceptionTypes()           | 获取方法的异常信息                                         |

```java
@SuppressWarnings("all")
public class PersonMethodTest {
    public static void main(String[]args){
        System.out.println("---------------------------原始方式-----------------------------");
        //实例化Pseron对象构造器传入参数
        Person p = new Person("张三",10);
        //通过getName获取name的值
        String name = p.getName();
        //通过getAge获取age的值
        int age = p.getAge();
        System.out.println("名称:"+name+","+"年龄:"+age);
        System.out.println("---------------------------reflect获取类中指定成员方法-----------------------------");
        //通过反射机制构造对象并调用方法打印结果
        //获取Person的class对象
        Class clazz = Person.class;
        try{
            //根据class对象来获取对应的有参构造方法
            Constructor constructor = clazz.getConstructor(String.class,int.class);
            //使用有参构造方法实例化对象并传入构造参数
            Object obj = constructor.newInstance("刘桑",10);
            //根据class对象调用getMethod传入对应的方法名获取该方法
            Method methodName = clazz.getMethod("getName");
            //根据class对象调用getMethod传入对应的方法名获取该方法
            Method methodAge = clazz.getMethod("getAge");
            //方法对象调用invoke并传入实例对象是谁表示调用某个类的getName和getAge方法
            System.out.println("名称:"+methodName.invoke(obj)+","+"年龄:"+methodAge.invoke(obj));
        }catch(IllegalAccessException e){
            e.printStackTrace();
        }catch(InstantiationException e){
            e.printStackTrace();
        }catch(NoSuchMethodException e){
            e.printStackTrace();
        }catch(InvocationTargetException e){
            e.printStackTrace();
        }
        System.out.println("---------------------------reflect获取类中所有成员方法-----------------------------");
        Method[] methods = clazz.getMethods();
        for(Method i : methods){
            System.out.println("获取方法的名称:"+i.getName());
            System.out.println("获取方法返回值类型:"+i.getReturnType());
            System.out.println("获取方法修饰符:"+i.getModifiers());
            Class[] c = i.getParameterTypes();
            System.out.print("获取方法的参数类型:  ");
            for(Class i1 : c){
                System.out.print(i1+"   ");
            }
            System.out.print("方法异常类型:");
            Class[] exception = i.getExceptionTypes();
            for(Class ex : exception){
                System.out.print(ex);
            }
            System.out.println();
            System.out.println("------------------------");
        }
    }
}
----------------------------RUN----------------------------
---------------------------原始方式-----------------------------
名称:张三,年龄:10
---------------------------reflect获取类中指定成员方法-----------------------------
名称:刘桑,年龄:10
---------------------------reflect获取类中所有成员方法-----------------------------
获取方法的名称:getName
获取方法返回值类型:class java.lang.String
获取方法修饰符:1
获取方法的参数类型:  方法异常类型:
------------------------
获取方法的名称:toString
获取方法返回值类型:class java.lang.String
获取方法修饰符:1
获取方法的参数类型:  方法异常类型:
------------------------
获取方法的名称:setName
获取方法返回值类型:void
获取方法修饰符:1
获取方法的参数类型:  class java.lang.String   方法异常类型:
------------------------
获取方法的名称:setAge
获取方法返回值类型:void
获取方法修饰符:1
获取方法的参数类型:  int   方法异常类型:
------------------------
获取方法的名称:getAge
获取方法返回值类型:int
获取方法修饰符:1
获取方法的参数类型:  方法异常类型:
------------------------
获取方法的名称:wait
获取方法返回值类型:void
获取方法修饰符:273
获取方法的参数类型:  long   方法异常类型:class java.lang.InterruptedException
------------------------
获取方法的名称:wait
获取方法返回值类型:void
获取方法修饰符:17
获取方法的参数类型:  long   int   方法异常类型:class java.lang.InterruptedException
------------------------
获取方法的名称:wait
获取方法返回值类型:void
获取方法修饰符:17
获取方法的参数类型:  方法异常类型:class java.lang.InterruptedException
------------------------
获取方法的名称:equals
获取方法返回值类型:boolean
获取方法修饰符:1
获取方法的参数类型:  class java.lang.Object   方法异常类型:
------------------------
获取方法的名称:hashCode
获取方法返回值类型:int
获取方法修饰符:257
获取方法的参数类型:  方法异常类型:
------------------------
获取方法的名称:getClass
获取方法返回值类型:class java.lang.Class
获取方法修饰符:273
获取方法的参数类型:  方法异常类型:
------------------------
获取方法的名称:notify
获取方法返回值类型:void
获取方法修饰符:273
获取方法的参数类型:  方法异常类型:
------------------------
获取方法的名称:notifyAll
获取方法返回值类型:void
获取方法修饰符:273
获取方法的参数类型:  方法异常类型:
------------------------
```

**实体类** 

```java
public class Person {
    private String name;
    private int age;
    public Person(String name, int age) {this.name = name;this.age = age;}
    public Person() {}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public int getAge() {return age;}
    public void setAge(int age) {this.age = age;}
    @Override
    public String toString() {return "Person{" + "name='" + name + '\'' + ", age=" + age + '}';}
}
```

#### getMethods方法与getDeclaredMethods的区别:palm_tree: 

-  getMethods方法返回的是当前Class对象的所有共有的方法，包含从父类或父类接收继承而来的方法

-  getDeclaredMethods方法返回的是当前Class对象的所有（包括:public,protected,default,private）方法，但是并不包括继承自父类或父接口的方法

<font style="color:red">**代码演示**</font> 

<u>getMethods</u> 

```java
        Class clazz = Demo.class;
        Method[] method = clazz.getMethods();
        for (Method i:method) {
            System.out.println(i);
        }
-------------------RUN-------------------
public void com.dkx.classday13.Demo.run()
public java.lang.String com.dkx.classday13.Demo.toString()
public void com.dkx.classday13.Parent.art()
public final native void java.lang.Object.wait(long) throws java.lang.InterruptedException
public final void java.lang.Object.wait(long,int) throws java.lang.InterruptedException
public final void java.lang.Object.wait() throws java.lang.InterruptedException
public boolean java.lang.Object.equals(java.lang.Object)
public native int java.lang.Object.hashCode()
public final native java.lang.Class java.lang.Object.getClass()
public final native void java.lang.Object.notify()
public final native void java.lang.Object.notifyAll()
```

<u>getDeclaredMethods</u> 

```java
        Class clazz = Demo.class;
        Method[] method = clazz.getDeclaredMethods();
        for (Method i:method) {
            System.out.println(i);
        }
-------------------RUN-------------------
public void com.dkx.classday13.Demo.run()
public java.lang.String com.dkx.classday13.Demo.toString()
```



### 获取其它结构信息:evergreen_tree: 

| 方法声明                         | 功能介绍           |
| -------------------------------- | ------------------ |
| Pckage getPckage()               | 获取所在的包信息   |
| Class<? super T> getSuperclass() | 获取继承的父类信息 |
| Class<?>[ ] getInterfaces()      | 获取实现的所有接口 |
| Annotation[ ] getAnnotations()   | 获取注解信息       |
| Type[ ] getGenericlnterfaces()   | 获取泛型信息       |

```java
@SuppressWarnings("all")
public class StudentTest {
    public static void main(String[]args){
        //获取student类型的class对象
        Class c = Student.class;
        //获取的class为class com.dkx.classar.Student
        String cname = c.toString();
        //将class与类路径分割
        String[] pathname = cname.split(" ");
        Class clazz = null;
        try{
            //只取路径的部分传入forName中
            clazz = Class.forName(pathname[1]);
            System.out.println(clazz);
        }catch(ClassNotFoundException e){
            e.printStackTrace();
        }
        System.out.println("获取到的包信息:"+clazz.getPackage());
        System.out.println("获取到父类的信息:"+clazz.getSuperclass());
        Class[] interfaces = clazz.getInterfaces();
        System.out.println("-------------------------------");
        System.out.print("获取到的接口信息:");
        for(Class i:interfaces){
            System.out.print(i+"   ");
        }
        System.out.println();
        System.out.println("-------------------------------");
        Annotation[] annotation = clazz.getAnnotations();
        System.out.print("获取到的注解信息:");
        for(Annotation i:annotation){
            System.out.print(i+"   ");
        }
        System.out.println();
        System.out.println("-------------------------------");
        Type[] genericinterfaces = clazz.getGenericInterfaces();
        System.out.print("获取到泛型的接口信息:");
        for(Type i:genericinterfaces){
            System.out.print(i+"   ");
        }
        System.out.println();
        System.out.println("-------------------------------");
    }
}
----------------------------RUN----------------------------
class com.dkx.classar.Student
获取到的包信息:package com.dkx.classar
获取到父类的信息:class com.dkx.classar.Person
-------------------------------
获取到的接口信息:interface java.lang.Comparable   interface java.io.Serializable   
-------------------------------
获取到的注解信息:@com.dkx.classar.MyAnnotation()   
-------------------------------
获取到泛型的接口信息:java.lang.Comparable<java.lang.String>   interface java.io.Serializable   
-------------------------------
```

![awr](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081413508.png)