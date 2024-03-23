---
title: Map集合遍历几种方式
categories:
    - [计算机学科,java,知识点,集合]
tags:
    - 集合
    - 知识点
---

## EntrySet

以Set集合形式返回Map集合中所有键值对

EntrySet:通过指定的键删除对应的值

```java
public class Demo_6 {
    public static void main(String[]args){
        HashMap<Integer,String> ar = new HashMap<>();
        //向集合中添加一个键值对
        ar.put(1,"a");
        ar.put(2,"b");
        ar.put(3,"c");
        //集合对象名调用EntrySet获取对象,以Set集合形式,返回Map集合中所有键值
        Set<Map.Entry<Integer,String>> s = ar.entrySet();
        //通过Set集合的形式对象调用 迭代器
        Iterator<Map.Entry<Integer,String>> a = s.iterator();
        //判断是否有元素,有则true 没有则false
        while(a.hasNext()){
            //使用Map.Entry静态方法获取Map中键值对 next来往下遍历 并赋值给变量x
            Map.Entry<Integer,String> x = a.next();
            //获取每次遍历的 Key / 值
            Integer x1 = x.getKey();
            //通过Map集合的get方法通过指定的键获取指定的值的方式来判断哪个元素的值为a
            if(ar.get(x1) == "a"){
                //满足判断条件后执行 删除
                a.remove();
            }
            //通过指定的键获取值的方式来打印 键值对
            System.out.println(x1+","+ar.get(x1));
        }
    }
}
```

## KeySet

以Set集合形式返回Map集合中所有键

通过KeySet指定的键删除对应的值

```java
public class Demo_7 {
    public static void main(String[]args){
        //创建 Map集合
        HashMap<Integer,String> ar = new HashMap<>();
        //向集合中添加一个键值对
        ar.put(1,"a");
        ar.put(2,"b");
        ar.put(3,"c");
        //以Set集合形式,返回Map中所有键
        Set s = ar.keySet();
        //通过Set集合形式的对象调用 迭代器
        Iterator s1 = s.iterator();
        //遍历元素
        while(s1.hasNext()){
            //强制类型转换 获取 Integer类型的 键 遍历获取
            Integer key =(Integer) s1.next();
            //通过Map集合的get方法 遍历键对应的值是否为 a
            if(ar.get(key) == "a"){
                //删除
                s1.remove();
            }
            System.out.println(key+","+ar.get(key));
        }
    }
}
```

## values

以Collection集合形式,返回Map集合中所有的值

遍历元素

```java
public class Demo_8 {
    public static void main(String[]args){
        HashMap<Integer,String> a = new HashMap<>();
        a.put(1,"a");
        a.put(2,"b");
        a.put(3,"c");
        //以Collection集合形式,返回Map集合中所有值
        Collection s = a.values();
        //通过Collection集合形式的对象调用 迭代器
        Iterator s1 = s.iterator();
        //遍历 集合中 所有值
        while(s1.hasNext()){
            //将每次获取的值赋值给 String类型的变量ar
            String ar =(String) s1.next();
            System.out.println(ar);
        }
    }
}
```

