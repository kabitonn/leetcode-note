# 083. Remove Duplicates from Sorted List
[083. Remove Duplicates from Sorted List](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

## 题目描述(简单)

Given a sorted linked list, delete all duplicates such that each element appear only once.

Example 1:
```
Input: 1->1->2
Output: 1->2
```
Example 2:
```
Input: 1->1->2->3->3
Output: 1->2->3
```
## 思路

1. 迭代
2. 递归

## 解决方法

### 迭代

找到一个，删除一个；相等的话就删除下一个节点，不相等就后移

```java
    public ListNode deleteDuplicates(ListNode head) {
        ListNode cur = head;
        while(cur!=null&&cur.next!=null) {
        	ListNode next = cur.next;
        	if(cur.val == next.val) {
        		cur.next = next.next;
        	}else {
				cur = cur.next;
			}
        }
        return head;
    }
```

### 递归

```java
    public ListNode deleteDuplicates(ListNode head) {
    	if (head == null) {return null;}
        if (head.next!=null && head.val == head.next.val) {
        	head.next = head.next.next;
        	head = deleteDuplicates(head);
        }else {
			head.next = deleteDuplicates(head.next);
		}
        return head;
    }
```
### 迭代2

遇到重复数字，跳过后续所有

```java
    public ListNode deleteDuplicates(ListNode head) {
        ListNode cur = head;
        while (cur != null && cur.next != null) {
            while (cur.next != null && cur.val == cur.next.val) {
                cur.next = cur.next.next;
            }
            cur = cur.next;

        }
        return head;
    }
```

```java
	public ListNode deleteDuplicates(ListNode head) {
		ListNode start = new ListNode(0);
		start.next = head;
		ListNode pre = start;
		ListNode cur = pre.next;
		while(cur!=null&&cur.next!=null) {
			ListNode next = cur.next;
			boolean equal = false;
			while(cur.next!=null && cur.val == cur.next.val){
				cur = cur.next;
				equal = true;
			}
			if(equal){
				pre.next = cur;
			}
			pre = cur;
			cur = cur.next;
		}
		return start.next;
	}
```


### 递归2


```java
    public ListNode deleteDuplicates(ListNode head) {
    	if (head == null) {return null;}
		//头结点和后一个时候相等
        if (head.next!=null && head.val == head.next.val) {
			//跳过所有重复数字
        	while(head.next!=null && head.val == head.next.val) {
        		head.next = head.next.next;
        	}
        	head = deleteDuplicates(head);
        }else {
			head.next = deleteDuplicates(head.next);
		}
        return head;
    }
```




