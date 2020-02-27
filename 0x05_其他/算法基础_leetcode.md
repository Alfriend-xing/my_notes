>内容整理自[LeetCode](https://leetcode-cn.com/articles/)

---
## 反转链表

### 题目

```
反转一个单链表。
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
```

### 思路

迭代：遍历链表，获得当前节点、下一节点、上一节点，之后进行操作将当前节点指向上一节点，更新上一节点为当前节点，更新当前节点为下一节点


### 代码

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```
---
## 爬楼梯

### 题目

```
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。给定 n 是一个正整数。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

示例 1：

输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶

示例 2：

输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

### 思路

动态规划：不难发现，这个问题可以被分解为一些包含最优子结构的子问题，即它的最优解可以从其子问题的最优解来有效地构建，我们可以使用动态规划来解决这一问题。

第 ii 阶可以由以下两种方法得到：
- 在第 (i-1)阶后向上爬一阶。
- 在第 (i-2)阶后向上爬 22 阶。(这里如果i-2阶后选择迈两个1阶上楼则与i-1阶爬1阶的情况重复，所以不计)

所以到达第 ii 阶的方法总数就是到第 (i-1)(i−1) 阶和第 (i-2)(i−2) 阶的方法数之和。自顶向下的递归时间复杂度为2^n，使用缓存或者备忘录可以降低时间复杂度。使用自底向下可简化时空复杂度。

### 代码

```java
public class Solution {
    public int climbStairs(int n) {
        if (n == 1) {
            return 1;
        }
        int first = 1;
        int second = 2;
        for (int i = 3; i <= n; i++) {
            int third = first + second;
            first = second;
            second = third;
        }
        return second;
    }
}
```
---
## 最长同值路径

### 题目

```
给定一个二叉树，找到最长的路径，这个路径中的每个节点具有相同值。 这条路径可以经过也可以不经过根节点。

注意：两个节点之间的路径长度由它们之间的边数表示。

示例 1:

输入:

              5
             / \
            4   5
           / \   \
          1   1   5
输出:

2
示例 2:

输入:

              1
             / \
            4   5
           / \   \
          4   4   5
输出:

2
```

### 思路

递归:以递归方式遍历左子树和右子树，如果受到的节点为空直接返回0，之后获取当前节点左右子树的递归值，然后判断左节点不为空且左节点值等于当前节点，则左子树递归值加一后赋值给左侧路径长度，之所以不把左侧递归值直接当作左侧路径长度，是因为递归值可能是左子树自身的路径长度，只有与当前节点值相同才能算作当前节点的路径。右侧相同操作。最长路径取 历史最长 和 左右路径和 中最大值。递归函数返回左右路径中最大值。

### 代码

```java
class Solution {
    int ans;
    public int longestUnivaluePath(TreeNode root) {
        ans = 0;
        arrowLength(root);
        return ans;
    }
    public int arrowLength(TreeNode node) {
        if (node == null) return 0;
        int left = arrowLength(node.left)
        int right = arrowLength(node.right);
        int arrowLeft = 0, arrowRight = 0;
        if (node.left != null && node.left.val == node.val) {
            arrowLeft += left + 1;
        }
        if (node.right != null && node.right.val == node.val) {
            arrowRight += right + 1;
        }
        ans = Math.max(ans, arrowLeft + arrowRight);
        return Math.max(arrowLeft, arrowRight);
    }
}
```
---
## 

### 题目

```
```

### 思路

### 代码

```java

```
---
## 

### 题目

```
```

### 思路

### 代码

```java

```
---
## 

### 题目

```
```

### 思路

### 代码

```java

```
---
## 

### 题目

```
```

### 思路

### 代码

```java

```

