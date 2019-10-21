# 144. Binary Tree Preorder Traversal(M)


[](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)


## 题目描述(中等)

Given a binary tree, return the preorder traversal of its nodes' values.

Example:
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]
Follow up: Recursive solution is trivial, could you do it iteratively?
```

## 思路

- 递归
- 迭代
- Morris Traversal

## 解决方法



### 递归

```java
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        preorder(list, root);
        return list;
    }

    private void preorder(List<Integer> list, TreeNode p) {
        if (p == null) {
            return;
        }
        list.add(p.val);
        preorder(list, p.left);
        preorder(list, p.right);
    }

```

### 迭代1

模拟递归压栈出栈

```
    public List<Integer> preorderTraversal1(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode p = stack.pop();
            list.add(p.val);
            if (p.right != null) {
                stack.push(p.right);
            }
            if (p.left != null) {
                stack.push(p.left);
            }
        }
        return list;
    }
```

### 迭代2






