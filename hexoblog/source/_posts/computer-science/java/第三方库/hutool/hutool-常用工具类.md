---
title: hutool-常用工具类
categories:
   - [计算机学科,java,第三方库]
tags:
   - 计算机学科
   - java
   - 第三方库
   - hutool
---

# hutool-常用工具类

## 文件读取-FileRader

```java
public static void main(String[] args)
{
   File src = new File("E:\\test.txt");
   FileReader fr = null;
   InputStream input = null;
   try
   {
      // 创建流
      fr = new FileReader(src);
      input = FileUtil.getInputStream(src);
      ArrayList<String> readLines = IoUtil.readLines(fr, new ArrayList<>());
      // 从流中读取内容，使用UTF-8编码
      ArrayList<String> readUtf8Lines = IoUtil.readUtf8Lines(input, new ArrayList<>());
      // 从流中读取内容，可指定编码
      ArrayList<String> readLines1 = IoUtil.readLines(input, "UTF-8", new ArrayList<>());
      for (String readLine : readLines)
      {
         System.out.println(readLine);
      }
   } catch (FileNotFoundException e)
   {
      throw new RuntimeException(e);
   } finally
   {
      try
      {
         // 释放资源
         if (fr != null)
         {
            fr.close();
         }
         if (input != null)
         {
            input.close();
         }
      } catch (IOException e)
      {
         throw new RuntimeException(e);
      }
   }
}
```

## 判断null

```java
public static void main(String[] args)
{
   // 定义一个集合为null
   List<String> a = null;
   System.out.println("定义一个集合null：" + ObjectUtil.isEmpty(a) + "----" + ObjectUtil.isNull(a));
   // 定义一个集合为空
   ArrayList<String> b = new ArrayList<>();
   System.out.println("定义一个集合为空：" + ObjectUtil.isEmpty(b) + "----" + ObjectUtil.isNull(b));
}
```

```
定义一个集合null：true----true
定义一个集合为空：true----false
```

## 检测Html标签

```java
public static void main(String[] args)
{
   // 检测是否包含html标签
   String escape = HtmlUtil.escape("<div>这是一段html</div>");
   // 将html标签进行转码的结果
   System.out.println(escape);
   // 将转码后的html进行解码并打印输出
   System.out.println(HtmlUtil.unescape(escape));
   // 正则表达式，验证是否包含html标签
   System.out.println(ReUtil.contains(HtmlUtil.RE_HTML_MARK, "<div>这是一段html</div>"));
}
```

打印结果：

```
&lt;div&gt;这是一段html&lt;/div&gt;
<div>这是一段html</div>
true
```

## 字符串处理

```java
public static void main(String[] args)
{
   String str = "test";
   // 判断是否为空字符串
   System.out.println(StrUtil.isEmpty(str));
   System.out.println(StrUtil.isNotEmpty(str));
   // 去除字符串的前后缀
   System.out.println(StrUtil.removeSuffix("a.jpg", ".jpg"));
   System.out.println(StrUtil.removePrefix("a.jpg", "a."));
   //格式化字符串
   String template = "这只是个占位符:{}";
   String str1 = StrUtil.format(template, "我是占位符");
   System.out.println("strUtil format:" + str1);
}
```

打印结果：

```
false
true
a
jpg
strUtil format:这只是个占位符:我是占位符
```

## Validator

```java
public static void main(String[] args)
{
   // 验证是否为 url
   System.out.println(Validator.isUrl("http://www.baidu.com"));
   // 验证是否为 mail
   System.out.println(Validator.isEmail("test@qq.com"));
   // 验证字符串是否为 全小写
   System.out.println(Validator.isLowerCase("Abc"));
}
```

打印结果：

```
true
true
false
```

## 转换

```java
public static void main(String[] args)
{
   int a = 1;
   // 转换为字符串
   String aStr = Convert.toStr(a);
   System.out.println(aStr);
   String b[] = {"1", "2", "3"};
   // 转换为指定类型数组
   Integer[] bArr = Convert.toIntArray(b);
   System.out.println(bArr);
   String dateStr = "2017-05-06";
   // 转换为日期对象
   Date date = Convert.toDate(dateStr);
   System.out.println(date);
   String strArr[] = {"A", "B", "C"};
   // 转换为列表（集合）
   List<String> list = Convert.toList(String.class, strArr);
   System.out.println(list);
}
```

打印结果：

```
1
[Ljava.lang.Integer;@2077d4de
Sat May 06 00:00:00 CST 2017
[A, B, C]
```

## 创建新集合

```java
public static void main(String[] args)
{
   String[] array = {"a", "b", "c", "d", "e"};
   // 数组转换为集合
   ArrayList<String> list = CollUtil.newArrayList(array);
   // 数组转字符串时拼接 逗号
   String join = CollUtil.join(list, ",");
   System.out.println(join);
   // 将以逗号拼接的字符串再转换为集合
   List<String> split = StrUtil.split(join, ",");
   System.out.println(split);
   // 创建新的Set, List
   HashSet<Object> newHashSet = CollUtil.newHashSet();
   ArrayList<Object> newList = CollUtil.newArrayList();
   // 创建新的Map
   HashMap<Object, Object> newMap = MapUtil.newHashMap();
   // 判空
   MapUtil.isEmpty(newMap);
   // 判非空
   MapUtil.isNotEmpty(newMap);
}
```

## 创建新的数组

```java
public static void main(String[] args)
{
   String newArray[] = ArrayUtil.newArray(String.class, 3);
   String sta[] = {"1", "2"};
   ArrayUtil.isNotEmpty(sta);
}
```

## Bean转换

```java
public static void main(String[] args)
{
   Test10 d = new Test10();
   d.setId(1);
   d.setName("刘桑");
   // Bean转Map
   Map<String, Object> map = BeanUtil.beanToMap(d);
   System.out.println("beanUtil bean to Map：" + map);
   // Map转Bean
   Test10 d1 = BeanUtil.mapToBean(map, Test10.class, false);
   System.out.println("beanUtil Map to bean：" + d1);
   // Bean属性拷贝
   Test10 d2 = new Test10();
   // d(有数据) ------> d2(无数据)
   BeanUtil.copyProperties(d, d2);
   System.out.println("beanUtil copy properties：" + d2);
}
```

打印结果：

```
beanUtil bean to Map：{id=1, name=刘桑}
beanUtil Map to bean：Test10{id=1, name='刘桑'}
beanUtil copy properties：Test10{id=1, name='刘桑'}
```

## 判断是否为内网

```java
public static void main(String[] args)
{
   // 判断是否为内网地址
   System.out.println(NetUtil.isInnerIP("192.168.3.35"));
}
```

