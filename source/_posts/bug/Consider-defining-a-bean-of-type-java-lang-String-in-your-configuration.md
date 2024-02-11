---
title: Consider-defining-a-bean-of-type-java-lang-String-in-your-configuration
categories:
    - [问题总汇]
tags:
    - 问题总汇
    - SpringBoot
    - 读取配置文件
---

问题描述

SpringBoot创建application.yml实体类读取配置文件中的数组数据报错

代码:

- BookController类

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
   @Autowired
   private Environment environment;
   @Autowired
   private Enterprise enterprise;
   @GetMapping("/{id}")
   public String getById(@PathVariable Integer id){
      System.out.println("id ==> "+id);
      System.out.println(lesson);
      System.out.println(name);
      System.out.println(age);
      System.out.println(tels);
      System.out.println(subject_0);
      System.out.println(environment.getProperty("enterprise.name"));
      System.out.println("---------------------------------------");
      System.out.println(enterprise);
      return "bookcontroller getbyid running...";
   }
}
```

- 读取yml文件数组数据的类

```java
@SuppressWarnings("all")
@Component
@ConfigurationProperties(prefix = "enterprise")
public class Enterprise {
    private String name;
    private Integer age;
    private String tel;
    private String[] subject;

    public String getName() {
        return name;
    }

    public Enterprise(String name, Integer age, String tel, String[] subject) {
        this.name = name;
        this.age = age;
        this.tel = tel;
        this.subject = subject;
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

Run console result

![![image_2023-03-09-19-25-48](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image_2023-03-09-19-25-48_20230309195859.png)](Consider-defining-a-bean-of-type-java-lang-String-in-your-configuration_md_files/image_2023-03-09-19-25-48_20230309195859.png?v=1&type=image&token=V1:-un5KsUqcmW2LChlssYZih8uJskIN8vr9VobfvixLMs)

原因:因为使用了@Autowired自动装配而会实例化bean此时实例化bena是通过无参构造来完成的如果将该类重写的构造器并且还是个有参构造器那么就会报错

解决方案:

删除重写的有参构造器
