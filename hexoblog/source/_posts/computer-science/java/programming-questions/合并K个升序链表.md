---
title: 合并K个升序链表
categories:
    - [计算机学科,java,编程题]
tags:
    - 分而治之
    - 编程题
---

# 合并K个升序链表

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入：lists = []
输出：[]
```

**示例 3：**

```
输入：lists = [[]]
输出：[]
```

 

**提示：**

-  `k == lists.length`
-  `0 <= k <= 10^4`
-  `0 <= lists[i].length <= 500`
-  `-10^4 <= lists[i][j] <= 10^4`
-  `lists[i]` 按 **升序** 排列
-  `lists[i].length` 的总和不超过 `10^4`

**思路分析**：

多个节点合并，我们可以先看做是两个两个进行合并排序来简化问题并解决

![image-20240103205628725](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401032056809.png)

最终的合并结果如下图所示：

![image-20240103210710426](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401032107504.png)

代码：

```java
public ListNode mergeKLists(ListNode[] lists) {
      if(lists.length == 0)
      {
        return null;
      }
      return splitar(lists, 0, lists.length - 1);
    }
    public ListNode mergeTwoLists(ListNode p1, ListNode p2)
    {
        if(p1 == null)
        {
          return p2;
        }
        if(p2 == null)
        {
          return p1;
        }
        if(p1.val < p2.val)
        {
          p1.next = mergeTwoLists(p1.next, p2);
          return p1;
        }else
        {
          p2.next = mergeTwoLists(p1, p2.next);
          return p2;
        }
    }
    // 返回合并后的链表,i,j代表左右边界
    public ListNode splitar(ListNode lists[], int i, int j)
    {
      if(i == j)
      {
        return lists[i];
      }
      // 中间点
      int m = (i + j) >>> 1;
      // 从i=0，到m=2 拆分左边的合并链表
      ListNode left = splitar(lists, i, m);
      // 从 m+1=3，到j=长度-1  拆分右边的合并链表
      ListNode right = splitar(lists, m + 1, j);
      return mergeTwoLists(left, right);
    }
```

