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

访问根节点，左子树持续压栈，直到没有左子树出栈，遍历右子树

```java
    public List<Integer> preorderTraversal2(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                list.add(p.val);
                stack.push(p);
                p = p.left;
            }
            p = stack.pop();
            p = p.right;
        }
        return list;
    }

    public List<Integer> preorderTraversal3(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        while (p != null) {
            list.add(p.val);
            stack.push(p);
            p = p.left;
        }
        while (!stack.isEmpty()) {
            p = stack.pop().right;
            while (p != null) {
                list.add(p.val);
                stack.push(p);
                p = p.left;
            }
        }
        return list;
    }
```

### Morris Traversal

主要思想就是利用叶子节点的左右子树是 null ，所以我们可以利用这个空间去存我们需要的节点

```java
    public List<Integer> preorderTraversal4(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        TreeNode cur = root;
        while (cur != null) {
            //情况 1
            if (cur.left == null) {
                list.add(cur.val);
                cur = cur.right;
            } else {
                //找左子树最右边的节点
                TreeNode pre = cur.left;
                while (pre.right != null && pre.right != cur) {
                    pre = pre.right;
                }
                //情况 2.1
                if (pre.right == null) {
                    list.add(cur.val);
                    pre.right = cur;
                    cur = cur.left;
                }
                //情况 2.2
                if (pre.right == cur) {
                    pre.right = null; //这里可以恢复为 null
                    cur = cur.right;
                }
            }
        }
        return list;
    }
```



