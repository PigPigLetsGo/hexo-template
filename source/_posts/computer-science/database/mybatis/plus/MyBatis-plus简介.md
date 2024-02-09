---
title: MyBatisPlus
categories:
  - [计算机学科,database,mybatis]
tags:
  - 计算机学科
  - database
  - mybatis
---

# MyBatis-Plus简介

- MyBatisPlus(简称MP)是基于MyBatis框架基础上开发的增强型工具,指在简化开发,提高效率

- 开发方式
    - 基于MyBatis使用MyBatisPlus
    - 基于Spring使用MyBatisPlus
    - <font style="color:red">基于SpringBoot使用MyBatisPlus</font>
    
1. 创建新模块,选择Spring初始化,并配置模块相关基础信息

![image_2023-03-11-16-59-42](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-11-16-59-42.png)

2. 选择当前模块需要使用的技术集(仅保留JDBC)

![image_2023-03-11-17-00-30](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-11-17-00-30.png)

3. 手工添加MyBatis-Plus起步依赖

```xml
		<dependency>
			<groupId>com.baomidou</groupId>
			<artifactId>mybatis-plus-boot-starter</artifactId>
			<version>3.4.1</version>
		</dependency>
```

**注意事项:** 

由于MyBatis-Plus并未被收录到idea的系统内置配置,无法直接选择加入

4. 设置Jdbc参数(application.yml)

```yml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatisplus?characterEncoding=utf-8&serverTimezone=UTC
    username: root
    password: dkx
```

**注意事项:** 

如果使用Druid数据源,需要导入对应坐标

```xml
<dependency>
   <groupId>com.alibaba</groupId>
   <artifactId>druid</artifactId>
   <version>1.2.8</version>
</dependency>
```

5. 制作实体类与表结构(类名与表名对应,属性名与字段名对应,否则查询出错)

```sql
create table plus (
    `id` bigint(20) primary key auto_increment,
    `name` varchar(30) not null,
    `password` varchar(30) not null,
    `age` int(3) not null,
    `tel` varchar(30)
)character set utf8;
```

```java
public class Plus {
    private Long id;
    private String  name;
    private String password;
    private Integer age;
    private String tel;

    public Plus(Long id, String name, String password, Integer age, String tel) {
        this.id = id;
        this.name = name;
        this.password = password;
        this.age = age;
        this.tel = tel;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getTel() {
        return tel;
    }

    public void setTel(String tel) {
        this.tel = tel;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", password='" + password + '\'' +
                ", age=" + age +
                ", tel='" + tel + '\'' +
                '}';
    }
}
```

6. 定义数据接口,继承<font style="color:red">**BaseMapper<Plus>**</font>

```java
@Mapper
public interface UserMapper extends BaseMapper<Plus> {
}
```

7. 测试类中注入dao接口,测试功能

```java
@SpringBootTest
class MavenMybstisplusDemoApplicationTests {
	@Autowired
	private UserMapper userMapper;
	@Test
	void contextLoads() {
		List<Plus> list = userMapper.selectList(null);
		System.out.println(list);
	}
}
-------------------------RUN----------------------------
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.9)

2023-03-11 16:57:06.629  INFO 15900 --- [           main] c.m.MavenMybstisplusDemoApplicationTests : Starting MavenMybstisplusDemoApplicationTests using Java 11.0.16.1 on Administrantor-Dkx with PID 15900 (started by Administrator in E:\apache-maven-3.6.3\maven-work-space\space02\maven-mybstisplus-demo)
2023-03-11 16:57:06.632  INFO 15900 --- [           main] c.m.MavenMybstisplusDemoApplicationTests : No active profile set, falling back to 1 default profile: "default"
 _ _   |_  _ _|_. ___ _ |    _ 
| | |\/|_)(_| | |_\  |_)||_|_\ 
     /               |         
                        3.4.1 
2023-03-11 16:57:12.878  INFO 15900 --- [           main] c.m.MavenMybstisplusDemoApplicationTests : Started MavenMybstisplusDemoApplicationTests in 7.705 seconds (JVM running for 11.473)
2023-03-11 16:57:14.296  INFO 15900 --- [           main] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} inited
[User{id=1, name='Tom', password='tom', age=3, tel='12987345'}, User{id=2, name='Jerry', password='jerry', age=4, tel='1237568324'}, User{id=3, name='Jock', password='123456', age=41, tel='867862342'}, User{id=4, name='小日子-刘桑', password='it-小日子-刘桑', age=15, tel='912837893759'}]
2023-03-11 16:57:16.463  INFO 15900 --- [ionShutdownHook] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} closing ...
2023-03-11 16:57:16.473  INFO 15900 --- [ionShutdownHook] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} closed

Process finished with exit code 0
```

