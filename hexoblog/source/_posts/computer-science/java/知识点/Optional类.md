---
title: Optional类
categories:
    - [计算机学科,java,知识点]
tags:
    - 知识点
---

# Optional类

Optional 类是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。

Optional 是个容器：它可以保存类型T的值，或者仅仅保存null。Optional提供很多有用的方法，这样我们就不用显式进行空值检测。

Optional 类的引入很好的解决空指针异常。

## 类方法

| 方法                                                 | 描述                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| empty()                                              | 返回空的Optional实例                                         |
| equals(Object obj)                                   | 判断其它对象是否等于Optional                                 |
| filter(Predicate<? super<T>> predicate)              | 如果值存在，并且这个值匹配给定的 predicate，返回一个<br />Optional用以描述这个值，否则返回一个空的Optional。 |
| flatMap(Function<? super T, Optional<U>> mapper)     | 如果值存在，返回基于Optional包含的映射方法的值<br />否则返回一个空的Optional |
| get()                                                | 如果在这个Optional中包含这个值，返回值<br />否则抛出异常：NoSuchElementException |
| hashCode()                                           | 返回存在值的哈希码，如果值不存在 返回 0                      |
| ifPresent(Consumer<? super T> consumer)              | 如果值存在则使用该值调用 consumer , 否则不做任何事情         |
| ifPresent()                                          | 如果值存在则方法会返回true，否则返回 false                   |
| map(Function<? super T,? extends U> mapper)          | 如果有值，则对其执行调用映射函数得到返回值<br />如果返回值不为 null，则创建包含映射返回值的Optional<br />作为map方法返回值，否则返回空Optional |
| of(T value)                                          | 返回一个指定非null值的Optional                               |
| ofNullable(T value)                                  | 如果为非空，返回 Optional 描述的指定值，否则返回空的 Optional |
| orElse(T other)                                      | 如果存在该值，返回值， 否则返回 other                        |
| orElseGet(Supplier<? extends T> other)               | 如果存在该值，返回值， 否则触发 other，并返回 other 调用的结果 |
| orElseThrow(Supplier<? extends X> exceptionSupplier) | 如果存在该值，返回包含的值，否则抛出由 Supplier 继承的异常   |
| toString()                                           | 返回一个Optional的非空字符串，用来调试                       |

PS：这些方法都是从java.lang.Object类继承来的

## 简介

Optional 是一个对象容器，具有以下两个特点：

-     提示用户要注意该对象有可能为null
-     简化if else代码

## 代码演示：

**需求：**

  学校想从一批学生中，选出年龄大于等于18，参加过考试并且成绩大于80的人去参加比赛

```java
import java.util.Arrays;
import java.util.List;

public class Student
{

    private String name;
    private int age;
    private Integer score;

    public Student()
    {
    }

    public Student(String name, int age, Integer score)
    {
        this.name = name;
        this.age = age;
        this.score = score;
    }

    public String getName()
    {
        return name;
    }

    public void setName(String name)
    {
        this.name = name;
    }

    public int getAge()
    {
        return age;
    }

    public void setAge(int age)
    {
        this.age = age;
    }

    public Integer getScore()
    {
        return score;
    }

    public void setScore(Integer score)
    {
        this.score = score;
    }

    @Override
    public String toString()
    {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", score=" + score +
                '}';
    }

    public static void main(String[] args)
    {
        System.out.println(new Student().initData());
    }

    public List<Student> initData(){
        Student s1 = new Student("张三", 19, 80);
        Student s2 = new Student("李四", 19, 50);
        Student s3 = new Student("王五", 23, null);
        Student s4 = new Student("赵六", 16, 90);
        Student s5 = new Student("钱七", 18, 99);
        Student s6 = new Student("孙八", 20, 40);
        Student s7 = new Student("吴九", 21, 88);
        return Arrays.asList(s1, s2, s3, s4, s5, s6, s7);
    }
}
```

打印结果：

```
[Student{name='张三', age=19, score=80}, Student{name='李四', age=19, score=50}, Student{name='王五', age=23, score=null}, Student{name='赵六', age=16, score=90}, Student{name='钱七', age=18, score=99}, Student{name='孙八', age=20, score=40}, Student{name='吴九', age=21, score=88}]
```

## jdk8之前的写法

```java
public void beforeJava8() {
   List<Student> studentList = initData();
   for (Student student : studentList) {
      if (student != null) {
         if (student.getAge() >= 18) {
            Integer score = student.getScore();
            if (score != null && score > 80) {
               System.out.println("入选：" + student.getName());
            }
         }
      }
   }
}
```

打印结果：

```
[Student{name='张三', age=19, score=80}, Student{name='李四', age=19, score=50}, Student{name='王五', age=23, score=null}, Student{name='赵六', age=16, score=90}, Student{name='钱七', age=18, score=99}, Student{name='孙八', age=20, score=40}, Student{name='吴九', age=21, score=88}]
----jdk8之前的写法----
入选：钱七
入选：吴九
```

