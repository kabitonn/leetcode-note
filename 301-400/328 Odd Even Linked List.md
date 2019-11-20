# 328. Odd Even Linked List(M)

[328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list/)

## 题目描述(中等)

给定一个单链表，把所有的奇数节点和偶数节点分别排在一起。请注意，这里的奇数节点和偶数节点指的是节点编号的奇偶性，而不是节点的值的奇偶性。

请尝试使用原地算法完成。你的算法的空间复杂度应为 O(1)，时间复杂度应为 O(nodes)，nodes 为节点总数。

示例 1:
```
输入: 1->2->3->4->5->NULL
输出: 1->3->5->2->4->NULL
```
示例 2:
```
输入: 2->1->3->5->6->4->7->NULL 
输出: 2->3->6->7->1->5->4->NULL
```
**说明**:
- 应当保持奇数节点和偶数节点的相对顺序。
- 链表的第一个节点视为奇数节点，第二个节点视为偶数节点，以此类推。


## 思路

- 链表插入
- 链表连接

## 解决方法

### 一条链表依次连接

两指针指向奇数链表的尾节点和偶数链表的尾结点，奇数节点链表尾结点的next就是偶数节点链表

每次取出链表上两个节点，连接奇数节点时先保存偶数节点链表头部(奇数节点尾部的next)，将奇数节点插入其中，偶数节点接入偶数节点链表尾部

```java
    public ListNode oddEvenList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode odd = head;
        ListNode even = head.next;
        ListNode cur = head.next.next;
        while (cur != null) {
            ListNode next = cur.next != null ? cur.next.next : null;
            ListNode tmpEven = odd.next;
            odd.next = cur;
            even.next = cur.next;
            odd = odd.next;
            odd.next = tmpEven;
            even = even.next;
            cur = next;
        }

        return head;
    }
```

### 两条链表连接

存储奇数节点链表和偶数节点链表的头尾，将奇节点放在一个链表里，偶链表放在另一个链表里。然后把偶链表接在奇链表的尾部

```java
    public ListNode oddEvenList1(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode oddHead = head, odd = oddHead;
        ListNode evenHead = head.next, even = evenHead;
        while (even != null && even.next!=null) {
            odd.next = even.next;
            odd = odd.next;
            even.next = odd.next;
            even = even.next;
        }
        odd.next = evenHead;
        return oddHead;
    }
```