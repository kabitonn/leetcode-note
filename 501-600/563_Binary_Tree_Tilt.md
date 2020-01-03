
# 563. Binary Tree Tilt(E)
 
[563. 二叉树的坡度](https://leetcode-cn.com/problems/binary-tree-tilt/)

## 题目描述(简单)

给定一个二叉树，计算整个树的坡度。

一个树的节点的坡度定义即为，该节点左子树的结点之和和右子树结点之和的差的绝对值。空结点的的坡度是0。

整个树的坡度就是其所有节点的坡度之和。

示例:
```
输入: 
         1
       /   \
      2     3
输出: 1
解释: 
结点的坡度 2 : 0
结点的坡度 3 : 0
结点的坡度 1 : |2-3| = 1
树的坡度 : 0 + 0 + 1 = 1
```

**注意**:
- 任何子树的结点的和不会超过32位整数的范围。
- 坡度的值不会超过32位整数的范围。


## 思路

## 解决方法

### 一次递归

root{当前子树的坡度，当前子树节点和}

```java
    public int findTilt(TreeNode root) {
        return getDiffSumTree(root)[0];
    }

    public int[] getDiffSumTree(TreeNode p) {
        if (p == null) {
            return new int[]{0, 0};
        }
        int[] left = getDiffSumTree(p.left);
        int[] right = getDiffSumTree(p.right);
        int[] root = new int[2];
        root[0] = Math.abs(left[1] - right[1]) + left[0] + right[0];
        root[1] = left[1] + right[1] + p.val;
        return root;
    }
```

### 一次递归

递归过程中累计节点的坡度和

```java
    int sum = 0;

    public int findTilt2(TreeNode root) {
        sumTree(root);
        return sum;
    }

    public int sumTree(TreeNode p) {
        if (p == null) {
            return 0;
        }
        int left = sumTree(p.left);
        int right = sumTree(p.right);
        sum += Math.abs(left - right);
        return p.val + left + right;
    }
```

### 两次递归

第一次递归将结点修改为结点的坡度，第二次递归求数的坡度即结点和

```java
    public int findTilt1(TreeNode root) {
        travleSum(root);
        return getSum(root);
    }

    public int getSum(TreeNode p) {
        if (p == null) {
            return 0;
        }
        return p.val + getSum(p.left) + getSum(p.right);
    }

    public int travleSum(TreeNode p) {
        if (p == null) {
            return 0;
        }
        int val = p.val;
        int left = travleSum(p.left);
        int right = travleSum(p.right);
        p.val = Math.abs(left - right);
        return val + left + right;
    }

```