---
title: MyBatis-ORM
categories:
  - [计算机学科,database,mybatis]
tags:
  - 计算机学科
  - database
  - mybatis
  - ORM
---

# MyBatis什么是ORM(对象关系映射)

## 一, 什么是ORM

对象关系映射(Object Relational Mapping),简称ORM , 模式是一种==为了解决面向对象与关系数据库存在的互补匹配的现象技术==

--------

怎么理解上面的那句话呢?

在Java中我们使用String表示字符串,而Oracle中可以使用varchar2,MySQL中可以使用varchar,SQLserver可以使用nvarchar,完成对象与<font style="color:red">**关系数据库**</font>之间的映射时,我们往往需要手动转换,如下:

代码1:

```java
//将执行的sql
String sql = "select id, name, password from user";
//创建命令对象
preparedStatement = connection.prepareStatement(sql);
//执行并获得结果集
resultSet = preparedStatement.executeQuery();
//遍历结果集，将数据库中的数据转换成Java中的对象
while(resultSet.next()){
      String name = resultSet.getString("name");
      int id = resultSet.getInt("id");
      String password = resultSet.getString("password");
      User entity= new User(name,id,age,password);
      Users.add(entity);
}
```

很明显,这些代码是重复的且不好维护的,ORM的出现是的上述的过程可以自动化

## 二, MyBatis中ORM的体现

![image_2023-03-18-20-17-19](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-18-20-17-19.png)

简单的说,==ORM是通过使用描述对象和数据之间映射的元数据,将程序中的对象自动持久化到关系数据库中== 

--------

在MyBatis中体现就是我们常说的"结果集映射",可以说是一种半自动化的ORM,因为必须要自己写sql

代码2:

```java
<select id="selUserByID" resultType="stdpei.pojo.User">
        select id ,name, passwd as pwd from `user` 
</select>
```

<font style="color:red">**代码2**</font>就可以取代<font style="color:red">**代码1**</font>,这就是MyBatis给我们提供的ORM,不需要我们去写结果集的映射,当然要保证User中的属性名和user中的字段名相同,才可以[^不相同也有其解决方式]

--------

由于ORM可以**自动对Entity对象与数据库中的Table进行**<font style="color:red">**字段**</font>**与**<font style="color:red">**属性的映射**</font>,所以我们实际可能已经**不需要一个专用的,庞大的数据访问层** 

ORM提供了对数据库的映射,不用sql直接编码,**能够像操作对象一样从数据库获取数据** 

## 三, 核心原则

- **简单: 以最基本的形式建模数据** 

- **传达性: 数据库结构被任何人都能理解的语言文档化** 

- **准确性: 基于数据模型创建正确标准化了的结构** 

## 四, ORM的优缺点

1. **会牺牲程序的执行效率** 

因为ORM是在原本的基础上加了一层,数据完全的面向对象,我们都知道,实现业务需求所经历的层数越多,那么效率就往往越低

2. **固定思维模式** 

对于"框架"而言,往往都是"约定大于配置",因为是通过配置文件实现的业务逻辑,所以个性化操作往往很受限制,我们要遵循设计者的思维模式

3. **存在一般都有性能问题** 

在对对象做持久化时,ORM一般会持久化所有的属性,如果用上了ORM,程序员很有可能将全部的数据提取到内存对象中,然后再进行过滤和加工处理,这样就容易产生性能问题

## 五, 总结

数据库中的一张表,往往对应着面向对象中的一个实体类.

在没有ORM时,我们需要手动写一些数据库中的字段与实体类中的属性的映射关系,这个操作是重复且不好维护的.

ORM出现之后,我们就省去了手动写映射关系的代码,我们只需要注重实体类以及sql语句的即可.