## MyBatisPlus特性

- 无侵入:只做增强不做改变,不会对现有工程产生影响

- 强大的CRUD操作:内置通用Mapper,少量配置即可实现单表CRUD操作

- 支持Lambda:编写查询条件无需担心字段写错

- 支持主键自动生成

- 内置分页插件

- ...

### 标准数据层开发

#### 标准数据层CRUD功能

| 功能       | 自定义接口                            | MP接口                                       |
|------------|---------------------------------------|----------------------------------------------|
| 新增       | boolean save(T t)                     | int insert(T t)                              |
| 删除       | boolean delete(int id)                | int deleteById(Serializable id)              |
| 修改       | boolean update(T t)                   | int updateById(T t)                          |
| 根据id查询 | T getById(int id)                     | T selectById(Serializable id)                |
| 查询全部   | List<T> getAll()                      | List<T> selectList()                         |
| 分页查询   | PageInfo<T> getAll(int page,int size) | IPage<T> selectPage(IPage<T> page)           |
| 按条件查询 | List<T> getAll(Condition condition)   | IPage<T> selectPage(Wrapper<T> queryWrapper) |

- 案例

- dao

```java
@Mapper
public interface UserMapper extends BaseMapper<Plus> {
}
```

- domain

```java
@Data
public class Plus {
    private Long id;
    private String  name;
    private String password;
    private Integer age;
    private String tel;
}
```

- 启动类

[不展示了]

- 测试代码

