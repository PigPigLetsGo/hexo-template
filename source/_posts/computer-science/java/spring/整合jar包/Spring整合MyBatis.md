---
title: Spring整合MyBatis
categories:
    - [计算机学科,java,spring,整合jar包]
tags:
    - 计算机学科
    - spring
    - 整合jar包
    - Mybatis
---

# Spring整合MyBatis

将Mybatis原来的配置文件:Mybatis-config.xml替换成了Spring来整合它

![image_2023-02-26-11-33-42](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-11-33-42_20230226113504.png)

1.创建目录

![image_2023-02-26-11-42-11](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-11-42-11_20230226115722.png)

2.创建对应的类

![image_2023-02-26-11-42-54](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-02-26-11-42-54_20230226115651.png)

代码

## config

- JdbcConfig

```java
@SuppressWarnings("all")
public class JdbcConfig {
//    使用简单类型，通过配置文件properties注入数据
    @Value("${driver}")
    private String driver;
    @Value("${jdbcUrl}")
    private String url;
    @Value("${a.username}")
    private String ausername;
    @Value("${password}")
    private String password;
//    返回为bean
    @Bean
    public DataSource getDataSource() {
        DruidDataSource d = new DruidDataSource();
//        将成员变量的值赋值到DruidDataSource中
        d.setDriverClassName(driver);
        d.setUrl(url);
        d.setUsername(ausername);
        d.setPassword(password);
//        返回对象
        return d;
    }
}
```

- MybatisConfig

```java
@SuppressWarnings("all")
public class MybatisConfig {
//    返回为bean
    @Bean
    public SqlSessionFactoryBean getSqlSessionFactory(DataSource d){
//        获取SqlSession工厂实例化Bean
        SqlSessionFactoryBean sqlsession = new SqlSessionFactoryBean();
//        设置别名
        sqlsession.setTypeAliasesPackage("com.dkx.spring.domain");
//        设置数据库信息
        sqlsession.setDataSource(d);
        return sqlsession;
    }
//    返回为bean
    @Bean
    public MapperScannerConfigurer mappersacnnerconfigurer(){
//        获取设置Mappers中mapper的标签值的对象
        MapperScannerConfigurer mapper = new MapperScannerConfigurer();
//        设置路径指定dao层的Mapper接口
        mapper.setBasePackage("com.dkx.spring.dao");
        return mapper;
    }
}
```

- SpringConfig

```java
@SuppressWarnings("all")
//配置该类为配置类
@Configuration
//扫描路径，加载其它配置bean
@ComponentScan("com.dkx.spring")
//加载配置文件
@PropertySource("classpath:druid.properties")
//导入配置类
@Import({JdbcConfig.class,MybatisConfig.class})
public class SpringConfig {
}
```

## dao

- UserMapper

```java
@SuppressWarnings("all")
public interface UserMapper {
    @Insert("insert into spring_mybatis (id,name,money) values (#{id},#{name},#{money})")
    int save(SpringMybatis s);
    @Delete("delete spring_mybatis where id = #{id}")
    int delete();
    @Update("update from spring_mybatis set name = #{name} where id = #{id}")
    int update();
    @Select("select * from user")
    List<SpringMybatis> getList();
    @Select("select * from user where id = #{id}")
    SpringMybatis getUserById(Integer id);
}
```

## domian

- SpringMybatis

```java
@SuppressWarnings("all")
public class SpringMybatis {
   private Integer id;
   private String name;
   private Integer money;
   public SpringMybatis() {
   }

   public SpringMybatis(Integer id, String name, Integer money) {
      this.id = id;
      this.name = name;
      this.money = money;
   }

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

   public Integer getMoney() {
      return money;
   }

   public void setMoney(Integer money) {
      this.money = money;
   }

   @Override
   public String toString() {
      return "SpringMybatis{" +
              "id=" + id +
              ", name='" + name + '\'' +
              ", money=" + money +
              '}';
   }
}
```

## service

```java
@SuppressWarnings("all")
public interface UserService {
    void save(SpringMybatis s);
    void delete();
    void update();
    List<SpringMybatis> getList();
    SpringMybatis getUserById(Integer id);
}
```

### impl

```java
@SuppressWarnings("all")
//配置bean
@Service
public class UserServiceImpl implements UserService {
//    自动装配
    @Autowired
    private UserMapper userMapper;

    public void save(SpringMybatis s){
        userMapper.save(s);
    }
    public void delete(){
        userMapper.delete();
    }
    public void update(){
        userMapper.update();
    }
    public List<SpringMybatis> getList(){
        return userMapper.getList();
    }
    public SpringMybatis getUserById(Integer id){
        return userMapper.getUserById(id);
    }
}
```

## resources

```properties
driver=com.mysql.cj.jdbc.Driver
jdbcUrl=jdbc:mysql://localhost:3306/spring_mybatis?characterEncoding=utf-8&serverTimezone=UTC
a.username=root
password=dkx
```

- 测试代码

```java
    public void test2(){
        ApplicationContext c = new AnnotationConfigApplicationContext(SpringConfig.class);
        UserService service = c.getBean(UserService.class);
        List<SpringMybatis> list = service.getList();
        for(SpringMybatis i:list){
            System.out.println(i);
        }
    }
```
