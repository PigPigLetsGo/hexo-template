---
title: spring事务简介
categories:
   - [计算机学科,java,spring,事务]
tags:
   - 计算机学科
   - spring
   - 介绍
   - 基础
---

# Spring事务简介

- 事务作用:在数据层保障一系列的数据库操作同成功同失败

- Spring事务作用:在数据层或<font style="color:red">**业务层**</font>保障一系列的数据库操作同成功,同失败

- <font style="color:red">注意:</font>**该注解需要导入的依赖:spring-jdbc** 否则不能导包

pom.xml

```xml
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-jdbc</artifactId>
   <version>5.2.10.RELEASE</version>
</dependency>
```

## @Transactional

- 开启事务注解

- 可以定在方法中,或者类中,如果定义为类上那么这整个类中所有方法都将开启事务

- 将该注解添加到业务层接口中,降低代码的耦合度

- 在JdbcConfig类中定义PlaformTransactionManager的Bean方法(设置事务管理器)

```java
    @Bean
    public PlatformTransactionManager platformTransactionManager(DataSource d){
        DataSourceTransactionManager manager = new DataSourceTransactionManager();
        manager.setDataSource(d);
        return manager;
    }
```

- 在SpringConfig中定义注解@EnableTransactionManagement注解来告诉配置类去找事务的注解方法或类(开启注解式事务驱动)

<font style="color:red">**注意事项**</font> 

Spring注解式事务通常添加在业务层接口中而不会添加到业务层实现类中,降低耦合

注解式事务可以添加到业务方法上表示当前方法开启事务,也可以添加到接口上表示当前接口所有方法开启事务

事务管理器要根据实现技术进行选择,==MyBatis框架使用的是JDBC事务== 

## 代码案例

- config

```java
@SuppressWarnings("all")
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String jdriver;
    @Value("${jdbc.url}")
    private String jurl;
    @Value("${jdbc.username}")
    private String jusername;
    @Value("${jdbc.password}")
    private String jpassword;
    @Bean
    public DataSource getDataSource(){
        DruidDataSource d = new DruidDataSource();
        d.setDriverClassName(jdriver);
        d.setUrl(jurl);
        d.setUsername(jusername);
        d.setPassword(jpassword);
        return d;
    }
    @Bean
    public PlatformTransactionManager platformTransactionManager(DataSource d){
        DataSourceTransactionManager manager = new DataSourceTransactionManager();
        manager.setDataSource(d);
        return manager;
    }
}
```

```java
@SuppressWarnings("all")
public class MybatisConfig {
    @Bean
    public SqlSessionFactoryBean getSqlSession(DataSource d){
        SqlSessionFactoryBean session = new SqlSessionFactoryBean();
        session.setTypeAliasesPackage("com.dkx.spring.pojo");
        session.setDataSource(d);
        return session;
    }
    @Bean
    public MapperScannerConfigurer getMapper(){
        MapperScannerConfigurer mapper = new MapperScannerConfigurer();
        mapper.setBasePackage("com.dkx.spring.dao");
        return mapper;
    }
}
```

```java
@SuppressWarnings("all")
@Configuration
@ComponentScan("com.dkx.spring")
@PropertySource("classpath:druid.properties")
@Import({MybatisConfig.class,JdbcConfig.class})
@EnableTransactionManagement
public class SpringConfig {
}
```

- dao

```java
@SuppressWarnings("all")
@Repository
public interface UserMapper {
    @Select("update trans set monery = monery - #{monery} where name = #{name}")
    void transfer0(@Param("monery")int monery,@Param("name")String o);
    @Select("update trans set monery = monery + #{monery} where name = #{name}")
    void transfer1(@Param("monery")int monery,@Param("name")String o);
}
```

- pojo

```java
对应表结构这里就省略了
```

- service
    - impl

```java
@SuppressWarnings("all")
@Service
public class UserServiceImpl implements UserService {
    @Autowired
    private UserMapper userMapper;
    public void transfer(String p1,String p2,int monery){
        userMapper.transfer0(monery,p1);
        userMapper.transfer1(monery,p2);
    }
}
```

```java
@SuppressWarnings("all")
public interface UserService {
//    开启事务
    @Transactional
    void transfer(String p1,String p2,int monery);
}
```

- 测试代码

```java
@SuppressWarnings("all")
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class SpringUserTest {
    @Test
    public void test(){
        ApplicationContext c = new AnnotationConfigApplicationContext(SpringConfig.class);
        UserService service = c.getBean(UserService.class);
        service.transfer("张三","刘桑",100);
    }
}
```

# Spring事务角色

![image_2023-02-27-20-32-37](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-27-20-32-37_20230227205136.png)

它们都是单独的事务

![image_2023-02-27-20-33-08](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-27-20-33-08_20230227205149.png)

它们在这个整体中是一个事务,成功,都成功,失败,都失败

![image_2023-02-27-20-38-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-27-20-38-05_20230227205201.png)

## 事务管理员

我们将Spring开启的事务叫做<font style="color:red">**事务管理员**</font>

## 事务协调员

将加入到Spring事务的叫做<font style="color:red">**事务协调员**</font>

- 事务角色:
    - 事务管理员:发起事务方,在Spring中通常指带业务层开启事务的方法
       - 事务协调员:加入事务方法,在Spring中通常指代数据层方法,也可以是业务层方法
