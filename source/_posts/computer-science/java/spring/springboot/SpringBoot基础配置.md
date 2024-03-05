---
title: SpringBoot基础配置
categories:
   - [计算机学科,java,spring,springboot]
tags:
   - 计算机学科
   - springboot
   - 基础
---

# 基础配置

## 配置格式

- SpringBoot提供了多种属性配置方式

1. application.properties

```properties
server.port=80
```

2. application.yml(主写)

```yml
server:
  port: 81
```

3. application.yaml

```yaml
server:
  port: 82
```

## 三个配置文件的优先级

<font style="color:red">properties</font> --> <font style="color:red">yml</font> --> <font style="color:red">yaml</font>

## 自动提示功能消失解决方案 

1. 打开Project Structure... (快捷键 : shift+ctrl+alt+s)

![image_2023-03-09-10-25-53](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856933.png)

2. 添加yaml文件

![image_2023-03-09-10-27-27](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856810.png)

![image_2023-03-09-10-28-48](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856502.png)

3. 如果图标没有改变则将Idea的代码智能提示打开,想关闭就再将其关闭

<font style="color:red">**注意事项:**</font>  

SpringBoot核心配置文件名为application

SpringBoot内置属性过多,且所有属性集中在一起修改,在使用时,通过提示键+关键字修改属性

## 不同的运行级别

-  默认为:info

```yml
#设置运行级别
logging:
  level:
    root: info
```

Run console result

![image_2023-03-09-10-40-01](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856124.png)

- warn运行级别

```yml
#设置运行级别
logging:
  level:
    root: warn
```

Run console result

![image_2023-03-09-10-36-49](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856068.png)

- debug运行级别

```yml
#设置运行级别
logging:
  level:
    root: debug
```

Run console result

![image_2023-03-09-10-39-00](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856929.png)

## yaml

