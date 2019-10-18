## [2. Add Two Numbers](https://leetcode-cn.com/problems/add-two-numbers/)

## 1. 题目描述（中等）

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example:

```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

## 2. 思路

两数相加从有效低位开始相加，低位即两链表_l1_和_l2_的头部，两位数字相加出现进位_carry_，

伪代码如下

* 初始化一个节点的头，哑节点start ，但是这个头不存储数字。并且将 curr 指向它。
* 初始化进位 carry 为 0 。
* 初始化 p 和 q 分别为给定的两个链表 l1 和 l2 的头，也就是个位。
* 循环，直到 l1 和 l2 全部到达 null 。
  * 设置 x 为 p 节点的值，如果 p 已经到达了 null，设置 x 为 0 。
  * 设置 y 为 q 节点的值，如果 q 已经到达了 null，设置 y 为 0 。
  * 设置 sum = x + y + carry 。
  * 更新 carry = sum / 10 。
  * 创建一个值为 sum mod 10 的节点，并将 curr 的 next 指向它，同时 curr 指向变为当前的新节点。
  * 向前移动 p 和 q 。
* 判断 carry 是否等于 1 ，如果等于 1 ，在链表末尾增加一个为 1 的节点。
* 返回 start 的 next ，也就是个位数开始的地方。

## 3. 解决方法

### 3.1 carry不作为循环条件

```java
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode start = new ListNode(0);
        ListNode pre = start;
        int carry = 0;
        int x,y,sum;
        while(l1!=null||l2!=null) {
            x=l1!=null?l1.val:0;
            y=l2!=null?l2.val:0;
            sum = x+y+carry;
            carry = sum / 10;
            pre.next = new ListNode(sum%10);
            if(l1!=null) {
                l1 = l1.next;
            }
            if(l2!=null) {
                l2 = l2.next;
            }
            pre = pre.next;
        }
        if(carry!=0) {
            pre.next = new ListNode(carry);
        }
        return start.next;
    }
```

### 3.2 carry也作为循环条件

```java
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode start = new ListNode(0);
        ListNode pre = start;
        int carry = 0;
        while(l1!=null||l2!=null||carry != 0) {
            ListNode last = new ListNode(carry);
            if(l1!=null) {
                last.val += l1.val;
                l1 = l1.next;
            }
            if(l2!=null) {
                last.val += l2.val;
                l2 = l2.next;
            }
            carry = last.val/10;
            last.val%=10;
            pre.next = last;
            pre = last;
        }
        return start.next;
    }
```

## 4. 拓展

如果链表中的数字不是按逆序存储的呢？例如：

> \(3→4→2\)+\(4→6→5\)=8→0→7

### 4.1 思路1

首先想到的是链表先逆序计算，然后将结果再逆序呗，这就转换到我们之前的情况了。

