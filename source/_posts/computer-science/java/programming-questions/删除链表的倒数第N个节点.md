---
title: 删除链表的倒数第N个节点
categories:
    - [计算机学科,java,编程题]
tags:
    - 快慢指针
---

# 删除链表的倒数第N个节点

**题目要求**：

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401021030015.jpeg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

 

**提示：**

-  链表中结点的数目为 `sz`
-  `1 <= sz <= 30`
-  `0 <= Node.val <= 100`
-  `1 <= n <= sz`

## 双指针(快慢指针)：

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode s = new ListNode(-1, head);
        ListNode p1 = s;
        ListNode p2 = s;
        for(int i = 0; i < n + 1; i++)
        {
          // 让p2走n + 1步
          p2 = p2.next;
        }
        // 循环判断p2指针是否为null
        while(p2 != null)
        {
          //如果不为Null就让两个指针同时往前移动直到p2等于null
          p1 = p1.next;
          p2 = p2.next;
        }
        // 将p1当前的节点指向它的下一个下一个 跳过要被移除的元素
        p1.next = p1.next.next;
        // 返回哨兵的后一个节点开始
        return s.next;
    }
```

**思路分析图**：

![image-20240102105128266](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401021051336.png)