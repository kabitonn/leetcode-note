# 145. Binary Tree Postorder Traversal(H)


[145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)


## 题目描述(困难)

Given a binary tree, return the postorder traversal of its nodes' values.

Example:
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [3,2,1]
Follow up: Recursive solution is trivial, could you do it iteratively?

```

## 思路

- 递归
- 转换为根右左的逆序
- Morris Traversal


## 解决方法



### 递归

```java
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        postorder(list, root);
        return list;
    }

    public void postorder(List<Integer> list, TreeNode p) {
        if (p == null) {
            return;
        }
        postorder(list, p.left);
        postorder(list, p.right);
        list.add(p.val);
    }

```

### 转换为根右左的逆序


模拟递归压栈出栈
```java
    //根右左的逆序
    public List<Integer> postorderTraversal2(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if (root == null) {
            return list;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode p = stack.pop();
            list.add(p.val);
            if (p.left != null) {
                stack.push(p.left);
            }
            if (p.right != null) {
                stack.push(p.right);
            }
        }
        Collections.reverse(list);
        return list;
    }

```

访问根节点，右子树持续压栈，直到没有右子树出栈，访问左子树，最后访问List逆序

```java
    //根右左的逆序
    public List<Integer> postorderTraversal3(TreeNode root) {
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
                p = p.right;
            }
            p = stack.pop();
            p = p.left;
        }
        Collections.reverse(list);
        return list;
    }
```

Morris Traversal 的逆序

```java
    public List<Integer> postorderTraversal4(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        TreeNode cur = root;
        while (cur != null) {
            if (cur.right == null) {
                list.add(cur.val);
                cur = cur.left;
            } else {
                TreeNode pre = cur.right;
                while (pre.left != null && pre.left != cur) {
                    pre = pre.left;
                }
                if (pre.left == null) {
                    list.add(cur.val);
                    pre.left = cur;
                    cur = cur.right;
                }
                if (pre.left == cur) {
                    pre.left = null;
                    cur = cur.left;
                }
            }
        }
        Collections.reverse(list);
        return list;
    }
```


### 迭代


### Morris Traversal


```java
```





