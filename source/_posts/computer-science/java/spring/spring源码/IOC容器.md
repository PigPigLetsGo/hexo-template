---
title: IOC容器
categories:
   - [计算机学科,java,spring,spring源码]
tags:
   - 计算机学科
   - spring
   - 源码
---

## IOC底层原理

IOC是什么？

1.  控制反转，把对象创建和对象之间的调用过程，交给Sprign进行管理
2.  使用IOC目的：为了耦合度降低

IOC底层 使用到了,==xml解析==，==工厂模式==，==反射== 做到IOC的流程

画图讲解IOC底层原理：

![image-20230609154447677](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230609154447677.png)

## IOC接口

1.  IOC思想基于IOC容器完成，IOC容器底层就是对象工厂。

2.  Spring提供IOC容器实现两种方式：(两个接口)

    1.  BeanFactory
        -  IOC容器基本实现，是Spring内部的使用接口，不提供开发人员进行使用。
    2.  ApplicationContext
        -  BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员进行使用。
    3.  BeanFactory和ApplicationContext的区别：
        -  BeanFactory
           -  加载配置文件^ClassPathxmlApplicationContext("*.xml")^的时候不会创建对象，在获取对象(使用)^getBean()^的时候才去创建对象。
        -  ApplicationContext
           -  加载配置文件^ClassPathxmlApplicationContext("*.xml")^时候就会把在配置文件对象进行创建。

3.  ApplicationContext接口实现类：

    ![image-20230609160050706](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230609160050706.png)

    -  FileSystemXmlApplicationContext
       -  配置文件路径写为系统盘的路径也就是绝对路径
    -  ClassPathXmlApplicationContext
       -  当前工程的类路径

## IOC操作Bean管理

1，什么是Bean管理(==概念==)

Bean管理指的是两个操作：

1.  Spring创建对象
2.  Spring注入属性

2，Bean管理操作有两种方式

1.  基于XML配置文件方式实现
2.  基于注解方式实现

### IOC操作Bean管理(基于xml方式)

#### 1，基于xml方式创建对象

```xml
<bean id="user" class="com.dkx.UserDao"></bean>
```

1.  在Spring配置文件中，使用bean标签，标签里面添加对应属性，就可以实现对象创建
2.  bean标签有很多属性，介绍常用的属性：
    1.  id属性：唯一标识
    2.  class属性：创建对象类所在的全路径
3.  创建对象时候，默认也是执行无参构造方法完成对象创建

#### 2，基于xml方式注入属性

1，DI：依赖注入，就是注入属性

##### 第一种注入方式：使用set方法进行注入

1.  创建类，定义属性对应的set方法

    ```java
    public class Book {
    //    创建属性
        private String uame;
    //    创建属性对应的set方法
        public void setName(String uame){
            this.uame = uame;
        }
       public void getName(){
            System.out.println("名字: "+uame);
        }
    }
    ```

2.  在Spring配置文件配置对象创建，配置属性注入.

    ```xml
    <!--配置book对象创建-->
    <bean id="book" class="com.dkx.Book">
       <!--属性注入name对应set方法去掉set首字母小写value赋值-->
       <!--多个属性注入则写多个property来进行注入-->
       <property name="name" value="张三"></property>
    </bean>
    ```

3.  测试代码：

    ```java
    public class TestSpring5 {
        @Test
        public void test(){
    //        1.加载spring配置文件
            ApplicationContext context = new ClassPathXmlApplicationContext
                    ("application.xml");
    //        2.获取配置创建的对象
            Book book = context.getBean("book", Book.class);
            book.getName();
        }
    }
    ```

    测试结果：

    ```
    名字: 张三
    ```

##### 第二种注入方式：使用有参构造进行注入

1.  创建类，定义属性，创建属性对应有参构造方法：

    ```java
    public class Orders {
        //TODO aame
        private String aame;
        private String address;
    
        public Orders(String name, String address) {
            this.aame = name;
            this.address = address;
        }
    
        public void getOut(){
            System.out.println("名称: "+aame+"店名: "+address);
        }
    }
    ```

2.  在Spring的配置文件中，进行配置

    ```xml
    <!--有参构造注入属性-->
    <bean id="orders" class="com.dkx.Orders">
       <!--name通过构造器中的参数名来查找的-->
       <constructor-arg name="name" value="张三"></constructor-arg>
       <constructor-arg name="address" value="张三牌-板面"></constructor-arg>
    </bean>
    ```

3.  测试代码：

    ```java
    public class TestSpring5 {
        @Test
        public void test(){
    //        1.加载spring配置文件
            ApplicationContext context = new ClassPathXmlApplicationContext
                    ("application.xml");
    //        2.获取配置创建的对象
            Orders orders = context.getBean("orders", Orders.class);
            orders.getOut();
        }
    }
    ```

    测试结果：

    ```
    名称: 张三店名: 张三牌-板面
    ```

### 3，p名称空间注入(了解)

1.使用p名称空间注入，可以简化基于xml配置方式

第一步，添加p名称空间在配置文件中。

![image-20230609170848984](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230609170848984.png)

第二步，进行属性注入，在bean标签里面进行操作

