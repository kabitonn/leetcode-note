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

- 非环部分长度为x
- 环的长度为n
- 环首与快慢指针的环内相遇点距离y

![](/assets/101-200/142-s-1-1.png)

设 slow 指针走过的距离为 t，那么 fast 指针走过的一定是 slow 指针的 2 倍，也就是 2t

slow 指针从 head 出发走了 x 的距离到达入口点，然后可能走了 k1 圈，然后再次回到入口点，再走了 y 的距离到达相遇点和 fast 指针相遇。

`t = x + k1 * n + y ( k1 = 0, 0 <= y < n)`

> 实际上k1=0，快慢指针相遇时慢指针必然未走完一圈，假设slow到入口点时，最坏情况fast在slow前一个结点，相差n-1的距离，在slow走n-1步后相遇，此时slow并未走完一圈。

fast 指针从 head 出发走了 x 的距离到达入口点，然后可能走了 k2 圈，然后再次回到入口点，再走了 y 的距离到达相遇点和 slow 指针相遇。

`2t = x + k2 * n + y ( k2 >= 1 )`

> 快指针环内走的路程必然超过一圈

上边两个等式代入消去t，可以得到

`k2 * n = x + y`

移项可得

`x = k2 * n - y`

n - y 就是从相遇点到入口点的距离， (k2 - 1) * n  就是转 (k2 - 1) 圈

从相遇点走到入口点，然后转 (k2 - 1) 圈后再次回到入口点的这段时间，刚好就等于从 head 走向入口点的时间，完成二次相遇。




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