- YAML(YAML Ain't Markup Language),一种数据序列化格式

- 优点:
    - 容易阅读
    - 容易与脚本语言交互
    - 以数据为核心,重数据轻格式

- YAML文件扩展名
    - <font style="color:red">.yml(主流)</font>
    - .yaml

![image_2023-03-09-10-50-43](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856485.png)

### yaml语法规则

- 大小写敏感

- 属性层级关系使用多行描述,每行结尾使用冒号结束

- 使用缩进表示层级关系,同层级左侧对齐,只允许使用空格(不允许使用Tab键)

- 属性值前面添加空格(属性名与属性值之间使用冒号+空格作为分隔)

- # 表示注释

- 核心规则:<font style="color:red">**数据前面要加空格与冒号隔开**</font>

### yaml数组数据

- 数组数据在数据书写位置的下方使用减号(-)作为数据开始符号,每行书写一个数据,减号(-)与数据间空格分隔

![image_2023-03-09-11-09-16](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856647.png)

### yaml数据读取

#### @EnableConfigurationProperties

作用 :**使使用@ConfigurationProperties注解的类生效** 

- 说明

>  如果一个配置类只配置@ConfigurationProperties注解,而没有配置@Component,那么在IoC容器中是获取不到properties配置文件转化的bean,说白了@EnableConfigurationProperties相当于把使用@ConfigurationProperties的类进行了一次注入

#### @ConfigurationProperties(prefix = "yml中的数组属性名称")

作用 **:配置@Value注解用于获取配置文件中的属性定义并绑定到JavaBean或属性中** 

- 使用@Value读取单个数据,属性名引用方式:${一级属性名,二级属性名,...}

- 单级的直接使用属性名称,多级的使用`.` 来连接属性名称,数组使用[]索引来获取哪一个

```yml
lesson: SpringBoot
server:
  port: 80
logging:
  level:
    root: warn
enterprise:
  name: 刘桑
  age: 18
  tel: 1231256
  subject:
    - Java
    - MySQL
    - Redis
    - Nginx
    - Maven
    - Nexus
    - 小日子-刘桑
    - 刘桑
```

- BookController

```java
@SuppressWarnings("all")
@RestController
@RequestMapping("/books")
public class BookController {
   @Value("${lesson}")
   private String lesson;
   @Value("${enterprise.name}")
   private String name;
   @Value("${enterprise.age}")
   private String age;
   @Value("${enterprise.tel}")
   private String tels;
   @Value("${enterprise.subject[0]}")
   private String subject_0;
   @GetMapping("/{id}")
   public String getById(@PathVariable Integer id){
      System.out.println("id ==> "+id);
      System.out.println(lesson);
      System.out.println(name);
      System.out.println(age);
      System.out.println(tels);
      System.out.println(subject_0);
      return "bookcontroller getbyid running...";
   }
}
------------------------Run console result--------------------------

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.9)

id ==> 1
SpringBoot
刘桑
18
1231256
Java
```

- 封装全部数据到Environment对象

```java
@SuppressWarnings("all")
@RestController
@RequestMapping("/books")
public class BookController {
   @Autowired
   private Environment environment;
   @GetMapping("/{id}")
   public String getById(@PathVariable Integer id){
      System.out.println("id ==> "+id);
      System.out.println(environment.getProperty("enterprise.name"));
      return "bookcontroller getbyid running...";
   }
}
-----------------------------Run console result------------------------------
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.9)

id ==> 1
刘桑
```

- 自定义对象封装指定数据
    - 创建封装数据类:Enterprise

```java
@SuppressWarnings("all")
@Component
//在加载配置类中(启动类)中配置@EnableConfigurationProperties({Enterprise.class})
@ConfigurationProperties(prefix = "enterprise")
public class Enterprise {
    private String name;
    private Integer age;
    private String tel;
    private String[] subject;


    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
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

    public String[] getSubject() {
        return subject;
    }

    public void setSubject(String[] subject) {
        this.subject = subject;
    }

    @Override
    public String toString() {
        return "Enterprise{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", tel='" + tel + '\'' +
                ", subject=" + Arrays.toString(subject) +
                '}';
    }
}
```

```java
@SuppressWarnings("all")
@RestController
@RequestMapping("/books")
public class BookController {
   @Autowired
   private Enterprise enterprise;
   @GetMapping("/{id}")
   public String getById(@PathVariable Integer id){
      System.out.println("id ==> "+id);
      System.out.println(enterprise);
      return "bookcontroller getbyid running...";
   }
}
----------------------------Run console result---------------------------------
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.9)

id ==> 1
Enterprise{name='刘桑', age=18, tel='1231256', subject=[Java, MySQL, Redis, Nginx, Maven, Nexus, 小日子-刘桑, 刘桑]}
```

- 可能出现的报错,以下是解决方式

![image_2023-03-09-19-25-48](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856717.png)

[报错解决方式](./问题/Consider-defining-a-bean-of-type-java-lang-String-in-your-configuration.md)

## 多环境开发配置

![image_2023-03-09-20-27-21](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856876.png)

### yml

yml格式的多环境启动配置

```yml
logging:
  level:
    root: info

#设置启动环境
spring:
  profiles:
   active: prod

---
#开发
spring:
  profiles: dev
server:
  port: 81

---
#测试
spring:
  profiles: test
server:
  port: 82

---
#生产
spring:
  profiles: prod
server:
  port: 83
```

- 过时格式与不过时格式

![image_2023-03-10-08-27-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856862.png)

### properties

文件多环境启动

多文件区分环境

- 主启动配置文件application.properties

```properties
#设置启动环境
spring.profiles.active=test
```

- 环境分类配置文件application-prod.properties

```properties
#设置启动端口号
server.port=8082
```

- 环境分类配置文件application-dev.properties

```properties
#设置启动端口号
server.port=8081
```

- 环境分类配置文件application-test.properties

```properties
#设置启动端口号
server.port=8083
```

### 多环境启动命令格式

打包前需要注意:

在settings中设置Encoding中的编码为utf-8否则配置文件中的中文会出现乱码

![image_2023-03-10-09-45-40](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856188.png)

- 带参数启动SpringBoot

```
java -jar .\maven-springboot-demo13-1.0-SNAPSHOT.jar --spring.profiles.active=dev
```

- 临时改参数(端口号)

```
java -jar .\maven-springboot-demo13-1.0-SNAPSHOT.jar --server.port=8888
```

### 多环境开发控制

- Maven为主

- SpringBoot为辅

![image_2023-03-10-10-42-01](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856663.png)

1. Maven中设置多环境属性

```xml
<!--    设置启动环境-->
    <profiles>
        <profile>
            <id>dev</id>
            <properties>
                <profile.active>dev</profile.active>
            </properties>
        </profile>
        <profile>
            <id>pro</id>
            <properties>
                <profile.active>pro</profile.active>
            </properties>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
        </profile>
        <profile>
            <id>test</id>
            <properties>
                <profile.active>test</profile.active>
            </properties>
        </profile>
    </profiles>
```

2. SpringBoot中引用Maven属性

```yml
#设置启动环境
spring:
  profiles:
   active: ${profile.active}

---
#开发
spring:
  profiles: dev
server:
  port: 81

---
#测试
spring:
  profiles: test
server:
  port: 82

---
#生产
spring:
  profiles: prod
server:
  port: 83
```

yml读取maven中设置的profile.active启动以后就会加载profile.active这个属性名

![image_2023-03-10-15-18-50](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856535.png)

3. 执行Maven打包指令,生成了对应的包,其中类参与编译,但是配置文件并没有编译,而是复制到包中

![image_2023-03-10-15-20-20](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856932.png)

- 解决思路:对于源码中非java类的操作要求加载Maven对应的属性,解析${}占位符

4. 对资源文件开启对默认占位符的解析

```xml
<build>
<plugins>
<!--            对资源文件处理的插件-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>3.2.0</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                    <useDefaultDelimiters>true</useDefaultDelimiters>
                </configuration>
            </plugin>
</plugins>
</build>
```

- Maven打包加载到属性,打包顺利通过

![image_2023-03-10-15-24-02](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856040.png)

### 配置文件分类

- SpringBoot中4级配置文件

- file[系统目录]

- classpath[类路径]

1级: file : config/application.yml[最高]

2级: file : application.yml

3级: classpath : config/application.yaml

4级: classpath : application.yml[最低]

- 作用
    - 1级与2级保留系统打包后设置通用属性(用于上线服务)
    - 3级与4级用于系统开发阶段设置通用属性(用于开发服务)

-  在编译后的target目录也就是与jar包同级目录中写application.yml那么这个配置文件就会覆盖config/applicatin.yml和application.yml

![image_2023-03-10-15-50-01](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040856029.png)

- 在target目录中创建config进入到config中创建子目录(2.5.0的一个BUG必须创建因为不创建启动时会报错)在子目录aaa中创建application.yml此时这个配置文件为最高优先级

![image_2023-03-10-16-11-37](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202310040857908.png)

### SpringBoot开启或关闭Logo

-  该方式只对启动类 启动生效,对Test不生效,Test需要在yml中进行

- 在启动类中设置

```java
@SpringBootApplication
public class App {
    public static void main( String[] args ) {
        SpringApplication app = new SpringApplication(App.class);
        app.setBannerMode(Banner.Mode.CONSOLE);//输出到控制台
        app.setBannerMode(Banner.Mode.LOG);//输出到日志中
        app.setBannerMode(Banner.Mode.OFF);//关闭LOGO
        app.run(args);
    }
}
```

### 设置SpringBoot的Loggo

- 在resources目录中创建banner.txt

```txt
               ${AnsiColor.BRIGHT_GREEN}$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
               ${AnsiColor.BRIGHT_YELLOW}$$                                _.ooOoo._                               $$
               ${AnsiColor.BRIGHT_RED}$$                               o888888888o                              $$
               ${AnsiColor.BRIGHT_CYAN}$$                               88"  .  "88                              $$
               ${AnsiColor.BRIGHT_MAGENTA}$$                               (|  ^_^  |)                              $$
               ${AnsiColor.BRIGHT_GREEN}$$                               O\   =   /O                              $$
               ${AnsiColor.BRIGHT_RED}$$                            ____/`-----'\____                           $$
               ${AnsiColor.BRIGHT_CYAN}$$                          .'  \\|       |$$  `.                         $$
               ${AnsiColor.BRIGHT_MAGENTA}$$                         /  \\|||   :   |||$$  \                        $$
               ${AnsiColor.BRIGHT_GREEN}$$                        /  _|||||  -:-  |||||-  \                       $$
               ${AnsiColor.BRIGHT_YELLOW}$$                        |   | \\\   -   $$/ |   |                       $$
               ${AnsiColor.BRIGHT_GREEN}$$                        | \_|  ''\-----/''  |   |                       $$
               ${AnsiColor.BRIGHT_YELLOW}$$                        \  .-\___  `-`  ____/-. /                       $$
               ${AnsiColor.BRIGHT_CYAN}$$                      ___`. .'   /--.--\   `. . ___                     $$
               ${AnsiColor.BRIGHT_RED}$$                    ."" '<  `.____\_<|>_/____.'  >'"".                  $$
               ${AnsiColor.BRIGHT_GREEN}$$                  | | :  `- \`.;`.\ _ /``;.`/ - ` : | |                 $$
               ${AnsiColor.BRIGHT_YELLOW}$$                  \  \ `-.   \_ ___\ /___ _/   .-` /  /                 $$
               ${AnsiColor.BRIGHT_CYAN}$$            ========`-.____`-.____\_____/____.-`____.-'========         $$
               ${AnsiColor.BRIGHT_MAGENTA}$$                                  `=---='                               $$
               ${AnsiColor.BRIGHT_YELLOW}$$            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^        $$
               ${AnsiColor.BRIGHT_GREEN}$$                     佛祖保佑          永无BUG         永不修改         $$
               ${AnsiColor.BRIGHT_YELLOW}$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
                                    ${AnsiColor.BRIGHT_YELLOW}Spring Boot: ${spring-boot.formatted-version}
```

- 配置yml

```yml
spring:
  banner:
    location: classpath:banner.txt
```

```
----------------------RUN----------------------
               $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
               $$                                _.ooOoo._                               $$
               $$                               o888888888o                              $$
               $$                               88"  .  "88                              $$
               $$                               (|  ^_^  |)                              $$
               $$                               O\   =   /O                              $$
               $$                            ____/`-----'\____                           $$
               $$                          .'  \\|       |$$  `.                         $$
               $$                         /  \\|||   :   |||$$  \                        $$
               $$                        /  _|||||  -:-  |||||-  \                       $$
               $$                        |   | \\\   -   $$/ |   |                       $$
               $$                        | \_|  ''\-----/''  |   |                       $$
               $$                        \  .-\___  `-`  ____/-. /                       $$
               $$                      ___`. .'   /--.--\   `. . ___                     $$
               $$                    ."" '<  `.____\_<|>_/____.'  >'"".                  $$
               $$                  | | :  `- \`.;`.\ _ /``;.`/ - ` : | |                 $$
               $$                  \  \ `-.   \_ ___\ /___ _/   .-` /  /                 $$
               $$            ========`-.____`-.____\_____/____.-`____.-'========         $$
               $$                                  `=---='                               $$
               $$            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^        $$
               $$                     佛祖保佑          永无BUG         永不修改         $$
               $$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$$
                                    Spring Boot:  (v2.7.9)
Logging initialized using 'class org.apache.ibatis.logging.stdout.StdOutImpl' adapter.
Property 'mapperLocations' was not specified.
 _ _   |_  _ _|_. ___ _ |    _ 
| | |\/|_)(_| | |_\  |_)||_|_\ 
     /               |         
                        3.4.1 