```xml
<!--set方法注入属性-->
<bean id="book" class="com.dkx.Book" p:name="张三"></bean>
```

测试代码：

```java
public class TestSpring5 {
    @Test
    public void test(){
//        1.加载spring配置文件
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
//        2.获取配置创建的对象
        Book book = context.getBean("book", Book.class);
        book.getName();
    }
}
```

测试结果：

```
名字: 张三
```

### IOC操作Bean管理(xml注入其它类型属性)

#### 1.字面量

1.  null值

    ```java
    public class Book {
    //    创建属性
        private String uame;
        private String address;
    //    创建属性对应的set方法
    
    
        public void setName(String uame) {
            this.uame = uame;
        }
    
        public void setAddress(String address) {
            this.address = address;
        }
    
        public void getName(){
            System.out.println("名字: "+uame+"地址: "+address);
        }
    }
    ```

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:p="http://www.springframework.org/schema/p"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!--set方法注入属性-->
        <bean id="book" class="com.dkx.Book" p:name="张三">
            <property name="address">
                <null/>
            </property>
        </bean>
    </beans>
    ```

    测试代码：

    ```java
    public class TestSpring5 {
        @Test
        public void test(){
    //        1.加载spring配置文件
            ApplicationContext context = new ClassPathXmlApplicationContext
                    ("application.xml");
    //        2.获取配置创建的对象
            Book book = context.getBean("book", Book.class);
            book.getName();
        }
    }
    ```

    测试结果：

    ```
    名字: 张三地址: null
    ```

2.  属性值包含特殊符号

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:p="http://www.springframework.org/schema/p"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <!--set方法注入属性-->
        <!--属性值包含特殊符号
            1.把<>进行转义
            2.把带特殊符号内容写到xml的CDATA语法中
        -->
        <bean id="book" class="com.dkx.Book" p:name="张三">
            <property name="address">
                <value><![CDATA[<<南京>>]]></value>
            </property>
        </bean>
    </beans>
    ```

    测试结果：

    ```
    名字: 张三地址: <<南京>>
    ```

#### 2.注入属性-外部Bean

1.  创建两个类service类和dao类

    ![image-20230609195756054](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230609195756054.png)

2.  在service调用dao里面的方法

    -  service

    ```java
    public class UserService {
        private UserDao userDao;
    
        public void setUserDao(UserDao userDao) {
            this.userDao = userDao;
        }
    
        public void add(){
            userDao.update();
            System.out.println("service add ...");
        }
    }
    ```

    -  dao

    ```java
    public interface UserDao {
        void update();
    }
    ```

    -  daoImpl

    ```java
    public class UserDaoImpl implements UserDao {
        @Override
        public void update(){
            System.out.println("Dao update ...");
        }
    }
    ```

3.  在Spring配置文件中进行配置

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:p="http://www.springframework.org/schema/p"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="userDao" class="com.dkx.dao.impl.UserDaoImpl"></bean>
        <!--
            name属性：类里面set方法去掉set首字母小写的名称setUserDao -> userDao
            ref属性：创建userDao对象bean标签id值
        -->
        <bean id="userService" class="com.dkx.service.UserService">
            <property name="userDao" ref="userDao"></property>
        </bean>
    </beans>
    ```

    测试代码：

    ```java
    public class TestSpring5 {
        @Test
        public void test(){
    //        1.加载spring配置文件
            ApplicationContext context = new ClassPathXmlApplicationContext
                    ("application.xml");
    //        2.获取配置创建的对象
            UserService service = context.getBean("userService", UserService.class);
            service.add();
        }
    }
    ```

    测试结果：

    ```
    Dao update ...
    service add ...
    ```

#### 3.注入属性-外部Bean

1.  一对多关系：部分和员工

一个部门有多个员工，一个员工属于一个部门

部门是一，员工是多

2.  在实体类之间表示一对多关系，员工表示所属部门，使用对象类型属性进行表示

```java
/**
 * 部门类
 */
public class Dept {
    private String dname;

    public void setDname(String dname) {
        this.dname = dname;
    }

    @Override
    public String toString() {
        return dname;
    }
}
```

```java
/**
 * 员工类
 */
public class Emp {
    private String  ename;
    private String gender;
//    员工属于一个部门，使用对象形式表示
    private Dept dept;

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    public void setEname(String ename) {
        this.ename = ename;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public void getOut(){
        System.out.println("名称: "+ename+", 性别: "+gender+", 部门: "+dept);
    }
}
```

3.  在Spring配置文件中进行配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="emp" class="com.dkx.bean.Emp" p:ename="张三" p:gender="男">
        <property name="dept">
            <bean id="dept" class="com.dkx.bean.Dept" p:dname="程序员"></bean>
        </property>
    </bean>
</beans>
```

测试代码：

```java
public class TestSpring5 {
    @Test
    public void test(){
//        1.加载spring配置文件
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
//        2.获取配置创建的对象
        Emp emp = context.getBean("emp", Emp.class);
        emp.getOut();
    }
}
```

测试结果：

```
名称: 张三, 性别: 男, 部门: 程序员
```

####  4.注入属性-级联赋值

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="emp" class="com.dkx.bean.Emp" p:ename="张三" p:gender="男">
        <property name="dept" ref="dept"></property>
    </bean>
    <bean id="dept" class="com.dkx.bean.Dept" p:dname="程序员"></bean>
</beans>
```

第二种写法：

```java
/**
 * 部门类
 */
public class Dept {
    private String dname;

