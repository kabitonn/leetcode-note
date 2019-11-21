# 086. Partition List(M)
[086. 分隔链表](https://leetcode-cn.com/problems/partition-list/)


## 题目描述(中等)

Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

Example:
```
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5
```
## 思路

## 解决方法


### 双指针

用一个指针tail记录当前小于分区点的链表的末尾，用另一个指针遍历链表，每次遇到小于分区点的数，就把它移除并插入到记录的链表末尾，并且更新末尾指针。

```java
    public ListNode partition(ListNode head, int x) {
        ListNode start = new ListNode(0);
        start.next = head;
        ListNode tail = null;
        head = start;
        //找到第一个大于等于分区点的节点，tail 指向它的前边
        while (head.next != null && head.next.val<x) {
            head=head.next;
        }
        tail = head;
        while (head.next != null) {
            //如果当前节点小于分区点，就把它插入到 tail 的后边
            if (head.next.val < x) {
                //拿出要插入的节点
                ListNode insert = head.next;
                //将要插入的结点移除
                head.next = insert.next;
                //将 move 插入到 tail 后边
                insert.next = tail.next;
                tail.next = insert;
                //更新 tail
                tail = insert;
            }else{
                head = head.next;
            }

        }
        return start.next;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。

### 三指针

用一个指针left指向记录当前小于分区点的链表的末尾，另外两个指针right1和right2指向记录当前大于等于分区点的首尾，遍历链表，遇到小于分区点的接在left后，否则接在right2后，最后连接两条链表

```java
    public ListNode partition0(ListNode head, int x) {
        ListNode start = new ListNode(0);
        start.next = head;
        ListNode left = start, right1 = null, right2 = null, cur = head;
        while (cur != null) {
            if (cur.val < x) {
                left.next = cur;
                left = cur;
            } else if (right1 != null) {
                right2.next = cur;
                right2 = cur;
            } else if (right1 == null) {
                right1 = cur;
                right2 = cur;
            }
            cur = cur.next;
        }
        left.next = right1;
        if (right2 != null) {
            right2.next = null;
        }
        return start.next;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。

### 四指针

minHead, min, maxHead, max 指向记录当前小于分区点的链表的首尾和记录当前大于等于分区点的首尾


```java
    public ListNode partition1(ListNode head, int x) {
        //小于分区点的链表
        ListNode minHead = new ListNode(0);
        ListNode min = minHead;
        //大于等于分区点的链表
        ListNode maxHead = new ListNode(0);
        ListNode max = maxHead;
        //遍历整个链表
        while (head != null) {
            if (head.val < x) {
                min.next = head;
                min = min.next;
            } else {
                max.next = head;
                max = max.next;
            }
            head = head.next;
        }
        //这步不要忘记，不然链表就出现环了
        max.next = null;
        //两个链表接起来
        min.next = maxHead.next;
        return minHead.next;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。
