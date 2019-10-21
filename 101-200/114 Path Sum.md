# 114. Flatten Binary Tree to Linked List(M)


[114. 二叉树展开为链表](https://leetcode-cn.com/problems/flatten-binary-tree-to-linked-list/)


## 题目描述(中等)

Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:
```
    1
   / \
  2   5
 / \   \
3   4   6
The flattened tree should look like:

1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```


## 思路

- 左子树最右子树
- 后序遍历的逆序
- 前序遍历


## 解决方法


### 找节点接右侧

可以发现展开的顺序其实就是二叉树的先序遍历。

1. 将左子树插入到右子树的地方
- 将原来的右子树接到左子树的最右边节点
- 考虑新的右子树的根节点，一直重复上边的过程，直到新的右子树为 null


```java
 public void flatten1(TreeNode root) {
        TreeNode p = root;
        while (p != null) {
            if (p.left != null) {
                TreeNode left = p.left;
                while (left.right != null) {
                    left = left.right;
                }
                TreeNode right = p.right;
                left.right = right;
                p.right = p.left;
                p.left = null;
            }
            p = p.right;
        }
    }
```

### 递归 变形后序

右左根的遍历顺序

每遍历一个节点就将当前节点的右指针更新为上一个节点。

```java
    private TreeNode pre = null;

    //变形后序遍历 右左根
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        flatten(root.right);
        flatten(root.left);
        root.left = null;
        root.right = pre;
        pre = root;
    }
```

### 迭代 变形后序

```java
    //变形后序遍历 右左根
    public void flatten2(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        TreeNode preVisited = null;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                stack.push(p);// 添加根节点
                p = p.right;
            }
            p = stack.peek();// 已经访问到最右的节点了
            if (p.left == null || p.left == preVisited) {
                stack.pop();
                p.right = preVisited;
                p.left = null;
                preVisited = p;
                p = null;
            } else {
                p = p.left;
            }
        }
    }
```

### 迭代 前序遍历


```java
    public void flatten3(TreeNode root) {
        if (root == null) {
            return;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        TreeNode pre = null;
        while (!stack.isEmpty()) {
            TreeNode p = stack.pop();
            if (p.right != null) {
                stack.push(p.right);
            }
            if (p.left != null) {
                stack.push(p.left);
                p.left=null;
            }
            if (pre != null) {
                pre.right = p;
            }
            pre = p;
        }
    }
```