    public void setDname(String dname) {
        this.dname = dname;
    }

    @Override
    public String toString() {
        return dname;
    }
}
```

生成Dept的get方法

```java
/**
 * 员工类
 */
public class Emp {
    private String  ename;
    private String gender;
//    员工属于一个部门，使用对象形式表示
    private Dept dept;

    public Dept getDept() {
        return dept;
    }

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    public void setEname(String ename) {
        this.ename = ename;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public void getOut(){
        System.out.println("名称: "+ename+", 性别: "+gender+", 部门: "+dept);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="emp" class="com.dkx.bean.Emp" p:ename="张三" p:gender="男">
        <!--需要依赖注入dept不能省略否则会报错-->
        <property name="dept" ref="dept"></property>
        <!--注意： 需要在emp中生成dept的get方法否则无法调用-->
        <!--通过emp中的getDept获取dept对象调用dname并value赋值-->
        <property name="dept.dname" value="程序员"></property>
    </bean>
    <bean id="dept" class="com.dkx.bean.Dept"></bean>
</beans>
```

测试代码：

```java
public class TestSpring5 {
    @Test
    public void test(){
//        1.加载spring配置文件
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
//        2.获取配置创建的对象
        Emp emp = context.getBean("emp", Emp.class);
        emp.getOut();
    }
}
```

测试结果：

```
名称: 张三, 性别: 男, 部门: 程序员
```

### IOC操作Bean管理(xml注入集合属性)

#### 1.注入数组类型属性

#### 2.注入List集合类型属性

#### 3.注入Map集合

1.  创建类，定义数组，List，Map，Set类型属性，并且生成对应的Set方法

```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Set;

public class Stu {
//    数组类型属性
    private String[] courses;
//    List集合类型属性
    private List<String> list;
//    Map集合类型属性
    private Map<String,String> map;
//    Set集合类型属性
    private Set<String> set;

    public void setCourses(String[] courses) {
        this.courses = courses;
    }

    public void setList(List<String> list) {
        this.list = list;
    }

    public void setMap(Map<String, String> map) {
        this.map = map;
    }

    public void setSet(Set<String> set) {
        this.set = set;
    }
    public void getOut(){
        System.out.println("courses: "+ Arrays.toString(courses));
        System.out.println("list: "+list);
        System.out.println("map: "+map);
        System.out.println("set: "+set);
    }
}
```

2.  在Spring配置文件中进行配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="stu" class="com.dkx.Stu">
        <!--数组类型属性注入-->
        <property name="courses">
            <array>
                <value>Java课程</value>
                <value>数据库课程</value>
            </array>
        </property>
        <!--List集合属性注入-->
        <property name="list">
            <list>
                <value>list-张三</value>
                <value>list-李四</value>
                <value>list-刘桑</value>
            </list>
        </property>
        <!--Map集合属性注入-->
        <property name="map">
            <map>
                <entry key="第一名" value="刘桑"></entry>
                <entry key="第二名" value="张三"></entry>
                <entry key="第三名" value="李四"></entry>
            </map>
        </property>
        <!--Set集合属性注入-->
        <property name="set">
            <set>
                <value>set-张三</value>
                <value>set-李四</value>
                <value>set-刘桑</value>
            </set>
        </property>
    </bean>
</beans>
```

3.测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
        Stu s = context.getBean("stu", Stu.class);
        s.getOut();
    }
}
```

测试结果：

```
courses: [Java课程, 数据库课程]
list: [list-张三, list-李四, list-刘桑]
map: {第一名=刘桑, 第二名=张三, 第三名=李四}
set: [set-张三, set-李四, set-刘桑]
```

#### 4.在集合中设置对象类型值

1.  创建一个新的类

```java
/**
 * 课程类
 */
public class Course {
    private String cname;

    public void setCname(String cname) {
        this.cname = cname;
    }

    @Override
    public String toString(){
        return cname;
    }
}
```

2.  添加一个值类型是对象的List集合

```java
public class Stu {
//    数组类型属性
    private String[] courses;
//    List集合类型属性
    private List<String> list;
//    Map集合类型属性
    private Map<String,String> map;
//    Set集合类型属性
    private Set<String> set;
//    对象集合类型属性
    private List<Course> courseList;

    public void setCourses(String[] courses) {
        this.courses = courses;
    }

    public void setList(List<String> list) {
        this.list = list;
    }

    public void setMap(Map<String, String> map) {
        this.map = map;
    }

    public void setSet(Set<String> set) {
        this.set = set;
    }

    public void setCourseList(List<Course> courseList) {
        this.courseList = courseList;
    }

