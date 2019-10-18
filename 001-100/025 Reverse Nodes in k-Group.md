# 025. Reverse Nodes in k-Group
[link](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

## 题目描述\(困难\)

## 思路

1. 递归：k个一组的链表断开，保存一组后的新的头结点，逆置当前k个节点连接顺序，并递归调用剩余部分，将剩余部分新头部连接至逆置后的链表
2. 迭代：k个一组的链表断开，保存一组后的新的头结点，逆置当前k个节点连接顺序，保存当前k个节点的尾结点作为连接点，连接下一次逆置后的新头部

## 解决方法

### 递归

```java
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode cur = head;
        int n = 1;
        while(cur!=null&&n++<k) {
            cur = cur.next;
        }
        if(cur==null)
            return head;
        cur = head;
        ListNode[] listNodes = new ListNode[k];
        for(int i=0;i<k;i++) {
            listNodes[i] = cur;
            cur = cur.next;
        }
        head.next = reverseKGroup(listNodes[k-1].next, k);
        for(int i=k-1;i>0;i--) {
            listNodes[i].next = listNodes[i-1];
        }
        return listNodes[k-1];
    }
```

```java
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode cur = head;
        int n = 1;
        while(cur!=null&&n++<k) {
            cur = cur.next;
        }
        if(cur==null)
            return head;
        ListNode tmp = cur.next;
        cur.next = null;
        ListNode newHead = reverse(head);
        head.next = reverseKGroup(tmp, k);
        return newHead;
    }
    public ListNode reverse(ListNode head) {
        ListNode cur = head;
        ListNode newHead = null;//newHead 即为prev
        ListNode next;
        while(cur!=null) {
            next = cur.next;
            cur.next = newHead;
            newHead = cur;
            cur = next;
        }
        return newHead;
    }
```

### 迭代

```java
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode start = new ListNode(0);
        start.next = head;
        ListNode cur = start.next;
        ListNode connect = start;
        ListNode[] listNodes = new ListNode[k];
        while(true) {
            //当轮头结点
            ListNode tmp = cur;
            int n = 1;
            while(cur!=null&&n++<k) {
                cur = cur.next;
            }
            if(cur==null)
                break;
            cur = tmp;
            for(int i=0;i<k;i++) {
                listNodes[i] = cur;
                cur = cur.next;
            }

            for(int i=k-1;i>0;i--) {
                listNodes[i].next = listNodes[i-1];
            }
            //接续点
            connect.next = listNodes[k-1];
            connect = listNodes[0];
            connect.next = cur;
        }
        return start.next;
    }
```

```java
    public ListNode reverseKGroup(ListNode head, int k) {
        ListNode start = new ListNode(0);
        start.next = head;
        ListNode cur = start.next;
        ListNode connect = start;
        while(true) {
            //当轮头结点
            ListNode tmp = cur;
            int n = 1;
            while(cur!=null&&n++<k) {
                cur = cur.next;
            }
            if(cur==null)
                break;
            //下一轮首结点next cur
            ListNode next = cur.next;
            cur.next = null;
            //当轮反转
            ListNode newHead = reverse(tmp);
            //接续点
            connect.next = newHead;
            connect = tmp;
            connect.next = next;
            cur = next;
        }
        return start.next;
    }
    public ListNode reverse(ListNode head) {
        ListNode cur = head;
        ListNode newHead = null;
        ListNode next;
        while(cur!=null) {
            next = cur.next;
            cur.next = newHead;
            newHead = cur;
            cur = next;
        }
        return newHead;
    }
```