```

关闭LOGO

```yaml
spring:
 main:
  banner-mode: off
```

## 关闭MybatisPlus的Banner

```yaml
mybatis-plus:
	global-config:
		banner: off
```

## SpringBoot配置读取Mapper.xml配置文件

```xml
mybatis:
  mapper-locations: classpath:mapper/*.xml
```

从绝对路径找到resource资源目录中mapper目录下的任何.xml配置文件

## SpringBoot配置MybatisPlus控制台打印日志

```yml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.stdoutimpl
```

## SpringBoot配置读取mybatis-config.xml配置文件

使用SpringBoot配置application.yml来配置mybatis-config.xml配置文件主要用来设置一些mybatis中的设置选项

```xml
mybatis:
  config-location: classpath:mybatis-config.xml
```

配置完成后可以在mybatis-config.xml中配置一些设置选项比如:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--开启驼峰命名-->
        <setting name="mapUnderscoreToCamelCase" value="true" />
    </settings>
</configuration>
```

## SpringBoot配置数据库连接池

```xml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource #可以省略不写
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/demo2?characterEncoding=utf-8&serverTimezone=UTC&useUnicode=true
    username: root
    password: dkx
```

## SpringBoot配置虚拟目录

```xml
server:
	servlet:
		context-path: /hello
```

# 日期格式化

```yml
spring:
	jackson:
		date-format: yyyy-MM-dd HH:mm:ss
		time-zone: GMT+8
```

