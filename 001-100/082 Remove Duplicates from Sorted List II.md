# 082. Remove Duplicates from Sorted List II(M)

[082. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)


## 题目描述(中等)

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

Example 1:
```
Input: 1->2->3->3->4->4->5
Output: 1->2->5
```
Example 2:
```
Input: 1->1->1->2->3
Output: 2->3

```

## 思路

## 解决方法


### 迭代 标记

```java
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode start = new ListNode(0);
        ListNode pre = start, cur = head;
        while (cur != null) {
            boolean unique = true;
            while (cur.next != null && cur.val == cur.next.val) {
                cur = cur.next;
                unique = false;
            }
            if (unique) {
                pre.next = cur;
                pre = cur;
            }
            cur = cur.next;
        }
        //防止pre后面还有重复的节点
        pre.next = null;
        return start.next;
    }
```


```java
    public ListNode deleteDuplicates1(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode start = new ListNode(0);
        start.next = head;
        ListNode pre = start, cur = head;
        while (cur != null) {
            boolean dup = false;
            while (cur.next != null && cur.val == cur.next.val) {
                cur = cur.next;
                dup = true;
            }
            if (dup) {
                pre.next = cur.next;
            } else {
                pre = cur;
            }
            cur = cur.next;
        }

        return start.next;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。


### 迭代 无标记

pre.next == cur 表示cur之前没有重复数字，若不等表示之前有重复数字

```java
    public ListNode deleteDuplicates2(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode start = new ListNode(0);
        start.next = head;
        ListNode pre = start, cur = head;
        while (cur != null) {
            while (cur.next != null && cur.val == cur.next.val) {
                cur = cur.next;
            }
            if (pre.next == cur) {
                pre = cur;
            } else {
                pre.next = cur.next;
            }
            cur = cur.next;
        }
        return start.next;
    }

```

时间复杂度：O(n)。

空间复杂度：O(1)。


### 递归

主要分两种情况，头结点和后边的节点相等，头结点和后边的节点不相等。

```java
    public ListNode deleteDuplicates(ListNode head) {
        if (head == null ) {
            return null;
        }
        //如果头结点和后边的节点相等
        if (head.next != null && head.val == head.next.val) {
            //跳过所有重复数字
            while (head.next != null && head.val == head.next.val) {
                head = head.next;
            }
            //将所有重复数字去掉后，进入递归
            return deleteDuplicates(head.next);
            //头结点和后边的节点不相等
        } else {
            //保留头结点，后边的所有节点进入递归
            head.next = deleteDuplicates(head.next);
        }
        //返回头结点
        return head;
    }
```