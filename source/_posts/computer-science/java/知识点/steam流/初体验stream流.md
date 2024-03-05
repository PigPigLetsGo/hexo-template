---
title: Stream初体验
categories:
    - [计算机学科,java,知识点,stream流]
tags:
    - stream流
    - 知识点
---

# Stream初体验

-  案例需求

   按照下面的要求完成集合的创建和遍历

   -  创建一个集合，存储多个字符串元素
   -  把集合中所有以 "张" 开头的元素存储到一个新的集合
   -  把 "张" 开头的集合中的长度为3的元素存储到一个新的集合
   -  遍历上一步得到的集合

## 原始方式示例代码

```java
public static void main(String[] args) {
   ArrayList<String> arrayList = new ArrayList<>(List.of("张三丰","张无忌","张翠山","王二麻子","张良","谢广坤"));
   ArrayList<String> list2 = new ArrayList<>();
   // 遍历所有集合中 姓张的元素 添加到list2集合中
   for (String s : arrayList) {
      if (s.startsWith("张")) {
         list2.add(s);
      }
   }
   ArrayList<String> list3 = new ArrayList<>();
   // 遍历list2集合所有元素将 名称长度为3 的元素存储到list3集合中
   for (String s : list2) {
      if (s.length() == 3) {
         list3.add(s);
      }
   }
   // 最终循环遍历list3集合里面都是我们上面符合要求的名称了
   for (String s : list3) {
      System.out.println(s);
   }
}
```

打印结果：

```
张三丰
张无忌
张翠山
```

可以看到上面的代码，我们费了老大劲才从集合中取到我们想要的结果，如果上面的操作换成stream流来操作呢？

## 使用Stream流示例代码

```java
public static void main(String[] args) {
   ArrayList<String> list = new ArrayList<>(List.of("张三丰","张无忌","张翠山","王二麻子","张良","谢广坤"));
   // stream()将list集合转换为 流
   // filter通过Lambda表达式的形式来过滤首字母不是 "张" 的名字
   // filter过滤掉长度不为3的名字
   // forEach循环遍历打印出前面预期的名字
   list.stream().filter(s -> s.startsWith("张"))
      .filter(s -> s.length() == 3).forEach(s -> System.out.println(s));
}
```

打印结果：

```
张三丰
张无忌
张翠山
```

**Stream流的好处**：

1.  直接阅读代码的字面意思即可完美展示无关逻辑方式的语义：获取流，过滤姓张，过滤长度为3，逐一打印
2.  Stream流把真正的函数式编程风格引入到java中
3.  代码简洁