# 142. Linked List Cycle II\(M\)

## 题目描述\(中等\)

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position \(0-indexed\) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

**Note**: Do not modify the linked list.

Example 1:

```
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

![](/assets/101-200/142-p-1.png)

Example 2:

```
Input: head = [1,2], pos = 0
Output: tail connects to node index 0
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

![](/assets/101-200/142-p-2.png)

Example 3:

```
Input: head = [1], pos = -1
Output: no cycle
Explanation: There is no cycle in the linked list.
```

![](/assets/101-200/142-p-3.png)

**Follow-up**:  
Can you solve it without using extra space?

## 思路

* 快慢指针
* 哈希

## 解决方法

### 快慢指针



![](/assets/101-200/142-s-1-1.png)



```java
    public ListNode detectCycle(ListNode head) {
        if (head==null) {    return null;}
        ListNode fast = head;
        ListNode slow = head;
        while(fast.next!=null&&fast.next.next!=null) {
            fast = fast.next.next;
            slow = slow.next;
            if(fast == slow) {
                fast = head;
                while(fast != slow) {
                    fast = fast.next;
                    slow = slow.next;
                }
                return fast;
            }
        }
        return null;
    }
```

### 哈希

遍历链表，并且把遍历过的节点用 HashSet 存起来，如果遍历过程中又遇到了之前的节点，说明这个节点就是我们要找到入口点。如果到达了 null 就说明没有环。

```java
    public ListNode detectCycle1(ListNode head) {
        HashSet<ListNode> set = new HashSet<>();
        while (head != null) {
            set.add(head);
            head = head.next;
            if (set.contains(head)) {
                return head;
            }
        }
        return null;
    }
```



