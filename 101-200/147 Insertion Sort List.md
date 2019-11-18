# 147. Insertion Sort List(M)


[](https://leetcode-cn.com/problems/insertion-sort-list/)


## 题目描述(中等)

Sort a linked list using insertion sort.


A graphical example of insertion sort. The partial sorted list (black) initially contains only the first element in the list.
With each iteration one element (red) is removed from the input data and inserted in-place into the sorted list

![](../assets/101-200/147-p-1.gif)

**Algorithm of Insertion Sort**:

1. Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
3. It repeats until no input elements remain.

Example 1:
```
Input: 4->2->1->3
Output: 1->2->3->4
```
Example 2:
```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
```

## 思路

- 前向插入
- 前向插入有序链表
- 判断插入必要

## 解决方法



### 前向插入

```java
    public ListNode insertionSortList(ListNode head) {
        int sorted = 1;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode node = head, next, pre;
        while (node != null) {
            next = node.next;
            node.next = null;
            pre = dummy;
            int i = 0;
            while (i < sorted && pre.next != null && pre.next.val < node.val) {
                i++;
                pre = pre.next;
            }
            if (pre.next != node) {
                node.next = pre.next;
                pre.next = node;
            }
            node = next;
            sorted++;
        }
        return dummy.next;
    }
```

### 前向插入有序链表

```java
    public ListNode insertionSortList0(ListNode head) {
        ListNode dummy = new ListNode(0);
        ListNode cur = head, pre = dummy;
        ListNode next;
        while (cur != null) {
            next = cur.next;
            while (pre.next != null && pre.next.val < cur.val) {
                pre = pre.next;
            }
            cur.next = pre.next;
            pre.next = cur;
            pre = dummy;
            cur = next;
        }
        return dummy.next;
    }

```

### 判断插入必要

```java
    public ListNode insertionSortList1(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode cur = head, pre = dummy;
        ListNode next;
        while (cur != null) {
            // 记录下一个要插入排序的值
            next = cur.next;
            // 只有 cur.next 比 cur 小才需要向前寻找插入点
            if (next != null && next.val < cur.val) {
                while (pre.next.val < next.val) {
                    pre = pre.next;
                }
                //将next插入在pre后,并将next的后继连接在cur的后面
                cur.next = next.next;
                next.next = pre.next;
                pre.next = next;
                pre = dummy;
            } else {
                // 都这直接把 cur 指针指向到下一个
                cur = next;
            }
        }
        return dummy.next;
    }

```

