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


---


---


---


---


---


---
