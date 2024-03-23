---
title: Stream流中间操作方法
categories:
    - [计算机学科,java,知识点,stream流]
tags:
    - stream流
    - 知识点
---

# Stream流中间操作方法

**概念**：

中间操作的意思是，执行完此方法之后，Stream流依然可以继续执行其他操作

**常见方法**：

| 方法名                                        | 说明                                                     |
| --------------------------------------------- | -------------------------------------------------------- |
| Stream<T> filter(Predicate predicate)         | 用于对流中的数据进行过滤                                 |
| Stream<T> limit(Long maxSize)                 | 返回此流中的元素组成的流，截取前指定参数个数的数据       |
| Stream<T> skip(Long n)                        | 跳过指定参数个数的数据，返回由该流的剩余元素组成的流     |
| static<T> Stream<T> concat(Stream a,Stream b) | 合并a和b两个流为一个流                                   |
| Stream<T> distinct()                          | 返回由该流的不同元素(根据Object.equals(Object)) 组成的流 |

## filter操作演示

```java
public class MyStream3 {
    public static void main(String[] args) {
//        Stream<T> filter(Predicate predicate)：过滤
//        Predicate接口中的方法	boolean test(T t)：对给定的参数进行判断，返回一个布尔值

        ArrayList<String> list = new ArrayList<>();
        list.add("张三丰");
        list.add("张无忌");
        list.add("张翠山");
        list.add("王二麻子");
        list.add("张良");
        list.add("谢广坤");

        //filter方法获取流中的 每一个数据.
        //而test方法中的s,就依次表示流中的每一个数据.
        //我们只要在test方法中对s进行判断就可以了.
        //如果判断的结果为true,则当前的数据留下
        //如果判断的结果为false,则当前数据就不要.
//        list.stream().filter(
//                new Predicate<String>() {
//                    @Override
//                    public boolean test(String s) {
//                        boolean result = s.startsWith("张");
//                        return result;
//                    }
//                }
//        ).forEach(s-> System.out.println(s));

        //因为Predicate接口中只有一个抽象方法test
        //所以我们可以使用lambda表达式来简化
//        list.stream().filter(
//                (String s)->{
//                    boolean result = s.startsWith("张");
//                        return result;
//                }
//        ).forEach(s-> System.out.println(s));

        list.stream().filter(s ->s.startsWith("张")).forEach(s-> System.out.println(s));
    }
}
```

## limit&skip代码演示

```java
public class StreamDemo02 {
    public static void main(String[] args) {
        //创建一个集合，存储多个字符串元素
        ArrayList<String> list = new ArrayList<String>();

        list.add("林青霞");
        list.add("张曼玉");
        list.add("王祖贤");
        list.add("柳岩");
        list.add("张敏");
        list.add("张无忌");

        //需求1：取前3个数据在控制台输出
        list.stream().limit(3).forEach(s-> System.out.println(s));
        System.out.println("--------");

        //需求2：跳过3个元素，把剩下的元素在控制台输出
        list.stream().skip(3).forEach(s-> System.out.println(s));
        System.out.println("--------");

        //需求3：跳过2个元素，把剩下的元素中前2个在控制台输出
        list.stream().skip(2).limit(2).forEach(s-> System.out.println(s));
    }
}
```

## concat&distinct代码演示

```java
public class StreamDemo03 {
    public static void main(String[] args) {
        //创建一个集合，存储多个字符串元素
        ArrayList<String> list = new ArrayList<String>();

        list.add("林青霞");
        list.add("张曼玉");
        list.add("王祖贤");
        list.add("柳岩");
        list.add("张敏");
        list.add("张无忌");

        //需求1：取前4个数据组成一个流
        Stream<String> s1 = list.stream().limit(4);

        //需求2：跳过2个数据组成一个流
        Stream<String> s2 = list.stream().skip(2);

        //需求3：合并需求1和需求2得到的流，并把结果在控制台输出
//        Stream.concat(s1,s2).forEach(s-> System.out.println(s));

        //需求4：合并需求1和需求2得到的流，并把结果在控制台输出，要求字符串元素不能重复
        Stream.concat(s1,s2).distinct().forEach(s-> System.out.println(s));
    }
}
```

