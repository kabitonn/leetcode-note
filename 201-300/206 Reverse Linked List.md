# 206. Reverse Linked List(E)
[206. Reverse Linked List](https://leetcode-cn.com/problems/reverse-linked-list/)

## 题目描述\(简单\)

Reverse a singly linked list.

Example:

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

Follow up:

> A linked list can be reversed either iteratively or recursively. Could you implement both?

## 思路

1. 递归
2. 迭代

## 解决方法

### 递归

```java
    public ListNode reverseList(ListNode head) {
        if(head==null||head.next==null) {
            return head;
        }
        ListNode newHead = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return newHead;
    }
```
时间复杂度：O(n)，n 是链表的长度
空间复杂度：O(n)，由于使用递归，将会使用隐式栈空间。递归深度可能会达到 n 层。

### 迭代

```java
    public ListNode reverseList1(ListNode head) {
        if(head==null||head.next==null) {    return head;}
        ListNode pre = null;
        ListNode cur = head;
        while(cur.next!=null) {
            ListNode tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;
        }
        cur.next = pre;
        return cur;
    }
```

```java
    public ListNode reverseList3(ListNode head) {
        if(head==null||head.next==null) {    return head;}
        ListNode pre = null;
        ListNode cur = head;
        while(cur!=null) {
            ListNode tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
    }
```
时间复杂度：O(n)，n 是链表的长度，时间复杂度是
空间复杂度：O(1)。

### list倒序

```java
    public ListNode reverseList2(ListNode head) {
        List<ListNode> list = new ArrayList<>();
        ListNode cur = head;
        while(cur!=null) {
            list.add(cur);
            cur = cur.next;
        }
        int size = list.size();
        ListNode start = new  ListNode(0);
        cur = start;
        for(int i=size-1;i>=0;i--) {
            cur.next = list.get(i);
            cur = cur.next;
        }
        cur.next = null;
        return start.next;
    }
```



