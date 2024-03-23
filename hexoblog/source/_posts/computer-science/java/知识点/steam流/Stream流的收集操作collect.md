---
title: Stream流的收集操作collect
categories:
    - [计算机学科,java,知识点,stream流]
tags:
    - stream流
    - 知识点
---

# Stream流的收集操作collect

**概念**：

对数据使用Stream流的方式操作完毕后，可以把流中的数据收集到集合中

**常用方法**：

| 方法名                         | 说明               |
| ------------------------------ | ------------------ |
| R collect(Collector collector) | 把结果收集到集合中 |

工具类Collectors提供了具体的收集方式

| 方法名                                                       | 说明                   |
| ------------------------------------------------------------ | ---------------------- |
| public static<T> Collector toList()                          | 把元素收集到List集合中 |
| public static<T> Collector toSet()                           | 把元素收集到Set集合中  |
| public static Collector toMap(Function keyMapper, Function valueMapper) | 把元素收集到Map集合中  |

**注意**：如果收集到的数据中有重复的==List不会去重而Set会去重==

**代码演示**：

```java
// toList和toSet方法演示 
public class MyStream7 {
    public static void main(String[] args) {
        ArrayList<Integer> list1 = new ArrayList<>();
        for (int i = 1; i <= 10; i++) {
            list1.add(i);
        }

        list1.add(10);
        list1.add(10);
        list1.add(10);
        list1.add(10);
        list1.add(10);

        //filter负责过滤数据的.
        //collect负责收集数据.
                //获取流中剩余的数据,但是他不负责创建容器,也不负责把数据添加到容器中.
        //Collectors.toList() : 在底层会创建一个List集合.并把所有的数据添加到List集合中.
        List<Integer> list = list1.stream().filter(number -> number % 2 == 0)
                .collect(Collectors.toList());

        System.out.println(list);

    Set<Integer> set = list1.stream().filter(number -> number % 2 == 0)
            .collect(Collectors.toSet());
    System.out.println(set);
}
}
/**
Stream流的收集方法 toMap方法演示
创建一个ArrayList集合，并添加以下字符串。字符串中前面是姓名，后面是年龄
"zhangsan,23"
"lisi,24"
"wangwu,25"
保留年龄大于等于24岁的人，并将结果收集到Map集合中，姓名为键，年龄为值
*/
public class MyStream8 {
	public static void main(String[] args) {
      	ArrayList<String> list = new ArrayList<>();
        list.add("zhangsan,23");
        list.add("lisi,24");
        list.add("wangwu,25");

        Map<String, Integer> map = list.stream().filter(
                s -> {
                    String[] split = s.split(",");
                    int age = Integer.parseInt(split[1]);
                    return age >= 24;
                }

         //   collect方法只能获取到流中剩余的每一个数据.
         //在底层不能创建容器,也不能把数据添加到容器当中

         //Collectors.toMap 创建一个map集合并将数据添加到集合当中

          // s 依次表示流中的每一个数据

          //第一个lambda表达式就是如何获取到Map中的键
          //第二个lambda表达式就是如何获取Map中的值
        ).collect(Collectors.toMap(
                s -> s.split(",")[0],
                s -> Integer.parseInt(s.split(",")[1]) ));

        System.out.println(map);
	}
}
```

