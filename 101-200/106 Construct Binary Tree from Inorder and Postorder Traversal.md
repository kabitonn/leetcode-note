# 106. Construct Binary Tree from Inorder and Postorder Traversal(M)


[106. 从中序与后序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)


## 题目描述(中等)

Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

For example, given
```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
Return the following binary tree:

    3
   / \
  9  20
    /  \
   15   7

```

## 思路

- 递归
- 迭代

## 解决方法



### 递归

中序遍历的顺序是 左子树 -> 根节点 -> 右子树。
后序遍历的顺序是 左子树 -> 右子树 -> 根节点。

先确定根节点，然后在中序遍历中找根节点的位置，然后分出左子树和右子树。

```java
    public TreeNode buildTree(int[] inorder, int[] postorder) {
        int n = inorder.length;
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < n; i++) {
            map.put(inorder[i], i);
        }
        return buildTree(map, inorder, 0, n - 1, postorder, 0, n - 1);
    }

    private TreeNode buildTree(Map<Integer, Integer> map, int[] inorder, int inL, int inR, int[] postorder, int postL, int postR) {
        if (postL > postR) {
            return null;
        }
        int rootVal = postorder[postR];
        TreeNode rootNode = new TreeNode(rootVal);
        int index = map.get(rootVal);
        int rightNum = inR - index;
        int leftNum = index - inL;
        //rootNode.right = buildTree(map, inorder, index + 1, inR, postorder, postR - rightNum, postR - 1);
        //rootNode.left = buildTree(map, inorder, inL, index - 1, postorder, postL, postR - rightNum - 1);
        rootNode.right = buildTree(map, inorder, index + 1, inR, postorder, postL + leftNum, postR - 1);
        rootNode.left = buildTree(map, inorder, inL, index - 1, postorder, postL, postL + leftNum - 1);
        return rootNode;
    }

```

### 递归2

递归的话得先构造右子树再构造左子树，此外各种指针，也应该从末尾向零走。

```
    3
   / \
  9  20
    /  \
   15   7

s 初始化一个树中所有的数字都不会相等的数，所以代码中用了一个 long 来表示
<------------------
中序
  9, 3, 15, 20, 7
^               ^
s               i

后序
9, 15, 7, 20, 3
              ^  
              p
<-------------------

```
p 和 i 都从右往左进行遍历，所以 p 开始产生的每次都是右子树的根节点。之前代码里的++要相应的改成--。

```java
    private int in;
    private int post;

    public TreeNode buildTree1(int[] inorder, int[] postorder) {
        int n = inorder.length;
        in = n - 1;
        post = n - 1;
        return buildTree(inorder, postorder, (long) Integer.MIN_VALUE - 1);
    }

    private TreeNode buildTree(int[] inorder, int[] postorder, long stop) {
        if (post < 0) {
            return null;
        }
        if (inorder[in] == stop) {
            in--;
            return null;
        }
        int rootVal = postorder[post--];
        TreeNode root = new TreeNode(rootVal);
        root.right = buildTree(inorder, postorder, rootVal);
        root.left = buildTree(inorder, postorder, stop);

        return root;
    }
```


### 迭代1

```java
    public TreeNode buildTree2(int[] inorder, int[] postorder) {
        int n = inorder.length;
        if (n == 0) {
            return null;
        }
        int in = n - 1;
        int post = n - 1;
        int rootVal = postorder[post--];
        TreeNode root = new TreeNode(rootVal);
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        TreeNode curRoot = root;
        while (post >= 0) {
            if (curRoot.val != inorder[in]) {
                curRoot.right = new TreeNode(postorder[post--]);
                curRoot = curRoot.right;
            } else {
                while (!stack.isEmpty() && stack.peek().val == inorder[in]) {
                    in--;
                    curRoot = stack.pop();
                }
                curRoot.left = new TreeNode(postorder[post--]);
                curRoot = curRoot.left;
            }
            stack.push(curRoot);
        }
        return root;
    }
```
### 迭代2

```java
    public TreeNode buildTree3(int[] inorder, int[] postorder) {
        int n = inorder.length;
        if (n == 0) {
            return null;
        }
        int in = n - 1;
        int post = n - 1;
        int rootVal = postorder[post--];
        TreeNode root = new TreeNode(rootVal);
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        for (; post >= 0; post--) {
            TreeNode curRoot = new TreeNode(postorder[post]);
            TreeNode back = null;
            while (!stack.isEmpty() && stack.peek().val == inorder[in]) {
                in--;
                back = stack.pop();
            }
            if (back == null) {
                stack.peek().right = curRoot;
            } else {
                back.left = curRoot;
            }
            stack.push(curRoot);
        }
        return root;
    }
```