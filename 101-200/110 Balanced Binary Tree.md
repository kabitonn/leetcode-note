# 110. Balanced Binary Tree
[110. 平衡二叉树](https://leetcode-cn.com/problems/balanced-binary-tree/)

## 题目描述(简单)

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example 1:
```
Given the following tree [3,9,20,null,null,15,7]:

    3
   / \
  9  20
    /  \
   15   7
Return true.
```
Example 2:
```
Given the following tree [1,2,2,3,3,null,null,4,4]:

       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
Return false.
```



## 思路

- 递归

## 解决方法

### 自顶向下递归

```java
    public boolean isBalanced(TreeNode root) {
        if (root == null) {
            return true;
        }
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);
        return Math.abs(leftDepth - rightDepth) <= 1 && isBalanced(root.left) && isBalanced(root.right);
    }

    private int maxDepth(TreeNode p) {
        if (p == null) {
            return 0;
        }
        return 1 + Math.max(maxDepth(p.left), maxDepth(p.right));
    }
```



### 自底向上递归

只需要求一次高度，并且在求左子树和右子树的高度的同时，判断一下当前是否是平衡二叉树。

depth 函数返回的是int值，同时高度不可能为负数，那么如果求高度过程中我们发现了当前不是平衡二叉树，就返回-1。

```java
    //自底向上
    public boolean isBalanced1(TreeNode root) {
        return depth(root) != -1;
    }

    private int depth(TreeNode p) {
        if (p == null) {
            return 0;
        }
        int left = depth(p.left);
        if (left == -1) {
            return -1;
        }
        int right = depth(p.right);
        if (right == -1) {
            return -1;
        }
        if (Math.abs(left - right) <= 1) {
            return Math.max(left, right) + 1;
        } else {
            return -1;
        }
    }
```

