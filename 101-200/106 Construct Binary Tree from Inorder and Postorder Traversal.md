# 106. Construct Binary Tree from Inorder and Postorder Traversal(M)


[106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)


## 题目描述(中等)

Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

For example, given
```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
Return the following binary tree:

    3
   / \
  9  20
    /  \
   15   7

```

## 思路

- 递归
- 迭代

## 解决方法



### 递归

中序遍历的顺序是 左子树 -> 根节点 -> 右子树。
后序遍历的顺序是 左子树 -> 右子树 -> 根节点。





