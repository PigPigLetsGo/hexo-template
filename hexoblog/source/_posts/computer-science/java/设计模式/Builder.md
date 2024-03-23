---
title: 建造者模式
categories:
    - [计算机学科,java,设计模式]
tags:
    - 计算机学科
    - java
    - 设计模式
---

# Builder

## 介绍

Builder模式，也称为建造者模式，是一种创建设计模式，它将一个复杂的构建与它的表示分离。

使得同样的构建过程可以创建不同的表示。这种模式通常用于创建具有多个属性的复杂对象，特别是当这些属性之间存在一定的依赖或顺序关系时。

### 原生代码实现

下面是java实现builder构建的方式

Person类 其中Builder静态内部类是 构建器类

```java
public class Person {
    private Integer id;
    private String name;

    private Person(Builder builder)
    {
        this.id = builder.id;
        this.name = builder.name;
    }

    public static class Builder
    {
        private Integer id;
        private String name;

        public Builder id(Integer id)
        {
            this.id = id;
            return this;
        }

        public Builder name(String name)
        {
            this.name = name;
            return this;
        }

        public Person build()
        {
            return new Person(this);
        }

        @Override
        public String toString() {
            return "Builder{" +
                    "id=" + id +
                    ", name='" + name + '\'' +
                    '}';
        }
    }

    public static Builder builder()
    {
        return new Builder();
    }

    @Override
    public String toString() {
        return "Person{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

Main

```java
public class Main1 {
    public static void main(String[] args) {
        Person person = Person.builder()
                .id(1)
                .name("李四")
                .build();
        System.out.println(person);
    }
}
```

执行结果：

```
Person{id=1, name='李四'}
```

## 使用@Builder

### 介绍@Builder

>  lombok库中的@Builder注解是一种实现Builder设计模式的工具。它可以帮助我们生成相对复杂的构建器API，从而简化对象的创建过程

**以下是@Builder的主要功能和作用** 

**1、生成构建器类**：@Builder会为你的类生成一个名为ThisClassBuilder的内部静态类，这个类就是构建器。构建器类中包含了与原始类相同的属性

**2、生成构建器方法**：对于原始类中的每个参数，@Builder会在构建器类中生成一个类似于setter的方法。这些方法的名称与参数名相同，返回值是构建器本身，这样就可以支持链式调用

**3、生成build方法**：构建器类中，@Builder会生成一个build()方法。调用这个方法会根据设置的值创建实体对象

**4、生成toString方法**：@Builder还会在构建器类中生成一个toString()方法。<font color='red'>注意不是重写Object 的toString</font>.

**5、生成builder方法**：在原始类中，@Builder会生成一个builder()方法。这个方法用于创建构建器

**6、支持集合操作**：@Builder还可以配合@Singular注解来处理集合类型的字段。@Singular可以生成添加单个元素到集合的方法，以及清空集合的方法。

**下面是java代码演示**：

Teacher类：

```java
@Builder
@ToString
public class Teacher {
    private Integer id;
    private String name;
    @Singular
    private List<String> skills;
    @Singular
    private List<Student> students;
}
```

Student类：

```java
@Builder
@ToString
public class Student {
    private Integer id;
    private String name;
    @Singular
    private List<String> subjects;
}
```

Main

```java
public class Main {
    public static void main(String[] args) {
        Teacher person = Teacher.builder()
                .id(1)
                .name("导师-张三")
                .skill("Java")
                .skill("C/C++")
                .skill("Python")
                .skill("Go")
                .skill("Javascript")
                .student(
                        Student.builder()
                                .id(1)
                                .name("李四")
                                .subject("Java")
                                .build()
                )
                .student(
                        Student.builder()
                                .id(1)
                                .name("王五")
                                .subject("Java")
                                .build()
                )
                .student(
                        Student.builder()
                                .id(1)
                                .name("李华")
                                .subject("Java")
                                .build()
                )
                .build();
        System.out.println(person);
    }
}
```

执行结果：

```
Teacher(id=1, name=导师-张三, skills=[Java, C/C++, Python, Go, Javascript], students=[Student(id=1, name=李四, subjects=[Java]), Student(id=1, name=王五, subjects=[Java]), Student(id=1, name=李华, subjects=[Java])])
```

