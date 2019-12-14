# 019. Remove Nth Node From End of List(M)
[019. Remove Nth Node From End of List](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

## 题目描述(中等)

Given a linked list, remove the n-th node from the end of list and return its head.

Example:
```
Given linked list: 1->2->3->4->5, and n = 2.
After removing the second node from the end, the linked list becomes 1->2->3->5.
```
**Note**:
> Given n will always be valid.

Follow up:

> Could you do this in one pass?

## 思路

1. 前后指针遍历 $n+(l-n)*2 = 2l - n$
    1. 单个指针遍历 $l+l-n = 2l - n$
2. 一次遍历，存储结点


## 解决方法

### 两次遍历

先让第一个指针遍历 n 步，然后再让它俩同时开始遍历，这样的话，当第一个指针到头的时候，第二个指针就离第一个指针有 n 的距离，所以第二个指针的位置就刚好是倒数第 n 个结点。

```java
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode start = new ListNode(0);
        start.next = head;
    	ListNode last = start;
        int num = 0;
    	while(num++<n) {
    		last = last.next;
    	}
    	ListNode first = start;
    	while(last.next!=null) {
    		last = last.next;
    		first = first.next;
    	}
    	first.next = first.next.next;
    	return start.next;
    }
```

先遍历一遍得到它的长度，之后用长度减去 n 就是要删除的结点的位置，然后遍历到结点的前一个位置

```java
    public ListNode removeNthFromEnd1(ListNode head, int n) {
        ListNode start = new ListNode(0);
        start.next = head;
    	ListNode first = start;
    	int len = 0;
        while(first.next!=null) {
    		first = first.next;
    		len++;
    	}
    	len-=n;
    	first = start;
    	while(len-->0) {
    		first = first.next;
    	}
    	first.next = first.next.next;
    	return start.next;
    }
```

时间复杂度: O(n)

空间复杂度：O(1)

### 一次遍历

第一次遍历链表确定长度的时候，顺便把每个结点存到数组里，空间换时间

```java
    public ListNode removeNthFromEnd2(ListNode head, int n) {
        ListNode start = new ListNode(0);
        start.next = head;
    	ListNode first = start;
    	List<ListNode> list = new ArrayList<>();
    	int len = 0;
        while(first.next!=null) {
        	list.add(first);
    		first = first.next;
    		len++;
    	}
    	len-=n;
    	first = list.get(len);
    	first.next = first.next.next;
    	return start.next;
    }
```
时间复杂度：O(L)

空间复杂度：O(L)




