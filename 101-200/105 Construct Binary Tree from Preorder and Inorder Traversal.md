# 105. Construct Binary Tree from Preorder and Inorder Traversal (M)


[](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)


## 题目描述(中等)

Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

For example, given
```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
Return the following binary tree:

    3
   / \
  9  20
    /  \
   15   7
```


## 思路

- 递归
- 


## 解决方法



### 递归

```java
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = inorder.length;
        return buildTree(preorder, 0, n - 1, inorder, 0, n - 1);
    }

    private TreeNode buildTree(int[] preorder, int preL, int preR, int[] inorder, int inL, int inR) {
        if (preL > preR) {
            return null;
        }
        int rootVal = preorder[preL];
        TreeNode rootNode = new TreeNode(rootVal);
        int index = inL;
        for (; index <= inR; index++) {
            if (inorder[index] == rootVal) {
                break;
            }
        }
        int leftNum = index - inL;
        rootNode.left = buildTree(preorder, preL + 1, preL + leftNum, inorder, inL, index - 1);
        rootNode.right = buildTree(preorder, preL + leftNum + 1, preR, inorder, index + 1, inR);
        return rootNode;
    }
```




