---
title: 二叉树Z层遍历
categories:
    - [计算机学科,java,编程题]
tags:
    - java
    - 编程题
    - 二叉树
    - 层序遍历
    - 计算机学科
---

# 二叉树Z层遍历

## 介绍：

比如说如下有一个二叉树：

```
	  1
	 / \
   2	 3
  /\    /\
 4	5	6	7
/\
8 9
```

那么上面的二叉树的遍历流程应该是下面的：

```
1
32
4567
98
```

它的遍历层序是一个Z字的形状

## 该题目在 leetcode 中的 介绍如下：

给你二叉树的根节点 `root` ，返回其节点值的 **锯齿形层序遍历** 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

 

**示例 1：**

![img](https://raw.githubusercontent.com/PigPigLetsGo/imeages/master/202401181008088.jpeg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[20,9],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

 

**提示：**

-  树中节点数目在范围 `[0, 2000]` 内
-  `-100 <= Node.val <= 100`

## 代码：

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
      List<List<Integer>> res = new ArrayList<>();
      Queue<TreeNode> queue = new ArrayDeque<>();
      boolean flag = true; // 表示 刚开始是 奇数层
      if(root != null)
      {
      queue.add(root);
      }
      int c1 = 1;
      while(! queue.isEmpty())
      {
        int c2 = 0;
        LinkedList<Integer> list = new LinkedList<>();
        for(int i = 0; i < c1; i++)
        {
          TreeNode node = queue.poll();
          if(flag)
          {
            list.offerLast(node.val);
          }else
          {
            list.offerFirst(node.val);
          }
          if(node.left != null)
          {
            queue.add(node.left);
            c2++;
          }
          if(node.right != null)
          {
            queue.add(node.right);
            c2++;
          }
        }
        flag = ! flag; // 由奇数层 切换到 偶数层，相反 由偶数层 切换到 奇数层
        c1 = c2;
        res.add(list);
      }
      return res;
    }
}
```