    public void getOut(){
        System.out.println("courses: "+ Arrays.toString(courses));
        System.out.println("list: "+list);
        System.out.println("map: "+map);
        System.out.println("set: "+set);
        System.out.println("list-course: "+courseList);
    }
}
```

3.  在Spring中 配置文件进行配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="stu" class="com.dkx.Stu">
        <!--数组类型属性注入-->
        <property name="courses">
            <array>
                <value>Java课程</value>
                <value>数据库课程</value>
            </array>
        </property>
        <!--List集合属性注入-->
        <property name="list">
            <list>
                <value>list-张三</value>
                <value>list-李四</value>
                <value>list-刘桑</value>
            </list>
        </property>
        <!--Map集合属性注入-->
        <property name="map">
            <map>
                <entry key="第一名" value="刘桑"></entry>
                <entry key="第二名" value="张三"></entry>
                <entry key="第三名" value="李四"></entry>
            </map>
        </property>
        <!--Set集合属性注入-->
        <property name="set">
            <set>
                <value>set-张三</value>
                <value>set-李四</value>
                <value>set-刘桑</value>
            </set>
        </property>
        <!--注入List集合类型，值是对象-->
        <property name="courseList">
            <list>
                <ref bean="course1"></ref>
                <ref bean="course2"></ref>
                <ref bean="course3"></ref>
            </list>
        </property>
    </bean>
    <bean id="course1" class="com.dkx.Course" p:cname="学生A"></bean>
    <bean id="course2" class="com.dkx.Course" p:cname="学生B"></bean>
    <bean id="course3" class="com.dkx.Course" p:cname="学生C"></bean>
</beans>
```

4.  测试代码：

    ```java
    public class AppTest {
        @Test
        public void test(){
            ApplicationContext context = new ClassPathXmlApplicationContext
                    ("application.xml");
            Stu s = context.getBean("stu", Stu.class);
            s.getOut();
        }
    }
    ```

    测试结果：

    ```
    courses: [Java课程, 数据库课程]
    list: [list-张三, list-李四, list-刘桑]
    map: {第一名=刘桑, 第二名=张三, 第三名=李四}
    set: [set-张三, set-李四, set-刘桑]
    list-course: [学生A, 学生B, 学生C]
    ```

#### 5.把集合注入部分提取出来

0.  创建Book类

```java
public class Book {
    private List<String> list;

    public void setList(List<String> list) {
        this.list = list;
    }

    public void getOut(){
        System.out.println(list);
    }
}
```

1.  在Spring配置文件中引入名称空间：util 不是 p 了

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
    <!--提取List集合类型属性注入-->
    <util:list id="bookList">
        <value>红楼梦</value>
        <value>山海经</value>
        <value>西游记</value>
    </util:list>
    <!--提取list集合类型属性注入使用-->
    <bean id="book" class="com.dkx.Book">
        <!--name对应set方法-->
        <property name="list" ref="bookList"></property>
    </bean>
</beans>
```

2.  测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application1.xml");
        Book book = context.getBean("book", Book.class);
        book.getOut();
    }
}
```

测试结果：

```
[红楼梦, 山海经, 西游记]
```

### IOC操作Bean管理(FactoryBean)

#### 1.Spring有两种类型bean，一种普通bean，另外一种工厂bean(FactoryBean)

1.  普通bean：在配置文件中定义bean类型就是返回类型

```xml
<bean id="book" class="com.dkx.Book"></bean>
```

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application1.xml");
        Book book = context.getBean("book", Book.class);
    }
}
```

2.  工厂bean：在配置文件定义bean类型可以和返回类型不一样

    1.  第一步：创建类，让这个类作为工厂bean，实现接口FactoryBean

        ```java
        import com.dkx.Course;
        import org.springframework.beans.factory.FactoryBean;
        
        public class MyBean implements FactoryBean<Course> {
           
        }
        ```

    2.  第二步：实现接口里面的方法，在实现的方法中定义返回的bean类型

        ```java
        import com.dkx.Course;
        import org.springframework.beans.factory.FactoryBean;
        
        public class MyBean implements FactoryBean<Course> {
        //    定义返回Bean
            @Override
            public Course getObject() throws Exception {
                Course c = new Course();
                c.setCname("张三");
                return c;
            }
        
            @Override
            public Class<?> getObjectType() {
                return null;
            }
        
            @Override
            public boolean isSingleton() {
                return FactoryBean.super.isSingleton();
            }
        }
        ```

    3.  在Spring配置文件中进行配置

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:p="http://www.springframework.org/schema/p"
               xmlns:util="http://www.springframework.org/schema/util"
               xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                    http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
            <bean id="myBean" class="com.dkx.factorybean.MyBean"></bean>
        </beans>
        ```

    4.  测试代码：

        ```java
        public class AppTest {
            @Test
            public void test(){
                ApplicationContext context = new ClassPathXmlApplicationContext
                        ("application2.xml");
                Course myBean = context.getBean("myBean", Course.class);
                System.out.println(myBean);
            }
        }
        ```

        测试结果：

        ```
        张三
        ```

### IOC操作Bean管理(bean作用域)

1.  在Spring里面，设置创建bean实例是单实例还是多实例

2.  在Spring里面，默认情况下，bean是单实例对象

    ```xml
    <bean id="book" class="com.dkx.Book"></bean>
    ```

    ```java
    public class AppTest {
        @Test
        public void test(){
            ApplicationContext context = new ClassPathXmlApplicationContext
                    ("application1.xml");
            Book book = context.getBean("book", Book.class);
            Book book1 = context.getBean("book",Book.class);
            System.out.println(book);
            System.out.println(book1);
        }
    }
    ```

    结果：两个对象是相同的。

    ```
    com.dkx.Book@fe18270
    com.dkx.Book@fe18270
    ```

