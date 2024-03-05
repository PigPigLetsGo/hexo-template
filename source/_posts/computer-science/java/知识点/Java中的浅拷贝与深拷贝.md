---
title: Java中的浅拷贝与深拷贝
categories:
    - [计算机学科,java,知识点]
tags:
    - 知识点
---

# Java中的浅拷贝与深拷贝

## 引用拷贝

>当我们使用=号将一个对象引用另一个对象时,此时的对象只是引用指向了另一个对象,当被引用对象发生改变时,引用对象也会发生改变

![image_2023-03-08-09-16-13](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081445087.png)

案例:

```java
public static void main(String[]args){
   Person p1 = new Person("小日子-刘桑",18);
	//克隆对象=new一个新的同样的对象并进行赋值不同操作返回结果是同一个对象但是结果不同
   Person p2 = p1;
   p2.setName("大佐-小日子-刘桑");
   System.out.println(p1);
   System.out.println(p2);
   /**
    * 使用 = 来引用对象判断时输出为true因为两个是同一个对象
    * 使用clone克隆出来的对象是不同的对象所以两个地址值是不同的输出为false
    */
   System.out.println(p1 == p2);
}
----------------输出结果----------------
Person{name='大佐-小日子-刘桑', age=18}
Person{name='大佐-小日子-刘桑', age=18}
true
```

## 浅拷贝

>在浅拷贝中Object提供了一个clone的方法,该方法的访问修饰符为protected如果子类不重写该方法,将将其声明为public那外部就调用不到了对象的clone方法,它是native方法底层已经实现了拷贝对象的逻辑,子类一定要实现Cloneable否则调用clone方法将会报错CloneNotSupportedException异常

![image_2023-03-08-09-22-05](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081445241.png)

```java

实现Cloneable,并重写clone方法
@SuppressWarnings("all")
//使用clone克隆对象，必须实现Cloneable接口否则会报错CloneNotSupportedException
public class Person implements Cloneable{
    String name;
    int age;
    public Person(String name,int age){
        this.name = name;
        this.age = age;
    }
//    重写克隆对象方法
    @Override
    public Person clone(){
        try{
            return (Person) super.clone();
        }catch(Exception e){
            e.printStackTrace();
            return null;
        }
    }

    public void setName(String name){
        this.name = name;
    }
    public String getName(){
        return name;
    }
    public void setId(int age){
        this.age = age;
    }
    public int getId(){
        return age;
    }
    public static void main(String[]args){
        Person p1 = new Person("小日子-刘桑",18);
//        克隆对象=new一个新的同样的对象并进行赋值不同操作返回结果是同一个对象但是结果不同
        Person p2 = p1.clone();
        p2.setName("大佐-小日子-刘桑");
        System.out.println(p1);
        System.out.println(p2);
        /**
         * 使用 = 来引用对象判断时输出为true因为两个是同一个对象
         * 使用clone克隆出来的对象是不同的对象所以两个地址值是不同的输出为false
         */
        System.out.println(p1 == p2);
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
----------------输出结果----------------
Person{name='小日子-刘桑', age=18}
Person{name='大佐-小日子-刘桑', age=18}
false
```

### 但是当属性中出现引用类型的成员变量,那么这种浅拷贝的方式只会复制该属性的引用地址,即拷贝对象和原对象的属性都指向了同一个对象,如果对一个对象进行操作会影响到另一个对象的属性,如果想要将对象中的引用类型进行拷贝就需要用深拷贝了

![image_2023-03-08-09-24-34](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081445145.png)

```java
public class Person implements Cloneable{
    String name;
    int age;
    int[] arr = new int[]{1,2,3,4,5};
//    重写克隆对象方法
    @Override
    public Person clone(){
        try{
            return (Person) super.clone();
        }catch(Exception e){
            e.printStackTrace();
            return null;
        }
    }
    public static void main(String[]args){
        Person p1 = new Person("小日子-刘桑",18);
//        克隆对象=new一个新的同样的对象并进行赋值不同操作返回结果是同一个对象但是结果不同
        Person p2 = p1.clone();
        p2.setName("大佐-小日子-刘桑");
        p2.arr[0] = 10;
        System.out.println(p1);
        System.out.println(p2);
        /**
         * 使用 = 来引用对象判断时输出为true因为两个是同一个对象
         * 使用clone克隆出来的对象是不同的对象所以两个地址值是不同的输出为false
         */
        System.out.println(p1 == p2);
        System.out.println(p1.arr == p2.arr);
    }
}
-----------------------输出结果-----------------------
Person{name='小日子-刘桑', age=18, arr=[10, 2, 3, 4, 5]}
Person{name='大佐-小日子-刘桑', age=18, arr=[10, 2, 3, 4, 5]}
false
true
```

## 深拷贝

>我们在重写clone的方法中对对象进行向下类型转换拷贝对象后再用对象名调用对象中的引用数据类型再进行clone这样就将对象中的引用类型 也拷贝了,引用类型拷贝的原理和对象拷贝一样

```java
//使用clone克隆对象，必须实现Cloneable接口否则会报错CloneNotSupportedException
public class Person implements Cloneable{
    String name;
    int age;
    int[] arr = new int[]{1,2,3,4,5};
    public Person(String name,int age){
        this.name = name;
        this.age = age;
    }
//    重写克隆对象方法
    @Override
    public Person clone(){
        try{
           Person person = (Person) super.clone();
//           将对象中的引用数据类型也进行拷贝
           person.arr = this.arr.clone();
           return person;
        }catch(Exception e){
            e.printStackTrace();
            return null;
        }
    }
    public static void main(String[]args){
        Person p1 = new Person("小日子-刘桑",18);
//        克隆对象=new一个新的同样的对象并进行赋值不同操作返回结果是同一个对象但是结果不同
        Person p2 = p1.clone();
        p2.setName("大佐-小日子-刘桑");
        p2.arr[0] = 10;
        System.out.println(p1);
        System.out.println(p2);
        /**
         * 使用 = 来引用对象判断时输出为true因为两个是同一个对象
         * 使用clone克隆出来的对象是不同的对象所以两个地址值是不同的输出为false
         */
        System.out.println(p1 == p2);
        System.out.println(p1.arr == p2.arr);
    }
}
-------------------------------输出结果-------------------------------
Person{name='小日子-刘桑', age=18, arr=[1, 2, 3, 4, 5]}
Person{name='大佐-小日子-刘桑', age=18, arr=[10, 2, 3, 4, 5]}
false
```

![image_2023-03-08-09-34-45](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401081445408.png)
