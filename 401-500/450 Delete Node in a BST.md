
# 450. Delete Node in a BST(M)

[450. 删除二叉搜索树中的节点](https://leetcode-cn.com/problems/delete-node-in-a-bst/)

## 题目描述(中等)

给定一个二叉搜索树的根节点 `root` 和一个值 `key`，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：
1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

**说明**： 要求算法时间复杂度为 O(h)，h 为树的高度。

示例:
```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。

一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。

    5
   / \
  4   6
 /     \
2       7

另一个正确答案是 [5,2,6,null,4,null,7]。

    5
   / \
  2   6
   \   \
    4   7
```

## 思路

- 搜索节点 删除 更换节点
- 搜索节点 插入子树

## 解决方法

### 搜索节点 + 更换节点

先搜索节点，再删除，对于搜索到的节点分类
- 非根节点
- 根节点
  - 子树均空    ：返回空即可删除
  - 左子树为空  ：返回右子树即可删除
  - 右子树为空  ：返回左子树即可删除
  - 左右子树非空：将左子树的最右节点或右子树的最左节点提上来，
    - 左子树的最右节点作为根节点(最右节点无右子树)
      - 若最右节点的父节点是根节点：即根节点的左子树即最右节点，将右节点的左子树连上根节点的左子树
      - 若最右节点的父节点非根节点：将右节点的左子树连上父节点的右子树
      - 返回右节点作为根节点
    - 右子树的最左节点作为根节点(最左节点无左子树)
      - 若最左节点的父节点是根节点：即根节点的右子树即最左节点，将左节点的右子树连上根节点的右子树
      - 若最左节点的父节点非根节点：将左节点的右子树连上父节点的左子树
      - 返回左节点作为根节点

两种方式将节点作为根节点返回
- 将节点值赋值给根节点，返回根节点
- 将根节点的子树连接到节点上，返回节点

```java
    public TreeNode deleteNode(TreeNode root, int key) {
        if (root == null) {
            return null;
        }
        if (root.val > key) {
            root.left = deleteNode(root.left, key);
            return root;
        } else if (root.val < key) {
            root.right = deleteNode(root.right, key);
            return root;
        } else {
            if (root.left == null) {
                return root.right;
            }
            if (root.right == null) {
                return root.left;
            }
            TreeNode prev = null, right = root.left;
            while (right.right != null) {
                prev = right;
                right = right.right;
            }
            if (prev == null) {
                root.left = right.left;
            } else {
                prev.right = right.left;
            }
            root.val = right.val;
            return root;
            /*
            right.left = root.left;
            right.right = root.right;
            return right;
             */
        }
    }

```

### 搜索节点 插入

先搜索节点，再删除，对于搜索到的节点分类
- 非根节点
- 根节点
  - 子树均空    ：返回空即可删除
  - 左子树为空  ：返回右子树即可删除
  - 右子树为空  ：返回左子树即可删除
  - 左右子树非空：返回左子树或右子树
    - 返回左子树：找到左子树的最右节点r，最右节点无右子树，将根节点的右子树作为r的右子树即可删除根节点
    - 返回右子树：找到右子树的最左节点l，最左节点无左子树，将根节点的左子树作为l的左子树即可删除根节点

```java
    public TreeNode deleteNode1(TreeNode root, int key) {
        if (root == null) {
            return null;
        }
        if (root.val > key) {
            root.left = deleteNode1(root.left, key);
            return root;
        } else if (root.val < key) {
            root.right = deleteNode1(root.right, key);
            return root;
        } else {
            if (root.left == null) {
                return root.right;
            }
            if (root.right == null) {
                return root.left;
            }
            TreeNode right = root.left;
            while (right.right != null) {
                right = right.right;
            }
            right.right = root.right;
            return root.left;
        }
    }
```