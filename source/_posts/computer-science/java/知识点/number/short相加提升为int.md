---
title: 为什么short相加会自动提升为int？
categories:
    - [计算机学科,java,知识点,number]
tags:
    - number
    - 知识点
---

## 为什么short相加会自动提升为int？

Java中进行二元与运算类型的提升规则

整数运算：

如果两个操作数有一个为long，则结果也为long；

没有long时，结果为int，即使操作数全为short，byte，结果也是int。

浮点数运算：

如果两个操作数有一个为double，则结果为double；

只有两个操作数都是float，则结果才是float。

注意：int与float运算，结果为float。

为什么两个short类型相加会自动提升为int？

![image-20230618093102338](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081432091.png)

s1+s2系统会自动将它们提升为int再运算，结果为int类型，赋给short类型，编译报错；s3 = s2 + 1 也是同样的原因；s3 = 4 + 45，系统先计算4 + 45 = 50，也就是变为s3 = 50,50在short表示的范围内自动转型为short，但是为什么java在两个short型运算时自动提升为int，即使它们没有超过表示范围？

![image-20230618093321379](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081432680.png)

使用较小类型运算没有性能优势，消除较小的类型使得字节码更简单，并且使得具有未来扩展空间的完整指令集仍然适合单个字节码中的操作码，因此，较小的类型通常被视为Java设计中的二等公民，在各个步骤转换为int，因为这简化了一些事情。

Why does the Java API use int instead of short or byte?

为什么 Java API 使用 int 而不是 short 或 byte？

![image-20230618093548011](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081432375.png)

![image-20230618093559929](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081432055.png)

答案可能只与Java虚拟机指令集有关。较小的类型(byte，short)基本上只用于数组。

If each typed instruction supported all of the Java Virtual Machine's run-time data types, there would be more instructions than could be represented in a byte。

为较小的类型引入专用的算术逻辑单元不值得付出努力：它需要额外的晶体管，但它仍然只能在一个时钟周期内执行一次加法。 JVM设计时的主流架构是32位，适合32位int。
