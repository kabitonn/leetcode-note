# 203. Remove Linked List Elements(E)
[203. Remove Linked List Elements](https://leetcode-cn.com/problems/remove-linked-list-elements/)

## 题目描述(简单)

Remove all elements from a linked list of integers that have value val.

Example:
```
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5
```

## 思路

- 迭代
- 递归


## 解决方法

### 迭代双指针 虚拟头结点


```java
    public ListNode removeElements1(ListNode head, int val) {
        ListNode start = new ListNode(0);
        start.next = head;
        ListNode prev = start;
        ListNode cur = prev.next;
        while (cur != null) {
            if (cur.val == val) {
                prev.next = cur.next;
            } else {
                prev = cur;
            }
            cur = cur.next;
        }
        return start.next;
    }
```
### 不使用虚拟头结点


```java
    public ListNode removeElements2(ListNode head, int val) {
        while(head!=null&&head.val==val){
            head = head.next;
        }
        if(head==null){return head;}
        ListNode prev = head;
        while(prev.next!=null){
            if(prev.next.val==val){
                prev.next = prev.next.next;
            }
            else {
                prev = prev.next;
            }
        }
        return head;
    }
```



### 递归


```java
    public ListNode removeElements(ListNode head, int val) {
        if(head==null){return null;}
        head.next = removeElements(head.next,val);
        if(head.val == val){return  head.next;}
        else{return head;}
    }
```


