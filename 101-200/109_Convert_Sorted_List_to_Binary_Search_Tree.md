# 109. Convert Sorted List to Binary Search Tree(M)


[109. 有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)


## 题目描述(中等)

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example:
```
Given the sorted linked list: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:

      0
     / \
   -3   9
   /   /
 -10  5
```

## 思路

- 递归



## 解决方法



### 递归 快慢指针

快慢指针中点

```
    public TreeNode sortedListToBST(ListNode head) {

        return sortedListToBST(head, null);
    }

    public TreeNode sortedListToBST(ListNode head, ListNode end) {
        if (head == end) {
            return null;
        }
        ListNode slow = head, fast = head;
        while (fast != end && fast.next != end) {
            slow = slow.next;
            fast = fast.next.next;
        }
        TreeNode root = new TreeNode(slow.val);
        root.left = sortedListToBST(head, slow);
        root.right = sortedListToBST(slow.next, end);
        return root;
    }

```
时间复杂度：根据递归式可知，T(n) = 2 * T（n / 2 ) + n，O(nlog(n))。

空间复杂度：O(log(n))。

### 转换数组

```java
    public TreeNode sortedListToBST0(ListNode head) {
        List<Integer> nums = new ArrayList<>();
        ListNode p = head;
        while (p != null) {
            nums.add(p.val);
            p = p.next;
        }
        return sortedArrayToBST(nums, 0, nums.size() - 1);
    }

    private TreeNode sortedArrayToBST(List<Integer> nums, int start, int end) {
        if (start > end) {
            return null;
        }
        //int mid = start + (end - start) / 2;
        int mid = (start + end) >>> 1;
        TreeNode root = new TreeNode(nums.get(mid));
        root.left = sortedArrayToBST(nums, start, mid - 1);
        root.right = sortedArrayToBST(nums, mid + 1, end);
        return root;
    }
```
时间复杂度：O(log(n))。

空间复杂度：数组进行辅助，O(n)。

### 建立BST 中序遍历赋值

```java
    private ListNode curNode = null;

    public TreeNode sortedListToBST1(ListNode head) {
        curNode = head;
        return sortedListToBST(0, size(head));
    }

    private TreeNode sortedListToBST(int start, int end) {
        if (start >= end) {
            return null;
        }
        int mid = (start + end) >>> 1;
        TreeNode left = sortedListToBST(start, mid);
        TreeNode root = new TreeNode(curNode.val);
        root.left = left;
        curNode = curNode.next;
        root.right = sortedListToBST(mid + 1, end);
        return root;
    }

    private int size(ListNode head) {
        int size = 0;
        while (head != null) {
            size++;
            head = head.next;
        }
        return size;
    }

```

时间复杂度：O(n)，主要是得到开始的 size，需要遍历一遍链表。

空间复杂度：O(log(n))，递归压栈的消耗。
