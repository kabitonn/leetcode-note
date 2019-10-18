# 021. Merge Two Sorted Lists(E)
[021. Merge Two Sorted Lists](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

## 题目描述\(简单\)

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

Example:

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```

## 思路

1. 遍历链表，取当前较小值，最后连接非空链表部分
2. 递归：选定当前节点，递归处理剩余节点

## 解决方法

### 迭代

```java
    public ListNode mergeTwoLists1(ListNode l1, ListNode l2) {
        ListNode start = new ListNode(0);
        ListNode cur=start;
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                cur.next = l1;
                cur = cur.next;
                l1 = l1.next;
            } else {
                cur.next = l2;
                cur = cur.next;
                l2 = l2.next;
            }
        }
        if(l1==null){
            cur.next=l2;
        }
        if(l2==null){
            cur.next=l1;
        } 
        return start.next;
    }
```

### 递归

```java
    public  ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null) {
            return l2;
        }
        if(l2==null) {
            return l1;
        }
        if(l1.val<l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        }
        else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
```