```java
@SpringBootTest
class MavenMybstisplusDemoApplicationTests {
	@Autowired
	private UserMapper userMapper;

	/**
	 * 添加数据
	 */
	@Test
	void insertTest(){
		Plus plus = new Plus();
		plus.setName("大佐-刘桑");
		plus.setPassword("刘桑");
		plus.setAge(123);
		plus.setTel("967487236");
		int i = userMapper.insert(plus);
		String msg = i > 0 ? "执行成功":"执行失败";
		System.out.println(msg);
	}

	/**
	 * 通过id删除数据
	 */
	@Test
	void deleteTest(){
		int i = userMapper.deleteById(1634506345139843073L);
		String msg = i > 0 ? "执行成功":"执行失败";
		System.out.println(msg);
	}

	/**
	 * 通过id修改数据
	 */
	@Test
	void updateTest(){
		Plus plus = new Plus();
		plus.setId(1L);
//		plus.setName("Tom666");
		plus.setName("Tom888");
		plus.setPassword("tom888");
		int i = userMapper.updateById(plus);
		String msg = i > 0 ? "执行成功":"执行失败";
		System.out.println(msg);
	}

	/**
	 * 通过id查询数据
	 */
	@Test
	void selectByIdTest(){
		Plus plus = userMapper.selectById(1L);
		System.out.println(plus);
	}

	/**
	 * 查询所有用户信息
	 */
	@Test
	void selectAll() {
		List<Plus> list = userMapper.selectList(null);
		System.out.println(list);
	}
}
---------------------RUN---------------------
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.9)

2023-03-11 20:26:44.900  INFO 10204 --- [           main] c.m.MavenMybstisplusDemoApplicationTests : Starting MavenMybstisplusDemoApplicationTests using Java 11.0.16.1 on Administrantor-Dkx with PID 10204 (started by Administrator in E:\apache-maven-3.6.3\maven-work-space\space02\maven-mybstisplus-demo)
2023-03-11 20:26:44.902  INFO 10204 --- [           main] c.m.MavenMybstisplusDemoApplicationTests : No active profile set, falling back to 1 default profile: "default"
 _ _   |_  _ _|_. ___ _ |    _ 
| | |\/|_)(_| | |_\  |_)||_|_\ 
     /               |         
                        3.4.1 
2023-03-11 20:26:50.892  INFO 10204 --- [           main] c.m.MavenMybstisplusDemoApplicationTests : Started MavenMybstisplusDemoApplicationTests in 7.226 seconds (JVM running for 10.072)
2023-03-11 20:26:52.148  INFO 10204 --- [           main] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} inited
Plus(id=1, name=Tom888, password=tom888, age=3, tel=12987345)
[Plus(id=1, name=Tom888, password=tom888, age=3, tel=12987345), Plus(id=2, name=Jerry, password=jerry, age=4, tel=1237568324), Plus(id=3, name=Jock, password=123456, age=41, tel=867862342), Plus(id=4, name=小日子-刘桑, password=it-小日子-刘桑, age=15, tel=912837893759), Plus(id=1634531132654465025, name=大佐-刘桑, password=刘桑, age=123, tel=967487236)]
执行成功
执行成功
执行成功
2023-03-11 20:26:54.572  INFO 10204 --- [ionShutdownHook] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} closing ...
2023-03-11 20:26:54.582  INFO 10204 --- [ionShutdownHook] com.alibaba.druid.pool.DruidDataSource   : {dataSource-1} closed

Process finished with exit code 0
```

## Service CRUD 接口

文档入口：https://baomidou.com/pages/49cc81/#service-crud-%E6%8E%A5%E5%8F%A3

介绍入口：https://blog.csdn.net/qq_44869121/article/details/124297999

>  <span alt='solid'>说明</span>：
>
>  -  通用Service CRUD 封装IService接口，进一步封装CRUD采用 `get`查询 `remove`删除 `list`查询集合 `page`分页 前缀命名方式区分 `Mapper`层避免混淆
>  -  泛型 `T` 为任意实体对象
>  -  建议如果存在自定义通用service方法的可能，请创建自己的`IBaseService`继承`Mybatis-plus`提供的基类
>  -  对象`Wrapper`为 条件构造器

**使用方式**：

1.  在service接口类中集成IService<实体类> 

    ```java 
    package com.dkx.system.service;
    
    import com.atguigu.model.system.SysRole;
    import com.baomidou.mybatisplus.extension.service.IService;
    
    public interface SysRoleService extends IService<SysRole> {
    }
    ```

2.  在serviceImpl实现类中集成ServiceImpl并实现service接口类

    ```java
    package com.dkx.system.service.impl;
    
    import com.atguigu.model.system.SysRole;
    import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
    import com.dkx.system.mapper.SysRoleMapper;
    import com.dkx.system.service.SysRoleService;
    import org.springframework.stereotype.Service;
    
    @Service
    public class SysRoleServiceImpl extends ServiceImpl<SysRoleMapper, SysRole> implements SysRoleService {
    }
    ```

<center>图片解释</center>

![image-20230904111312304](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309041113608.png)

测试代码：

```java
package com.dkx.system;

import com.atguigu.model.system.SysRole;
import com.dkx.system.service.SysRoleService;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest
public class SysRoleServiceTest {
    @Autowired
    private SysRoleService sysRoleService;
    @Test
    public void testList () {
        List<SysRole> list = sysRoleService.list();
        for (SysRole item : list) {
            System.out.println(item);
        }
    }
}
```

结果：

![image-20230904111430807](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202309041114047.png)

具体里面的一些操作的CRUD方法看官方文档。
