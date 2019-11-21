# 234. Palindrome Linked List(E)
[234. Palindrome Linked List](https://leetcode-cn.com/problems/palindrome-linked-list/)

## 题目描述(简单)

Given a singly linked list, determine if it is a palindrome.

Example 1:
```
Input: 1->2
Output: false
```
Example 2:
```
Input: 1->2->2->1
Output: true
```
**Follow up:**
Could you do it in O(n) time and O(1) space?


## 思路

- 存储
- 倒序

## 解决方法

### 遍历存储


```java
    public boolean isPalindrome(ListNode head) {
        Deque<Integer> queue = new LinkedList<>();
        ListNode cur = head;
        while(cur!=null) {
            queue.add(cur.val);
            cur = cur.next;
        }
        int size = queue.size();
        for(int i=0;i<size/2;i++) {
            if(!queue.pop().equals(queue.pollLast())) {return false;}
        }
        return true;
    }
```


### Stack逆序，回文判断一半

栈保存链表全部节点，pop时和链表从头遍历比较

```java
    public boolean isPalindrome(ListNode head) {
        Stack<Integer> stack = new Stack<>();
        ListNode cur  = head;
        int count = 0;
        while(cur!=null) {
            stack.push(cur.val);
            cur = cur.next;
            count ++;
        }
        cur = head;
        for(int i=0;i<count/2;i++) {
            if(cur.val!=stack.pop()) {return false;}
            cur = cur.next;
        }
        return true;
    }
```

### 快慢指针 stack 前部翻转

栈保存链表前一半节点值，pop时和后一半遍历时比较

```java
    public boolean isPalindrome1_1(ListNode head) {
        if (head == null) {
            return true;
        }
        Stack<Integer> stack = new Stack<>();
        ListNode slow = head;
        ListNode fast = head;
        while (fast.next != null && fast.next.next != null) {
            stack.push(slow.val);
            slow = slow.next;
            fast = fast.next.next;
        }
        if (fast.next != null) {
            //链表长度为偶数,slow指向偏左中点仍未入栈
            //若链表长度为奇数，slow指向中间节点，中点左侧节点已入栈
            stack.push(slow.val);
        }
        //此时slow指向中点偏右侧
        slow = slow.next;
        while (!stack.isEmpty()) {
            if (slow.val != stack.pop()) {
                return false;
            }
            slow = slow.next;
        }
        return true;
    }

```


### 快慢指针-后部翻转


1. 快慢指针找中点
2. 中点后继节点逆序$$ \lfloor n/2 \rfloor $$个节点
3. 回文判断

```java
    public boolean isPalindrome(ListNode head) {
        if(head==null) {return true;}
        ListNode slow=head,fast=head;
        while(fast.next!=null&&fast.next.next!=null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode mid = slow;
        //mid为中间或中间偏左
        mid.next = reverseList(mid.next);
        ListNode left = head,right = mid.next;
        while(right!=null) {
            if(left.val!=right.val) {return false;}
            left = left.next;
            right = right.next;
        }

        return true;
    }
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
### 快慢指针-前部翻转

1. 前半段逆转 $$ \lfloor n/2 \rfloor $$ 个节点
2. 快慢指针找中点
3. 回文判断

```java
    public boolean isPalindrome(ListNode head) {
        ListNode slow = head, fast = head;
        ListNode prev = null;
        while(fast!=null&&fast.next!=null){
            fast = fast.next.next;
            ListNode next = slow.next;
            slow.next = prev;
            prev = slow;
            slow = next;
        }
        ListNode left = prev;
        //prev为中间偏左，slow为中间或中间偏右
        ListNode right = fast==null?slow:slow.next;

        while (right != null) {
            if (left.val != right.val) {
                return false;
            }
            left = left.next;
            right = right.next;
        }

        return true;
    }
```


## 快慢指针找中点

链表快慢指针找中点trick，判断fast.next和fast.next.next

* fast.next!=null时，链表长度为偶数，slow指向偏左中点
* fast.next==null时，链表长度为奇数，slow指向中点结点

链表快慢指针找中点trick，判断fast和fast.next
(想要获取中点左侧或偏左结点需要记录前一个指针)
* fast==null时，链表长度为偶数时，slow指向偏右中点
* fast!=null时，链表长度为奇数时，slow指向中点结点

链表快慢指针找中点trick，fast先走一步，判断fast和fast.next

* fast!=null时，链表长度为偶数时，slow指向偏左中点
* fast==null时，链表长度为奇数时，slow指向中点结点


第一种
```java
        ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        if (fast.next != null) {
            //链表长度为偶数,slow指向偏左中点
        }else{
            //若链表长度为奇数，slow指向中间节点
        }
```

第二种
```java
        ListNode slow = head, fast = head;
        ListNode prev = null;
        while (fast.next != null && fast.next.next != null) {
            prev = slow;
            fast = fast.next.next;
            slow = slow.next;
        }
        if (fast== null) {
            //链表长度为偶数,slow指向偏右中点,prev指向偏左中点
        }else{
            //若链表长度为奇数，slow指向中间节点,prev指向中间节点左侧节点
        }
```

第三种

```java
        ListNode slow = head, fast = head.next;
        while (fast.next != null && fast.next.next != null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        if (fast!= null) {
            //链表长度为偶数,slow指向偏左中点,
        }else{
            //若链表长度为奇数，slow指向中间节点,
        }
```