3.  如何设置单实例还是多实例

    1.  在Spring配置文件bean标签里面有属性用于设置单实例还是多实例
    2.  scope属性值
        1.  默认值：singleton，表示单实例对象
        2.  可选值：prototype，表示多实例对象

    将配置设置为多实例对象：

    ```xml
    <bean id="book" class="com.dkx.Book" scope="prototype"></bean>
    ```

    测试结果：不是同一个对象了

    ```
    com.dkx.Book@2fd66ad3
    com.dkx.Book@5d11346a
    ```

    3.  singleton和prototype区别：

        1.  singleton单实例，prototype多实例
        2.  设置scope值是singleton时候，加载spring配置文件时候^ClassPathXmlApplicationContext("application1.xml");^就会创建单实例对象
        3.  设置scope值是prototype时候，不是在加载spring配置文件时候创建对象，在调用getBean方法时候创建多实例对象

        ```java
        ApplicationContext context = new ClassPathXmlApplicationContext("application1.xml");
        //到getBean的时候才会创建对象所以每次获取的时候都是不同的对象
        Book book = context.getBean("book", Book.class);
        ```

### IOC操作Bean管理(bean生命周期)

1.  生命周期
    1.  从对象创建到对象销毁的过程
2.  Bean生命周期
    1.  通过构造器创建Bean实例(无参构造)
    2.  为Bean的属性设置值和其它Bean引用(调用set方法)
    3.  调用Bean的初始化的方法(需要进行配置初始化的方法)
    4.  Bean可以使用了(对象获取到了)
    5.  当容器关闭时候，调用Bean的销毁的方法(需要进行配置销毁的方法)
3.  演示Bean生命周期

```java
public class Orders {
    private String oname;

    public Orders(){
        System.out.println("第一步 Orders 无参构造执行了...");
    }

    public void setOname(String oname) {
        this.oname = oname;
        System.out.println("第二步 Orders set方法执行了...");
    }

//    创建初始化执行的方法
    public void initMethod(){
        System.out.println("第三步 Orders initMethod初始化方法执行了...");
    }

//    创建执行的销毁的方法
    public void destroyMethod(){
        System.out.println("第五步 Orders destroyMethod销毁方法执行了...");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="orders" class="com.dkx.bean.Orders" p:oname="手机" init-method="initMethod" destroy-method="destroyMethod"></bean>
</beans>
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext
                ("application3.xml");
        Orders orders = context.getBean("orders", Orders.class);
        System.out.println("第四步 获取创建bean实例对象:=> "+orders);
//        手动让Bean实例销毁
        context.close();
    }
}
```

测试结果：

```
第一步 Orders 无参构造执行了...
第二步 Orders set方法执行了...
第三步 Orders initMethod初始化方法执行了...
第四步 获取创建bean实例对象:=> com.dkx.bean.Orders@8e24743
第五步 Orders destroyMethod销毁方法执行了...
```

4.  Bean的后置处理器，加载后置处理器Bean生命周期共有7步操作

    1.  通过构造器创建Bean实例(无参构造)
    2.  为Bean的属性设置值和其它Bean引用(调用set方法)
    3.  <font style="color:red">把Bean实例传递Bean后置处理器的方法</font>.
        -  初始化前会执行：postProcessBeforeInitialization 方法
    4.  调用Bean的初始化的方法(需要进行配置初始化的方法)
    5.  <font style="color:red">把Bean实例传递Bean后置处理器的方法</font>.
        -  初始化后会执行：postProcessAfterInitialization 方法
    6.  Bean可以使用了(对象获取到了)
    7.  当容器关闭时候，调用Bean的销毁的方法(需要进行配置销毁的方法)

5.  演示添加后置处理器效果

    1.  创建类，实现接口BeanPostProcessor，创建后置处理器

        ```java
        public class MyBeanPost implements BeanPostProcessor {
            @Override
            public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
                System.out.println("在初始化之前执行的方法");
                return bean;
            }
        
            @Override
            public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
                System.out.println("在初始化后执行的方法");
                return bean;
            }
        }
        ```

    2.  在Spring配置文件中进行配置

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <beans xmlns="http://www.springframework.org/schema/beans"
               xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
               xmlns:p="http://www.springframework.org/schema/p"
               xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
            <bean id="orders" class="com.dkx.bean.Orders" p:oname="手机" init-method="initMethod" destroy-method="destroyMethod"></bean>
            <!--配置后置处理器-->
            <bean id="myBeanPost" class="com.dkx.bean.MyBeanPost"></bean>
        </beans>
        ```

    3.  测试代码：

        ```java
        public class AppTest {
            @Test
            public void test(){
                ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext
                        ("application3.xml");
                Orders orders = context.getBean("orders", Orders.class);
                System.out.println("第四步 获取创建bean实例对象:=> "+orders);
        //        手动让Bean实例销毁
                context.close();
            }
        }
        ```

        测试结果：

        ```
        第一步 Orders 无参构造执行了...
        第二步 Orders set方法执行了...
        在初始化之前执行的方法
        第三步 Orders initMethod初始化方法执行了...
        在初始化后执行的方法
        第四步 获取创建bean实例对象:=> com.dkx.bean.Orders@43738a82
        第五步 Orders destroyMethod销毁方法执行了...
        ```

### IOC操作Bean管理(xml自动装配)

1.什么是自动装配

1.  根据指定装配规则(属性名称或属性类型)，Spring自动将匹配的属性值进行注入

2.演示自动装配过程

1.  根据属性名称自动装配

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--
		实现自动装配
 		bean标签属性autowire，配置自动装配
		autowire属性常用两个值：
			byName：根据属性名称注入，注入值bean的id值和类属性名称一样
			byType：根据属性类型注入
     -->
    <bean id="emp" class="com.dkx.autowire.Emp" autowire="byName"></bean>
    <bean id="dept" class="com.dkx.autowire.Dept"></bean>
</beans>
```

