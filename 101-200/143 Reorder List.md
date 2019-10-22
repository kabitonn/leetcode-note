# 143. Reorder List\(M\)

## 题目描述\(中等\)

Given a singly linked list L: $$L_0→L_1→…→L_{n-1}→L_n$$,  
reorder it to: $$ L_0→L_n→L_1→L_{n-1}→L_2→L_{n-2}→… $$

You may not modify the values in the list's nodes, only nodes itself may be changed.

Example 1:

```
Given 1->2->3->4, reorder it to 1->4->2->3.
```

Example 2:

```
Given 1->2->3->4->5, reorder it to 1->5->2->4->3.
```

## 思路

* 存储
* 递归

## 解决方法

### 数组存储

```java
    public void reorderList(ListNode head) {
        if (head == null) {
            return;
        }
        List<ListNode> nodes = new ArrayList<>();
        while (head != null) {
            nodes.add(head);
            head = head.next;
        }
        int i = 0, j = nodes.size() - 1;
        for (; i < j; i++, j--) {
            ListNode n1 = nodes.get(i);
            ListNode n2 = nodes.get(j);
            n2.next = n1.next;
            n1.next = n2;
        }
        nodes.get(i).next = null;
    }
```

### 递归

![](/assets/101-200/143-s-2-1.png)



![](/assets/101-200/143-s-2-2.png)

```java
    public void reorderList1(ListNode head) {
        if (head == null) {
            return;
        }
        int len = 0;
        ListNode node = head;
        while (node != null) {
            len++;
            node = node.next;
        }
        reorderList(head, len);
    }

    public ListNode reorderList(ListNode head, int len) {
        if (len == 1) {
            ListNode next = head.next;
            head.next = null;
            return next;
        }
        if (len == 2) {
            ListNode next = head.next.next;
            head.next.next = null;
            return next;
        }
        ListNode tail = reorderList(head.next, len - 2);
        ListNode next = tail.next;
        tail.next = head.next;
        head.next = tail;
        return next;
    }
```



