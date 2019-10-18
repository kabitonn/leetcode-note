## [234. Palindrome Linked List](https://leetcode-cn.com/problems/palindrome-linked-list/)

## 1. 题目描述(简单)

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


## 2. 思路

## 3. 解决方法

### 3.1 遍历存储


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


### 3.2 Stack逆序后半部分


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
### 3.3 快慢指针-后部翻转


```java
    public boolean isPalindrome(ListNode head) {
        if(head==null) {return true;}
        ListNode slow=head,fast=head;
        while(fast.next!=null&&fast.next.next!=null) {
            fast = fast.next.next;
            slow = slow.next;
        }
        ListNode mid = slow;//mid为中间或中间偏左
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
        ListNode left = prev;//prev为中间偏左，slow为中间或中间偏右
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





