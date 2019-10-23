# 148. Sort List\(M\)

[148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

## 题目描述\(中等\)

Sort a linked list in O\(n log n\) time using constant space complexity.

Example 1:

```
Input: 4->2->1->3
Output: 1->2->3->4
```

Example 2:

```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
```

## 思路

* 插入排序
* 归并排序
* 自底向上归并排序

### 解决方法

### 插入排序

```java
    public ListNode sortList1(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode cur = head, pre = dummy;
        ListNode next;
        while (cur != null) {
            next = cur.next;
            if (next != null && next.val < cur.val) {
                while (pre.next.val < next.val) {
                    pre = pre.next;
                }
                cur.next = next.next;
                next.next = pre.next;
                pre.next = next;
                pre = dummy;
            } else {
                cur = next;
            }
        }
        return dummy.next;
    }
```

时间复杂度：O\(n^2\)

### 归并排序 递归

![](/assets/101-200/148-s-2-1.png)



链表快慢指针找中点trick，判断fast.next.next
* 链表长度为偶数时，slow指向中点偏左结点
* 链表长度为奇数时，slow指向中点左侧结点

链表快慢指针找中点trick，判断fast.next
* 链表长度为偶数时，slow指向中点偏右结点
* 链表长度为奇数时，slow指向中点结点

链表快慢指针找中点trick，fast先走一步，判断fast.next
* 链表长度为偶数时，slow指向中点偏左结点
* 链表长度为奇数时，slow指向中点左侧结点





```java
    public ListNode sortList2(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode right = sortList2(slow.next);
        slow.next = null;
        ListNode left = sortList2(head);

        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;
        while (left != null && right != null) {
            if (left.val < right.val) {
                cur.next = left;
                left = left.next;
            } else {
                cur.next = right;
                right = right.next;
            }
            cur = cur.next;
        }
        cur.next = left != null ? left : right;
        return dummy.next;
    }
```

### 

```java
    public ListNode sortList3(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        ListNode[] counter = new ListNode[32];
        int maxIndex = 0;
        ListNode cur = head;
        ListNode next;
        while (cur != null) {
            next = cur.next;
            cur.next = null;
            int i = 0;
            ListNode newMergeNode;
            while (counter[i] != null) {
                newMergeNode = mergeTwoLists(cur, counter[i]);
                cur = newMergeNode;
                counter[i] = null;
                i++;
            }
            maxIndex = Math.max(maxIndex, i);
            counter[i] = cur;
            cur = next;
        }
        ListNode newHead = null;
        for (ListNode l : counter) {
            newHead = mergeTwoLists(newHead, l);
        }
        return newHead;
    }

    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) {
            return l2;
        }
        if (l2 == null) {
            return l1;
        }
        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
```