2.  根据属性类型自动注入-不能有两个同样类型单名称不同的bean注入否则报错

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   <!--
		实现自动装配
 		bean标签属性autowire，配置自动装配
		autowire属性常用两个值：
			byName：根据属性名称注入，注入值bean的id值和类属性名称一样
			byType：根据属性类型注入
     -->
    <bean id="emp" class="com.dkx.autowire.Emp" autowire="byType"></bean>
    <bean id="dept" class="com.dkx.autowire.Dept"></bean>
    <!--根据类型注入当有两个同样类型就会报错-->
    <bean id="dept1" class="com.dkx.autowire.Dept"></bean>
</beans>
```

### IOC操作Bean管理(外部属性文件)

1.直接配置数据库信息

1.  引入德鲁伊连接池依赖jar包

    ![image-20230610204426396](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230610204426396.png)

2.  配置德鲁伊连接池

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--配置连接池-->
    <!--DruidDataSource druid = new DruidDataSource();-->
    <bean id="druid" class="com.alibaba.druid.pool.DruidDataSource">
        <!--set方法注入druid.setDriverClassName("com.mysql.cj.jdbc.Driver");-->
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/test?characterEncoding=utf-8&amp;serverTimezone=UTC&amp;useUnicode=true"></property>
        <property name="username" value="root"></property>
        <property name="password" value="dkx"></property>
    </bean>
</beans>
```

2.引入外部属性文件配置数据库连接池

1.  创建外部属性文件，properties格式文件，写数据库信息

    ```properties
    jdbc.driverClass=com.mysql.cj.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/test?characterEncoding=utf-8&serverTimezone=UTC&useUnicde=true
    jdbc.username=root
    jdbc.password=dkx
    ```

2.  把外部properties属性文件引入到Spring配置文件中

    引入context名称空间

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    </beans>
    ```

    在Spring配置文件使用标签引入外部属性文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                              http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
        <context:property-placeholder location="classpath:jdbc.properties"/>
        <!--配置连接池-->
        <!--DruidDataSource druid = new DruidDataSource();-->
        <bean id="druid" class="com.alibaba.druid.pool.DruidDataSource">
            <!--set方法注入druid.setDriverClassName("com.mysql.cj.jdbc.Driver");-->
            <property name="driverClassName" value="${jdbc.driverClass}"></property>
            <property name="url" value="${jdbc.url}"></property>
            <property name="username" value="${jdbc.username}"></property>
            <property name="password" value="${jdbc.password}"></property>
        </bean>
    </beans>
    ```

### IOC操作Bean管理(基于注解方式)

#### 1.什么是注解

1.  注解是代码特殊标记，格式：@注解名称(属性名称=属性值，属性名称=属性值)
2.  使用注解，注解作用在类上面，方法上面，属性上面
3.  使用注解目的：简化xml配置

#### 2.Spring针对Bean管理中创建对象提供注解

1.  @Component
2.  @Service
3.  @Controller
4.  @Repository

上面四个注解功能是一样的，都可以用来创建Bean实例，但是不同的注解代表不同的含义

#### 3.基于注解方式实现对象创建

1.  第一步   引入依赖

![image-20230610213635945](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230610213635945.png)

![image-20230610213753487](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230610213753487.png)

2.  第二步   开启组件扫描

    2.  1 使用context名称空间，引入外部属性

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    </beans>
    ```

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                                http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
        <!--
            开启组件扫描
            1.如果扫描多个包，多个包之间使用逗号隔开,如下：
            com.dkx.dao,com.dkx.service
            2.直接扫描它们两个的父级目录,如下(不建议因为会扫描我们并不想扫描的东西)：
            com.dkx
        -->
        <context:component-scan base-package="com.dkx.dao,com.dkx.service"></context:component-scan>
    </beans>
    ```

3.  第三步   创建类，在类上面添加创建对象注解

    ```java
    /**
     * 在注解里面value属性值可以省略不写
     * 默认值是类名称，首字母小写 UserService -> userService
     */
    @Component(value = "userService") //<bean id="userService" class="..">
    public class UserService {
        public void add(){
            System.out.println("service add ... ");
        }
    }
    ```

