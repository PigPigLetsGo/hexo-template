---
title: hutool-深克隆
categories:
   - [计算机学科,java,第三方库]
tags:
   - 计算机学科
   - java
   - 第三方库
   - hutool
---

# hutool-深克隆

**导入依赖**：

```xml
<dependency>
   <groupId>cn.hutool</groupId>
   <artifactId>hutool-all</artifactId>
   <version>5.7.22</version>
</dependency>
```

如果想要使用hutool来实现对象的深克隆，可以使用如下：

```java
ObjectUtil.cloneByStream(obj)
```

PS：前提是这个对象必须实现Serializable接口才行，否则就会报错 空指针异常。

ObjectUtil同样提供一些静态方法：clone(obj)，clonefPossible(obj)用于简化克隆调用，详细的说明请查看核心类的相关文档。

**代码使用案例如下**：

```java
import cn.hutool.core.util.ObjectUtil;

import java.io.Serializable;
import java.util.Arrays;

public class Test implements Serializable /*Cloneable*/
{
    private String name = "miaomaio";
    private int age = 18;
    // private int arr[] = new int[]{1, 2, 3, 4, 5, 6};

//    @Override
//    protected Test clone()
//    {
//        try
//        {
//            return (Test) super.clone();
//        } catch (CloneNotSupportedException e)
//        {
//            throw new RuntimeException(e);
//        }
//    }

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

//    public int[] getArr()
//    {
//        return arr;
//    }
//
//    public void setArr(int[] arr)
//    {
//        this.arr = arr;
//    }

    @Override
    public String toString()
    {
        return "Test{" +
                "name='" + name + '\'' +
                ", age=" + age +
//                ", arr=" + Arrays.toString(arr) +
                '}';
    }

    public static void main(String[] args)
    {
        Test test1 = new Test();
        test1.setName("张三");
        test1.setAge(18);
//        Test test2 = test1.clone();
        Test test2 = ObjectUtil.cloneByStream(test1);
        test2.setName("李四");
        test2.setAge(20);
//        test1.arr[0] = 9;
        System.out.println("test1 : " + test1);
        System.out.println("test2 : " + test2);
    }
}
```

**测试顺序**：1. 先进行简单的对象引用测试，2. 使用实现Serializable接口来进行浅拷贝，此时解开数组的注释就会发现浅拷贝不行事儿了，3. 使用hutool提供的ObjectUtil.cloneByStream来实现深拷贝，发现深拷贝可以解决数组问题