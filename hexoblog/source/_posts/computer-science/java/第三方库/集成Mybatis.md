---
title: Java原生工程集成Mybatis

categories:
   - [计算机学科,java,第三方库]
tags:
   - 计算机学科
   - java
   - 第三方库
   - mybatis
---

# Java原生工程集成Mybatis

![img](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/20210531225211611.png)

## MyBatis简介

### ORM概述

ORM (object Relational Mapping) 对象关系映射

![img](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/20210531225228971.png)

常用ORM框架有：hebernate (全自动ORM映射) ，MyBatis (半自动ORM映射)，jpa

## MyBatis介绍

历史：

-  MyBatis本是apache的一个开源项目，名为iBatis
-  2010年这个项目由apache迁移到了google，并且改名为MyBatis
-  2013年迁移到Github

简介：

MyBatis官网地址：http://www.mybatis.org/mybatis-3/

mybatis是一款优秀的持久层框架，他不需要像JDBC繁琐编写代码，只需要开发人员关注（接口+sql）
它采用了简单的xml配置+接口方式实现增删改查，开发时我们只需要关注SQL本身
三 Mybatis快速入门

查询一个表的数据：

## 导入mybatis的jar包到项目中

![image-20240229171025899](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240229171025899.png)

## 实体类

```java
public class Data {
    private Integer id;
    private String name;
    private Integer pId;
    private Integer order;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getpId() {
        return pId;
    }

    public void setpId(Integer pId) {
        this.pId = pId;
    }

    public Integer getOrder() {
        return order;
    }

    public void setOrder(Integer order) {
        this.order = order;
    }

    @Override
    public String toString() {
        return "Data{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", pId=" + pId +
                ", order=" + order +
                '}';
    }
}
```

## UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace 绑定一个对应的 Dao (mapper) 接口 -->
<mapper namespace="UserMapper">
    <!-- id: 重写 UserDao 接口的抽象方法 resultType: 返回类型通过反射找到 -->
    <select id="getAll" resultType="domain.Data">
        <!-- 查询 sql 语句 -->
        select * from data;
    </select>
</mapper>
```

##  mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/tree_data?characterEncoding=utf-8&amp;serverTimezone=UTC"/>
                <property name="username" value="root"/>
                <property name="password" value="dkx.."/>
            </dataSource>
        </environment>
    </environments>

    <!--加载映射文件-->
    <mappers>
        <mapper resource="com/mybatis/mapper/UserMapper.xml"></mapper>
    </mappers>
</configuration>
```

## 测试代码：

```java
public class TestDemoOne {
    @Test
    public void test() throws IOException {
        InputStream resourceAsStream = Resources.getResourceAsStream("com/mybatis-config.xml");
        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = build.openSession();
        List<Data> lists = sqlSession.selectList("UserMapper.getAll");
        for (Data list : lists) {
            System.out.println(list);
        }
        sqlSession.close();
    }
}
```

打印结果：

```
Data{id=1, name='邢台', pId=null, order=null}
Data{id=2, name='沙河市', pId=null, order=null}
Data{id=3, name='河北省', pId=null, order=null}
Data{id=4, name='北京 ', pId=null, order=null}
Data{id=5, name='朝阳', pId=null, order=null}
Data{id=6, name='赞善乡', pId=null, order=null}
```

