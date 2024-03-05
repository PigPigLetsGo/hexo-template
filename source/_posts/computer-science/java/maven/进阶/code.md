步骤:

1. 创建Maven模块

![![image_2023-03-06-20-43-36](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-06-20-43-36_20230307151537.png)](1_md_files/image_2023-03-06-20-43-36_20230307151537.png?v=1&type=image&token=V1:DIsRhdjvNviTbTFooiFlzit-cdR6S96AfyNXDQ2XGYw)

2. 将其import到该模块中

![![image_2023-03-06-20-44-31](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-06-20-44-31_20230307151549.png)](1_md_files/image_2023-03-06-20-44-31_20230307151549.png?v=1&type=image&token=V1:bkeMrVbVLWzz2sG-MUr5ifVT5Y32YN_egoarDo08jrE)

3. 选择模块

![![image_2023-03-06-20-44-59](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-06-20-44-59_20230307151600.png)](1_md_files/image_2023-03-06-20-44-59_20230307151600.png?v=1&type=image&token=V1:cGqp6eBvUwb0yoRYx6LojUubkb9B4orWt6T4CpP54_0)

4. 选择导入为Maven项目并finish

![![image_2023-03-06-20-45-26](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-06-20-45-26_20230307151613.png)](1_md_files/image_2023-03-06-20-45-26_20230307151613.png?v=1&type=image&token=V1:6kUxZR8V6urZoBnrfPPykDW5KLUpcg2Quvds7FYkjBM)

5. 将ssm模块中的domain给剪切到pojo模块中

![![image_2023-03-06-20-46-40](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-06-20-46-40_20230307151624.png)](1_md_files/image_2023-03-06-20-46-40_20230307151624.png?v=1&type=image&token=V1:nOlJeYZ1UVmeUIgrPjjcPrivdsFPN7kvjGH_eGLjKVA)

6. 配置ssm的pom文件,将pojo模块的坐标导入到该模块中来使用domain层的类

![![image_2023-03-06-20-48-07](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-06-20-48-07_20230307151635.png)](1_md_files/image_2023-03-06-20-48-07_20230307151635.png?v=1&type=image&token=V1:CwB4gAptryoo0Jn_KJ_DoYJtOl2zrUENzUyMfWV5NUg)

7. 同样的操作创建dao模块后将其导入坐标到ssm模块的pom中

![![image_2023-03-06-20-49-10](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-06-20-49-10_20230307151647.png)](1_md_files/image_2023-03-06-20-49-10_20230307151647.png?v=1&type=image&token=V1:iYMmkksju96HqeAG4qMjZeOwjRykLLaAfkVwAEej4NY)

8. 在dao模块中dao层需要使用到mybatis和jdbc的一些操作还有在BookDao中使用到了Book实体类,所以我们需要将这些东西都导入到dao模块的pom中

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.dkx.spring</groupId>
  <artifactId>maven-springssm-dao</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>maven-springssm-dao</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.11</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.6</version>
    </dependency>

    <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.28</version>
    </dependency>

    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.8</version>
    </dependency>

    <dependency>
      <groupId>com.dkx.spring</groupId>
      <artifactId>maven-springssm-pojo</artifactId>
      <version>1.0-SNAPSHOT</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>true</filtering>
      </resource>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>true</filtering>
      </resource>
    </resources>
  </build>
</project>
```

- 因为依赖关系,先将pojo模块下载到本地mvn Install,再将dao模块进行Install然后再对ssm模块进行compile就不会报错了,否则会报错找不到本地依赖jar包
