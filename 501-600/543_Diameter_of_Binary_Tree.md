
# 543. Diameter of Binary Tree(E)

[543. 二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

## 题目描述(简单)

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

示例 :
```
给定二叉树

          1
         / \
        2   3
       / \     
      4   5    
返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。
```

**注意**：两结点之间的路径长度是以它们之间边的数目表示。


## 思路

- 如果经过根节点,那么直径等于左子树的深度+右子树的深度;
- 如果不经过根节点,那么直径等于左子树的直径,和右子树的直径的最大值;
- 两种情况的最大值就是树的直径.

## 解决方法

### 两次递归

```java
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = getDepth(root.left);
        int right = getDepth(root.right);
        int leftDiameter = diameterOfBinaryTree(root.left);
        int rightDiameter = diameterOfBinaryTree(root.right);
        return Math.max((left + right), Math.max(leftDiameter, rightDiameter));
    }

    public int getDepth(TreeNode p) {
        if (p == null) {
            return 0;
        }
        int left = getDepth(p.left);
        int right = getDepth(p.right);
        return Math.max(left, right) + 1;
    }
```

### 一次递归

递归过程中同时记录当前子树的深度和直径root{直径，深度}



```java
    public int diameterOfBinaryTree1(TreeNode root) {
        return getDiameter(root)[0];
    }

    public int[] getDiameter(TreeNode p) {
        if (p == null) {
            return new int[]{0, 0};
        }
        int[] left = getDiameter(p.left);
        int[] right = getDiameter(p.right);
        int[] root = new int[2];
        root[0] = Math.max(left[1] + right[1], Math.max(left[0], right[0]));
        root[1] = Math.max(left[1], right[1]) + 1;
        return root;
    }
```

亦可以利用全局变量记录最大直径，仅需一次递归

```java
    int max = 0;

    public int diameterOfBinaryTree2(TreeNode root) {
        depth(root);
        return max;
    }

    public int depth(TreeNode p) {
        if (p == null) {
            return 0;
        }
        int left = depth(p.left);
        int right = depth(p.right);
        max = Math.max(max, left + right + 1);
        return Math.max(left, right) + 1;
    }
```