4.  测试代码：

    ```java
    public class AppTest {
    @Test
    public void test(){
          ApplicationContext context = new ClassPathXmlApplicationContext
             ("application.xml");
          UserService service = context.getBean("userService", UserService.class);
          service.add();
    }
    ```

    测试结果：

    ```
    service add ... 
    ```

5.  过程解释：Spring配置扫描，扫描指定的包路径，找到被扫描到的注解后将该对象进行实例Bean管理然后就可以通过getBean来获取IOC容器中的对象，然后获得对象后调用方法

6.  <font style="color:red">开启组件扫描细节配置</font>.

    -  use-default-filters="false"不扫描所有配置，如果为true则扫描全部但是可以排除使用exclude

    -  include：包含哪些扫描配置
    -  exclude：不包含哪些扫描配置

    ```xml
    <!--
            use-default-filters="false" 表示现在不使用默认filters，自己配置filter
            context:include-filter: 设置扫描哪些内容
        -->
    <context:component-scan base-package="com.dkx.dao,com.dkx.service" use-default-filters="false">
       <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    ```

#### 4.基于注解方式实现属性注入

#### 1.@Autowired

-  根据属性类型进行自动装配

1.  第一步 把service和dao对象创建，在service和dao类添加创建对象注解

```java
/**
 * dao接口
 */
public interface UserDao {
    public void add();
}
/**
 * dao实现类
 */
@Repository
public class UserDaoImpl implements UserDao{
    public void add(){
        System.out.println("UserDao add 执行了...");
    }
}
/**
 * service类
 * 在注解里面value属性值可以省略不写
 * 默认值是类名称，首字母小写 UserService -> userService
 */
@SuppressWarnings("all")
@Component(value = "userService") //<bean id="userService" class="..">
public class UserService {
    public void add(){
        System.out.println("service add ... ");
    }
}

```

2.  第二步 把service注入dao对象，在service类添加dao类属性，在属性上面使用注解

```java
/**
 * 在注解里面value属性值可以省略不写
 * 默认值是类名称，首字母小写 UserService -> userService
 */
@SuppressWarnings("all")
@Component(value = "userService") //<bean id="userService" class="..">
public class UserService {
    //定义dao类型属性
    //不需要添加Set方法,@Autowired底层已经帮我们实现了
    //添加注入属性注解
    @Autowired
    private UserDao userDao;
    public void add(){
        System.out.println("service add ... ");
        userDao.add();
    }
}
```

3.  测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
        UserService service = context.getBean("userService", UserService.class);
        service.add();
    }
}
```

测试结果：

```
service add ... 
UserDao add 执行了...
```

#### XML方式

```java
/**
 * 在注解里面value属性值可以省略不写
 * 默认值是类名称，首字母小写 UserService -> userService
 */
@SuppressWarnings("all")
@Component //<bean id="userService" class="..">
public class UserService {
   private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void add(){
        System.out.println("service add ... ");
        userDao.add();
    }
}
```

```java
//指定创建对象注解的名称
@Repository
public class UserDaoImpl implements UserDao {
    public void add(){
        System.out.println("UserDao add 执行了...");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--
        use-default-filters="false" 表示现在不使用默认filters，自己配置filter
        context:include-filter: 设置扫描哪些内容
    -->
    <context:component-scan base-package="com.dkx.dao,com.dkx.service" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
    </context:component-scan>
    <bean id="service" class="com.dkx.service.UserService" autowire="byType"></bean>
    <!--因为上面扫描注解已经创建了Bean了不需要再配置Bean了否则是多个Bean按类型注入会报错-->
<!--    <bean id="userDao" class="com.dkx.dao.impl.UserDaoImpl"></bean>-->
</beans>
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
        UserService service = context.getBean("service", UserService.class);
        service.add();
    }
}
```

测试结果：

```
service add ... 
UserDao add 执行了...
```

#### 2.@Qualifier

-  根据属性名称进行注入
-  这个@Qualifier注解的使用需要和上面的@Autowired一起使用
-  解决的场景：一个接口可以有多个实现类，如果按照类型注入则会不知道具体注入哪个实现类导致报错，在Dao实现类的@Repository创建对象注解中进行添加该Bean的名称比如：@Repository(value = "名称")

```java
//指定创建对象注解的名称
@Repository(value = "userDaoImpl")
public class UserDaoImpl implements UserDao{
    public void add(){
        System.out.println("UserDao add 执行了...");
    }
}
```

```java
/**
 * 在注解里面value属性值可以省略不写
 * 默认值是类名称，首字母小写 UserService -> userService
 */
@SuppressWarnings("all")
@Component(value = "userService") //<bean id="userService" class="..">
public class UserService {
    //定义dao类型属性
    //不需要添加Set方法,@Autowired底层已经帮我们实现了
    //添加注入属性注解
    @Autowired
    //根据名称进行注入,需要配合Autowired使用
    @Qualifier(value = "userDaoImpl")
    private UserDao userDao;
    public void add(){
        System.out.println("service add ... ");
        userDao.add();
    }
}
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
        UserService service = context.getBean("userService", UserService.class);
        service.add();
    }
}
```

测试结果：

```
service add ... 
UserDao add 执行了...
```

#### XML方式

```java
/**
 * 在注解里面value属性值可以省略不写
 * 默认值是类名称，首字母小写 UserService -> userService
 */
