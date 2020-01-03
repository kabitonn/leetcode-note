
# 538. Convert BST to Greater Tree(E)

[538. 把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

## 题目描述(简单)

给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

例如：
```
输入: 二叉搜索树:
              5
            /   \
           2     13

输出: 转换为累加树:
             18
            /   \
          20     13
```

## 思路

中序遍历的逆序遍历 右-中-左

## 解决方法

### 迭代

```java
    public TreeNode convertBST(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        int sum = 0;
        while (!stack.isEmpty() || p != null) {
            while (p != null) {
                stack.push(p);
                p = p.right;
            }
            p = stack.pop();
            sum += p.val;
            p.val = sum;
            p = p.left;
        }
        return root;
    }
```

### 递归

```java
    int sum = 0;

    public TreeNode convertBST1(TreeNode root) {
        reverseInorder(root);
        return root;
    }

    public void reverseInorder(TreeNode p) {
        if (p == null) {
            return;
        }
        reverseInorder(p.right);
        sum += p.val;
        p.val = sum;
        reverseInorder(p.left);
    }

```