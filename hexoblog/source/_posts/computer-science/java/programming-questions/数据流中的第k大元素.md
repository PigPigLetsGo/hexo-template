---
title: 数据流中的第K个最大元素
categories:
    - [计算机学科,java,编程题]
tags:
    - java
    - 编程题
    - 堆
    - 小顶堆
---

# 数据流中的第K个最大元素

设计一个找到数据流中第 `k` 大元素的类（class）。注意是排序后的第 `k` 大元素，不是第 `k` 个不同的元素。

请实现 `KthLargest` 类：

-  `KthLargest(int k, int[] nums)` 使用整数 `k` 和整数流 `nums` 初始化对象。
-  `int add(int val)` 将 `val` 插入数据流 `nums` 后，返回当前数据流中第 `k` 大的元素。

**示例：**

```
输入：
["KthLargest", "add", "add", "add", "add", "add"]
[[3, [4, 5, 8, 2]], [3], [5], [10], [9], [4]]
输出：
[null, 4, 5, 5, 8, 8]

解释：
KthLargest kthLargest = new KthLargest(3, [4, 5, 8, 2]);
kthLargest.add(3);   // return 4
kthLargest.add(5);   // return 5
kthLargest.add(10);  // return 5
kthLargest.add(9);   // return 8
kthLargest.add(4);   // return 8
```

**提示：**

-  `1 <= k <= 104`
-  `0 <= nums.length <= 104`
-  `-104 <= nums[i] <= 104`
-  `-104 <= val <= 104`
-  最多调用 `add` 方法 `104` 次
-  题目数据保证，在查找第 `k` 大元素时，数组中至少有 `k` 个元素

## 题目分析：

它主要让你实现两个方法，第一个方法就是一个构造方法主要是用来初始化的，构造方法的参数 会给告诉你要求第k大的元素并且会给一个初始数组

```java
public KthLargest(int k, int[] nums) {
   
}
```

重要的是下面的add方法

```java
public int add(int val) {
   return -1;
}
```

这个方法它会被不断地调用，传过来的参数就是用来模拟数据流中不断新收集到的数据，返回值就是返回第k大的数据

## 完整代码如下：

### 实现小堆顶的代码：

```java
public class MinHeap
{
    public int array[];
    public int size;

    public MinHeap(int capacity)
    {
        this.array = new int[capacity];
    }

    public boolean offer(int t)
    {
        // 判断 堆 是否满了
        if(isFull())
            return false;
        // 调用up方法 重新调整 小顶堆的特性
        up(t, size);
        // 元素个数 自增
        size++;
        return true;
    }

    public void up(int offered, int index)
    {
        // 获取当前 index 索引位置
        int child = index;
        while(child > 0)
        {
            // 根据 当前child位置 通过公式 获取 它的父节点位置
            int parent = (child - 1) / 2;
            // 判断 传入的元素值 是否 小于 parent索引 位置的值
            if(offered < array[parent])
                // 如果小那么就将 父节点的值 向下移动
                array[child] = array[parent];
            else
                // 如果 传入的值 大于 父节点的值 那么就不需要操作 直接 跳出循环
                break;
            // 将 child 的索引 赋值为 父节点的索引值
            child = parent;
        }
        // 将 当前child 的索引位置赋值为 传入的值
        array[child] = offered;
    }

    public void replace(int replaced)
    {
        array[0] = replaced;
        down(0);
    }

    public int peek()
    {
        return array[0];
    }

    public void swap(int i, int j)
    {
        int t = array[i];
        array[i] = array[j];
        array[j] = t;
    }

    public void down(int parent)
    {
        int left = parent * 2 + 1;
        int right = left + 1;
        int min = parent;
        if(left < size && array[min] > array[left])
            min = left;
        if(right < size && array[min] > array[right])
            min = right;
        if(min != parent)
        {
            swap(min, parent);
            down(min);
        }
    }

    public boolean isFull()
    {
        return size == array.length;
    }
}
```

### 解决题目问题代码：

```java
private MinHeap heap;

public static void main(String ... args)
{
   // 给你一个 数组 并且 要求你 求出 第3 大的数据
   Tests tests = new Tests(3, new int[]{4, 5, 8, 2});
   // 我们使用 小顶堆实现
   // 我们只保留最大的3 个元素 4 5 8
   // 小顶堆 4 5 8 留 最大 3 个
   // 第一次 3 比 小顶堆中的 4 小 所以 返回的是 4
   System.out.println(tests.add(3)); // [4 5 8] 4
   // 第二次 5 比 小顶堆中的 4 大 那么就 替换掉 4 此时小顶堆就换成了 [5 8 10] 返回 5
   System.out.println(tests.add(5)); // [5 5 8] 5
   // 第三次 10 比 小顶堆中所有的数据都大 那么就 将小顶堆 替换为 [5 8 10] 第3 大的还是 5
   // PS: 小顶堆 10比 所有数据大 它跟 堆顶元素 5 替换后 重新调整就变成了 5 8 10 了
   System.out.println(tests.add(10)); // [5 8 10] 5
   // 第四次 9 比 小顶堆中 的 8大 那么就将小堆顶 替换为 [8 9 10] 第3 大的是 8
   System.out.println(tests.add(9)); // [8 9 10] 8
   // 第五次 4 没有比 小顶堆中的大的 不作操作 , 返回第3 大的还是 8
   System.out.println(tests.add(4)); // [8 9 10] 8
}

public Tests(int k, int nums[])
{
   heap = new MinHeap(k);
   // 不断将 nums 中的元素 调用 add 进行添加
   for (int num : nums)
   {
      add(num);
   }
}

// 此方法会被不断调用，模拟数据流中新来的元素
public int add(int val)
{
   // 先判断如果 堆 没有满 那么就 先将 构造函数的数组元素 添加到 堆中 后续进行 其它数据的判断与替换
   if(! heap.isFull())
      heap.offer(val);
   // 如果传入的数据 比 堆顶 元素数据大 那么我们就需要做 替换
   else if(val > heap.peek())
      // 将当前 传入的数据 与 堆顶元素进行替换 并且 重新调整 小顶堆的特性
      heap.replace(val);
   // 返回第k大的数据 也就是堆顶的元素
   return heap.peek();
}
```

打印结果：

```
4
5
5
8
8
```

