
# 530. Minimum Absolute Difference in BST(E)

[530. 二叉搜索树的最小绝对差](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

## 题目描述(简单)

给定一个所有节点为非负值的二叉搜索树，求树中任意两节点的差的绝对值的最小值。

示例 :
```
输入:

   1
    \
     3
    /
   2

输出:
1

解释:
最小绝对差为1，其中 2 和 1 的差的绝对值为 1（或者 2 和 3）。
```

**注意**: 树中至少有2个节点。

## 思路

中序遍历序列，求相邻两数差的绝对值最小值

- 递归
- 迭代

## 解决方法

### 中序遍历后 求最小值

```java
    public int getMinimumDifference(TreeNode root) {
        List<Integer> list = new LinkedList<>();
        inorder(root, list);
        if (list.size() < 2) {
            return 0;
        }
        int prev = -1;
        int min = Integer.MAX_VALUE;
        for (int num : list) {
            if (prev >= 0) {
                min = Math.min(min, (num - prev));
            }
            prev = num;
        }
        return min;
    }

    public void inorder(TreeNode p, List<Integer> list) {
        if (p == null) {
            return;
        }
        inorder(p.left, list);
        list.add(p.val);
        inorder(p.right, list);
    }

```

### 中序遍历中求最小值 迭代

```java
    public int getMinimumDifference1(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        int prev = -1;
        int min = Integer.MAX_VALUE;
        while (!stack.isEmpty() || p != null) {
            while (p != null) {
                stack.push(p);
                p = p.left;
            }
            p = stack.pop();
            if (prev >= 0) {
                min = Math.min(min, (p.val - prev));
            }
            prev = p.val;
            p = p.right;
        }
        return min;
    }
```

### 中序遍历中求最小值 递归

```java
    int prev = -1;
    int min = Integer.MAX_VALUE;

    public int getMinimumDifference2(TreeNode root) {
        inorder(root);
        return min;
    }


    public void inorder(TreeNode p) {
        if (p == null) {
            return;
        }
        inorder(p.left);
        if (prev >= 0) {
            min = Math.min(min, (p.val - prev));
        }
        prev = p.val;
        inorder(p.right);
    }
```