## jdk8写法

```java
public void useJava8() {
   List<Student> studentList = initData();
   for (Student student : studentList) {
      Optional<Student> studentOptional = Optional.of(student);
      Integer score = studentOptional.filter(s -> s.getAge() >= 18).map(Student::getScore).orElse(0);
      if (score > 80) {
         System.out.println("入选：" + student.getName());
      }
   }
}
```

打印结果：

```
[Student{name='张三', age=19, score=80}, Student{name='李四', age=19, score=50}, Student{name='王五', age=23, score=null}, Student{name='赵六', age=16, score=90}, Student{name='钱七', age=18, score=99}, Student{name='孙八', age=20, score=40}, Student{name='吴九', age=21, score=88}]
----jdk8之前的写法----
入选：钱七
入选：吴九
----jdk8的写法----
入选：钱七
入选：吴九
```

## 代码演示2：

### Address

```java
public class Address
{
    private House house;

    public House getHouse()
    {
        return house;
    }

    public void setHouse(House house)
    {
        this.house = house;
    }
}
```

### House

```java
public class House
{
    private String name;

    public String getName()
    {
        return name;
    }

    public void setName(String name)
    {
        this.name = name;
    }
}
```

### User

```java
public class User
{
    private String name;
    private int age;
    private Address address;

    public User()
    {
    }

    public User(String name, int age)
    {
        this.name = name;
        this.age = age;
    }

    public String getName()
    {
        return name;
    }

    public Address getAddress()
    {
        return address;
    }

    public void setAddress(Address address)
    {
        this.address = address;
    }

    public void setName(String name)
    {
        this.name = name;
    }

    public int getAge()
    {
        return age;
    }

    public void setAge(int age)
    {
        this.age = age;
    }

    @Override
    public String toString()
    {
        return "User{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

### Test

```java
public class Test12
{
    public static void main(String[] args)
    {
        // Optional基本的用法
        User user = new User("刘桑啊", 20);
        // 实际项目不会这么用，仅做简单介绍
        Optional<User> userOptional = Optional.ofNullable(user);
        User user1 = userOptional.get();
        System.out.println("user1 = " + user1);
        // Optional.of(null); 传入null 抛异常
        // Optional.ofNullable(null); // 传啥都不会报错
        // 实际项目中这么用，如下：
        // 很好用，不用写 != null
        Optional.ofNullable(user).ifPresent(t -> System.out.println("user1不是null, 我才会执行"));
        Optional.ofNullable(user)
                .filter(t -> t.getName().contains("刘"))
                .ifPresent(t1 -> System.out.println("filter里是true的话我才能执行到"));
        // 传入null 创建一个默认对象并返回
        // 如果传入的不是一个null而是一个实例化的对象那么就不会返回默认对象了，而是我们传入的对象
        User user2[] = getUser(/*user*/ null);
        for (User user3 : user2)
        {
            System.out.println(user3);
        }
        // 对象的复杂操作
        ObjectTest();
        // 关于异常
        Optional.ofNullable(null).orElseThrow(() -> new RuntimeException("我是异常，需要捕获"));
    }


    public static User[] getUser(User user)
    {
        // 为空的时候获取默认值
        // jdk1.9
        // Optional.ofNullable(user).or(() -> Optional.of(new User("admin", 17))).get();
        // jdk1.8
        User admin = Optional.ofNullable(user).orElse(new User("admin", 90));
        // jdk1.8 orElseGet接收一个函数式接口
        User admin1 = Optional.ofNullable(user).orElseGet(() -> new User("admin", 24));
        return new User[]{admin, admin1};
    }

    private static void ObjectTest()
    {
        User user = new User();
        Address address = new Address();
        House house = new House();
        house.setName("刘桑的房子");
        address.setHouse(house);
        user.setAddress(address);
        // 将user存入optional得到optional
        Optional<User> userOptional = Optional.ofNullable(user);
        String name = userOptional
                // 如果不为null，创建包含映射返回值的optional
                .map(u -> u.getAddress())
                .map(address1 -> address1.getHouse())
                .map(house1 -> house1.getName())
                .orElse("default");
        System.out.println(name);
        // 效果和上面一样
        String s = userOptional
                .flatMap(u -> Optional.ofNullable(u.getAddress()))
                .flatMap(address1 -> Optional.ofNullable(address1.getHouse()))
                .flatMap(house1 -> Optional.ofNullable(house1.getName())).orElse("default");
        System.out.println(s);
    }
}
```

打印结果：

```
user1 = User{name='刘桑啊', age=20}
user1不是null, 我才会执行
filter里是true的话我才能执行到
User{name='admin', age=90}
User{name='admin', age=24}
刘桑的房子
刘桑的房子
Exception in thread "main" java.lang.RuntimeException: 我是异常，需要捕获
	at com.dkx.Test12.lambda$main$3(Test12.java:34)
	at java.util.Optional.orElseThrow(Optional.java:290)
	at com.dkx.Test12.main(Test12.java:34)
```