@SuppressWarnings("all")
@Component //<bean id="userService" class="..">
public class UserService {
   private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void add(){
        System.out.println("service add ... ");
        userDao.add();
    }
}
```

```java
//指定创建对象注解的名称
@Repository
public class UserDaoImpl implements UserDao {
    public void add(){
        System.out.println("UserDao add 执行了...");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--
        use-default-filters="false" 表示现在不使用默认filters，自己配置filter
        context:include-filter: 设置扫描哪些内容
    -->
    <context:component-scan base-package="com.dkx.dao,com.dkx.service" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
    </context:component-scan>
    <bean id="service" class="com.dkx.service.UserService" autowire="byName"></bean>
    <!--必须要配置一个需要注入的bean因为按照配置的bean来找的-->
    <bean id="userDao" class="com.dkx.dao.impl.UserDaoImpl"></bean>
    <!--配置多个同样bean不会报错,不配置则报错空指针-->
    <bean id="userDao1" class="com.dkx.dao.impl.UserDaoImpl"></bean>
</beans>
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
        UserService service = context.getBean("service", UserService.class);
        service.add();
    }
}
```

测试结果：

```
service add ... 
UserDao add 执行了...
```

#### 3.@Resource

-  可以根据类型注入，可以根据名称注入
-  在不指定名称name属性的情况下就是按类型注入，指定了name属性则按名称注入
-  需要注意：@Resource包：javax.annotation.Resource ，是Java扩展包中的而不是Spring中的Spring官方建议还是使用@Autowired和@Qualifier来进行注入。

```java
/**
 * 在注解里面value属性值可以省略不写
 * 默认值是类名称，首字母小写 UserService -> userService
 */
@SuppressWarnings("all")
@Component(value = "userService") //<bean id="userService" class="..">
public class UserService {
    //根据类型注入
//    @Resource
    //根据名称注入
    @Resource(name = "userDaoImpl")
    private UserDao userDao;
    public void add(){
        System.out.println("service add ... ");
        userDao.add();
    }
}
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
        UserService service = context.getBean("userService", UserService.class);
        service.add();
    }
}
```

测试结果：

```
service add ... 
UserDao add 执行了...
```

#### 4.@Value

-  注入普通类型属性

```java
/**
 * 在注解里面value属性值可以省略不写
 * 默认值是类名称，首字母小写 UserService -> userService
 */
@SuppressWarnings("all")
@Component(value = "userService") //<bean id="userService" class="..">
public class UserService {
    //普通类型注入
    @Value(value = "张三")
    private String name;

    public void add(){
        System.out.println("service add ... "+name);
    }
}
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
        UserService service = context.getBean("userService", UserService.class);
        service.add();
    }
}
```

测试结果：

```
service add ... 张三
```

#### XML方式

```java
/**
 * 在注解里面value属性值可以省略不写
 * 默认值是类名称，首字母小写 UserService -> userService
 */
@SuppressWarnings("all")
@Component //<bean id="userService" class="..">
public class UserService {
    private String name;

    public void setName(String name) {
        this.name = name;
    }

    public void add(){
        System.out.println("service add ... "+name);
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--
        use-default-filters="false" 表示现在不使用默认filters，自己配置filter
        context:include-filter: 设置扫描哪些内容
    -->
    <context:component-scan base-package="com.dkx.dao,com.dkx.service" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
    </context:component-scan>
    <bean id="service" class="com.dkx.service.UserService" p:name="张三"></bean>
    <!--或者-->
    <bean id="service" class="com.dkx.service.UserService">
        <property name="name" value="李四"/>
    </bean>
</beans>
```

测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext
                ("application.xml");
        UserService service = context.getBean("service", UserService.class);
        service.add();
    }
}
```

测试结果：

```
service add ... 李四
```

#### 6.完全注解开发

0.将xml配置文件删除或者.back掉

![image-20230611024902517](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20230611024902517.png)

1.创建配置类，替代xml配置文件

```java
@Configuration //作为配置类，替代xml配置文件
@ComponentScan(basePackages = {"com.dkx.dao","com.dkx.service"}) //扫描配置类
public class SpringConfig {

}
```

2.service类

```java
/**
 * 在注解里面value属性值可以省略不写
 * 默认值是类名称，首字母小写 UserService -> userService
 */
@SuppressWarnings("all")
@Component(value = "userService") //<bean id="userService" class="..">
public class UserService {
    @Autowired
    @Qualifier(value = "userDaoImpl")
    private UserDao userDao;

    public void add(){
        System.out.println("service add ... ");
        userDao.add();
    }
}
```

3.dao接口和实现类

```java
/**
 * dao接口
 */
public interface UserDao {
    public void add();
}
/**
 * dao实现类
 */
//指定创建对象注解的名称
@Repository(value = "userDaoImpl")
public class UserDaoImpl implements UserDao {
    public void add(){
        System.out.println("UserDao add 执行了...");
    }
}
```

4.测试代码：

```java
public class AppTest {
    @Test
    public void test(){
        ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
        UserService service = context.getBean("userService", UserService.class);
        service.add();
    }
}
```

测试结果：

```
service add ... 
UserDao add 执行了...
```







