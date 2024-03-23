---
title: MybatisPlus中使用SelectById查询数据总是null
categories:
    - [计算机学科,坑点]
tags:
    - 计算机学科
    - 坑点
---

# MybatisPlus中使用SelectById查询数据总是null

今天遇到了一个非常坑的问题！

使用MyBatisPlus框架 去查询了一条数据如下：

![image-20240312110738645](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240312110738645.png)

注意了 ，这个表中明明有id的值但是我们查询的代码如下：

```java
@Override
public ShopOrder getOne(Integer id) {
   ShopOrder shopOrder = mapper.selectOne(new LambdaQueryWrapper<ShopOrder>().eq(id != null, ShopOrder::getShopId, id));
   System.out.println(shopOrder);
   return shopOrder;
}
```

结果总是查询出来的数据为null 于是看了下字段发现了有个逻辑删除is_del 没有被赋值是个null 改成了0结果可以被查询出来了，记录一下今天遇到的坑。