# 141. Linked List Cycle(E)
[141. Linked List Cycle](https://leetcode-cn.com/problems/linked-list-cycle/)

## 题目描述\(简单\)

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer pos which represents the position \(0-indexed\) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

Example 1:

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

![](../assets/leetcode-note/101-200/141-problem-1.png)

Example 2:

```
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the first node.
```

![](../assets/leetcode-note/101-200/141-problem-2.png)

Example 3:

```
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
```

![](../assets/leetcode-note/101-200/141-problem-3.png)

Follow up:

> Can you solve it using O\(1\) \(i.e. constant\) memory?

## 思路

1. 双指针
2. 哈希

## 解决方法

### 快慢指针相交


```java
    public boolean hasCycle(ListNode head) {
    	if(head==null) {return false;}
        ListNode fast = head;
        ListNode slow = head;
        while(fast!=null&&fast.next!=null) {
        	slow = slow.next;
        	fast = fast.next.next;
        	if(slow == fast) {return true;}
        }
        return false;
    }
```

时间复杂度
    - 链表中不存在环：快指针将会首先到达尾部，其时间取决于列表的长度，也就是 O(n)。
    - 链表中存在环：我们将慢指针的移动过程划分为两个阶段：O(n+k)
        - 非环部分
        - 环形部分
空间复杂度：O(1)，我们只使用了慢指针和快指针两个结点，所以空间复杂度为 O(1)

### 哈希集


```java
    public boolean hasCycle1(ListNode head) {
    	ListNode cur = head;
    	Set<ListNode> set = new HashSet<>();
    	while(cur!=null) {
    		if(set.contains(cur)) {return true;}
    		set.add(cur);
    		cur = cur.next;
    	}
    	return false;
    }
```
时间复杂度：O(n)，对于含有 n 个元素的链表，我们访问每个元素最多一次。添加一个结点到哈希表中只需要花费 O(1) 的时间。

空间复杂度：O(n)，空间取决于添加到哈希表中的元素数目，最多可以添加 n 个元素。




