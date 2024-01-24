---
title: 数组中的第K个最大元素
categories:
    - [计算机学科,java,编程题]
tags:
    - java
    - 编程题
    - 堆
    - 小顶堆
---

# 数组中的第K个最大元素

给定整数数组 `nums` 和整数 `k`，请返回数组中第 `**k**` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

**示例 1:**

```
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4
```

**提示：**

-  `1 <= k <= nums.length <= 105`
-  `-104 <= nums[i] <= 104`

## 使用小顶堆来解决问题

#### 思路分析：

比如说 如下案例：

```
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

找 第 2 大的元素，我们就把 前两个元素 先加入到小顶堆，如果 k = 4那么我们就把 前面4个元素加入到 小顶堆，我们先看前两个的

![image-20240122165114007](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221651078.png)

后续的元素 我们不直接添加，而是进行判断 如果 要添加的 元素比 堆顶元素小 那么什么事儿也不做，如果比 堆顶元素大 那么就采用 replace 替换掉 堆顶元素，然后重新调整 小顶堆

我们添加 1 的时候什么也不做 当我们添加 5 的时候 先 replace 然后 调整小顶堆 就成了如下图所示：

![image-20240122165408503](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221654576.png)

看到这里差不多就明白了 其实 队里 留的 就是 2 个 最大的元素

下次 6 的时候 它比 堆顶元素 3 大，那么就 replace 然后 调整 小顶堆，此时就如下图所示：

![image-20240122165540459](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401221655535.png)

剩下一个 4 没有 5 大 那么直接 什么也不做

最终 经过上述的 算法运算后 我们小堆顶中 就剩下了 两个最大的 元素 那么这两个 最大的元素里 谁是 第二大的呢？

堆顶元素就是第二大的，所以最终我们只要返回 堆顶元素就可以了

## 代码实现：

实现小顶堆类

```java
/**
 * @author Dkx
 * @version 1.0
 * @2024/1/2217:05
 * @function
 * @comment
 */
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

解决题目问题 类

```java
/**
     * 思路
     * 1. 向小顶堆放入前k 个元素
     * 2. 剩余元素
     *  - 若 <= 堆顶元素，则略过
     *  - 若 > 堆顶元素，则替换堆顶元素
     * 3. 这样小顶堆始终保留的是到目前为止，前K大的元素
     * 4. 循环结束，堆顶元素即为第K大元素
     */
@Test
public void test()
{
   // int arr[] = {3, 2, 1, 5, 6, 4};
   int arr[] = {3, 2, 3, 1, 2, 4, 5, 5, 6};
   // int k = 2;
   int k = 4;
   MinHeap heap = new MinHeap(k);
   for(var i = 0; i < k; i++)
      heap.offer(arr[i]);
   // 循环剩余元素
   for(var i = k; i < arr.length; i++)
      if(arr[i] > heap.peek())
         heap.replace(arr[i]);
   System.out.println(heap.peek());
}
```

打印结果：

```
4
```

