# 098. Validate Binary Search Tree(M)
[098. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

## 题目描述(中等)
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
 

Example 1:
```
    2
   / \
  1   3

Input: [2,1,3]
Output: true
```
Example 2:
```
    5
   / \
  1   4
     / \
    3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

## 思路

- 递归
- 

## 解决方法


### 递归

判断左子树的最右节点小于根节点
判断右子树的最左节点大于根节点
判断左右子树也是二叉搜索树

```java
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        if (root.left != null) {
            TreeNode right = root.left;
            while (right.right != null) {
                right = right.right;
            }
            if (right.val >= root.val) {
                return false;
            }
        }
        if (root.right != null) {
            TreeNode left = root.right;
            while (left.left != null) {
                left = left.left;
            }
            if (left.val <= root.val) {
                return false;
            }
        }
        return isValidBST(root.left) && isValidBST(root.right);
    }
```

### 迭代


### 