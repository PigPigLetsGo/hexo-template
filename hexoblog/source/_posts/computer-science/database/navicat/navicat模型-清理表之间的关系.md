---
title: navicat模型-清理表之间的关系
categories:
    - [计算机学科,database,navicat]
tags:
    - database
    - 计算机学科
    - navicat
---

# navicat模型-清理表之间的关系

当我们使用navicat中模型的时候，将我们需要看的表导入到模型中时如下图：

![image-20240331104125115](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240331104125115.png)

我们可以看到红线所标注的线两边有不同的图标，看了可能会一头雾水

![image-20240331104349530](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240331104349530.png)

废话不多说，我们着重分析user与order表，两者是典型的一对多关系，那么order是多方，user是一方。所以，你看懂了吗？order是三个叉叉叉，user是一个叉.

如果在模型上的关系出邮件查看的话是，基数在订单上的是：零个或者多个，而在user上是零个或一个。

这样你应该更加明白了。

也就是说，一对多关系上，一方也就是父方是可以是0或者1个，而对应在order方（多方，子方）是0个或者多个。

这样是不是更好的体现了一对多的关系呢？

相信你已经懂。需要说明的是，如果要求很严格的话，这样设计是比较注重细节的，同样在此关系中，如果不注意弄成了唯一或者多个的话就不能十分准确的表现业务了

单拿出来一个关系表进行分析：

![image-20240331105045467](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240331105045467.png)

如上图：tb_product_specs 就是设外键的一方，如下图：

![image-20240331105136558](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240331105136558.png)

添加数据时，由于tb_product_specs是从表，所以需要看主表：tb_product是否有对应的id，因为从表关联了主表的id字段所以要看主表的id数据，比如有1那么可以在从表添加如下数据：

![image-20240331105355411](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240331105355411.png)

还可以添加为null的数据，但是不能添加其它的数据

![image-20240331105429776](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240331105429776.png)

![image-20240331105444853](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240331105444853.png)

PS: 这里补充一点，在关系上右键的前往目标和前往源指的是前往子父表。因为一个外键关键，关联表是源，被关联的表是目标

>  这个会有三个标记，你图中出现了两个： 1，三叉的，表示设有外键的一方； 2，一叉的，表示一对一的关系； 3，还有一个等于号的标记，表示一对多的关系； ----------- 补充其他相关的概念： 1，主表&从表的概念：一个“公共关键字”，设为主键的表为“父表”，否则为“从表”；故该公共关键字为从表的“外键”； 数据表的三种关系： 1，一对一：一个A表，只能对应一个B表； 2，一对多：一个A表，可以对应多个B表； 3，多对多：一个A表，可以对应多个B表；一个B表，也可以对应多个A表
>  

