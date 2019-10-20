# 023. Merge k Sorted Lists(H)
[023. Merge k Sorted Lists](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

## 题目描述\(困难\)

Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

Example:

```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```

## 思路

1. 利用二路归并，水平归并，依此和后者链表归并
2. 两两归并
3. 垂直归并，一列一列比较
4. 优先队列
5. 存储所有节点值，排序后生成

## 解决方法

### 水平归并

```java
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode cur = null;
        for(ListNode l:lists) {
            cur = mergeTwoLists(cur, l);
        }
        return cur;
    }
```

```java
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode start = new ListNode(0);
        ListNode ln1 = l1,ln2 = l2;
        ListNode cur = start;
        while(ln1!=null&&ln2!=null) {
            if(ln1.val<ln2.val) {
                cur.next = new ListNode(ln1.val);
                ln1 = ln1.next;
            }
            else {
                cur.next = new ListNode(ln2.val);
                ln2 = ln2.next;
            }
            cur = cur.next;
        }
        while(ln1!=null) {
            cur.next = new ListNode(ln1.val);
            ln1 = ln1.next;
            cur = cur.next;
        }
        while(ln2!=null) {
            cur.next = new ListNode(ln2.val);
            ln2 = ln2.next;
            cur = cur.next;
        }
        return start.next;
    }
```

### 两两归并

```java
    public ListNode mergeKLists(ListNode[] lists) {
        int cap = lists.length;
        if(cap == 0)
            return null;
        ListNode[] res = lists;
        while(cap!=1) {
            for(int i=0;i<cap/2;i++) {
                res[i]=mergeTwoLists(res[i], res[cap-1-i]);
            }
            cap=cap%2==0?cap/2:cap/2+1;
        }
        return res[0];
    }
```

### 垂直归并

```java
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode start = new ListNode(0);
        ListNode cur = start;
        while(true) {
            boolean isALL = true;
            int min = Integer.MAX_VALUE;
            int minIndex = -1;
            for(int i=0;i<lists.length;i++) {
                if(lists[i]!=null) {
                    if(lists[i].val<min) {
                        min = lists[i].val;
                        minIndex = i;
                    }
                    isALL = false;
                }
            }
            if(isALL)
                break;
            cur.next = lists[minIndex];
            cur = cur.next;
            lists[minIndex] = lists[minIndex].next; 
        }
        return start.next;
    }
```

### 优先队列

```java
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode start = new ListNode(0);
        ListNode cur = start;
        Queue<ListNode> queue = new PriorityQueue<>(new Comparator<ListNode>() {
            @Override
            public int compare(ListNode o1, ListNode o2) {
                return o1.val-o2.val;
            }
        });
        for(ListNode l:lists) {
            if(l!=null) {
                queue.add(l);
            }
        }
        while(!queue.isEmpty()) {
            ListNode pNode = queue.poll();
            cur.next = new ListNode(pNode.val);
            cur = cur.next;
            if(pNode.next!=null) {
                queue.add(pNode.next);
            }
        }
        return start.next;
    }
```

### 暴力法-数组排序

```java
    public ListNode mergeKLists(ListNode[] lists) {
        ListNode start = new ListNode(0);
        List<Integer> vals = new ArrayList<>();
        ListNode cur = start;
        for(ListNode l:lists) {
            while(l!=null) {
                vals.add(l.val);
                l=l.next;
            }
        }
        Collections.sort(vals);
        for(int val:vals) {
            cur.next = new ListNode(val);
            cur = cur.next;
        }
        return start.next;
    }
```
时间复杂度：假设 N 是所有的数字个数，存到数组是 O(N)，排序如果是用快速排序就是 $$ O(Nlog_N) $$，存到链表是 O(N)，所以取个最大的，就是 $$ O(Nlog_N) $$
空间复杂度：新建了一个链表，O(N)。


