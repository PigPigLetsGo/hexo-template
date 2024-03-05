---
title: Stream流的思想和获取Stream流
categories:
    - [计算机学科,java,知识点,stream流]
tags:
    - stream流
    - 知识点
---

# Stream流的思想和获取Stream流



![image-20240227152340813](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/image-20240227152340813.png)

-  Stream流的三类方法

   -  获取Stream流

      -  创建一条流水线，并把数据放到流水线上准备进行操作

   -  中间方法

      -  流水线上的操作
      -  一次操作完毕之后，还可以继续进行其他操作

   -  终结方法

      -  一个Stream流只能有一个终结方法
      -  是流水线上的最后一个操作

   -  生成Stream流的方式

      -  Collection体系集合

         使用默认方法stream()生成流，default Stream<E> stream()

      -  Map体系集合

         把Map转成Set集合，简洁的生成流

      -  数组

         通过Arrays中的静态方法stream生成流

      -  同种数据类型的多个数据

         通过Stream接口的静态方法of(T ... Values)生成流

```java
public static void main(String[] args) {
   // Collection体系的集合可以使用默认stream()方法生成流
   ArrayList<String> list = new ArrayList<>();
   Stream<String> stream = list.stream();

   Set<String> set = new HashSet<>();
   Stream<String> stream1 = set.stream();

   // Map体系的集合间接调用stream()方法生成流
   Map<String, Integer> map = new HashMap<>();
   Stream<String> stream2 = map.keySet().stream();
   Stream<Integer> stream3 = map.values().stream();
   Stream<Map.Entry<String, Integer>> stream4 = map.entrySet().stream();

   // 数组通过Arrays中的静态方法stream()生成流
   String strings[] = {"hello","world","java"};
   Stream<String> stream5 = Arrays.stream(strings);

   // 同种数据类型的多个数据可以通过Stream接口的静态方法of(T ... Values)生成流
   Stream<String> hello = Stream.of("hello", "world", "java");
}
```

