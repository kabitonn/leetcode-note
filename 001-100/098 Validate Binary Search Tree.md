# 098. Validate Binary Search Tree(M)
[098. 验证二叉搜索树](https://leetcode-cn.com/problems/validate-binary-search-tree/)

## 题目描述(中等)
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
 

Example 1:
```
    2
   / \
  1   3

Input: [2,1,3]
Output: true
```
Example 2:
```
    5
   / \
  1   4
     / \
    3   6

Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
```

## 思路

- 递归
- 中序遍历
- DFS
- DFS BFS

## 解决方法


### 递归

判断左子树的最右节点小于根节点
判断右子树的最左节点大于根节点
判断左右子树也是二叉搜索树

```java
    public boolean isValidBST(TreeNode root) {
        if (root == null) {
            return true;
        }

        if (root.left != null) {
            TreeNode right = root.left;
            while (right.right != null) {
                right = right.right;
            }
            if (right.val >= root.val) {
                return false;
            }
        }
        if (root.right != null) {
            TreeNode left = root.right;
            while (left.left != null) {
                left = left.left;
            }
            if (left.val <= root.val) {
                return false;
            }
        }
        return isValidBST(root.left) && isValidBST(root.right);
    }
```

### 迭代 中序遍历

中序遍历顺序会是左孩子，根节点，右孩子。二分查找树的性质，左孩子小于根节点，根节点小于右孩子。

只需要临近的两个数的相对关系，所以我们只需要在遍历过程中，把当前遍历的结果和上一个结果比较即可

```java
    public boolean isValidBST1(TreeNode root) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        TreeNode pre = null;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                stack.push(p);
                p = p.left;
            }
            p = stack.pop();
            if (pre != null && pre.val >= p.val) {
                return false;
            }
            pre = p;
            p = p.right;

        }
        return true;
    }
```

### DFS

根节点决定了左孩子和右孩子的合法取值范围

```
      10
    /    \
   5     15
  / \    /  
 3   6  7 

   考虑 10 的范围
     10(-inf,+inf)

   考虑 5 的范围
     10(-inf,+inf)
    /
   5(-inf,10)

   考虑 3 的范围
       10(-inf,+inf)
      /
   5(-inf,10)
    /
  3(-inf,5)  

   考虑 6 的范围
       10(-inf,+inf)
      /
   5(-inf,10)
    /       \
  3(-inf,5)  6(5,10)

   考虑 15 的范围
      10(-inf,+inf)
    /          \
    5(-inf,10) 15(10,+inf）
    /       \
  3(-inf,5)  6(5,10)  

   考虑 7 的范围，出现不符合返回 false
       10(-inf,+inf)
     /              \
5(-inf,10)           15(10,+inf）
  /       \             /
3(-inf,5)  6(5,10)   7（10,15）
```

```java
    public boolean isValidBST2(TreeNode root) {
        return isValid(root, null, null);
    }

    private boolean isValid(TreeNode t, Integer min, Integer max) {
        if (t == null) {
            return true;
        }
        if (min != null && min >= t.val) {
            return false;
        }
        if (max != null && max <= t.val) {
            return false;
        }
        return isValid(t.left, min, t.val) && isValid(t.right, t.val, max);
    }

```

### DFS 迭代


```java
    public boolean isValidBST3(TreeNode root) {
        if (root == null) {
            return true;
        }
        Stack<TreeNode> stack = new Stack<>();
        Stack<Integer> minValue = new Stack<>();
        Stack<Integer> maxValue = new Stack<>();
        pushStack(stack, minValue, maxValue, root, null, null);
        TreeNode p = null;
        Integer min, max;
        while (!stack.isEmpty()) {
            p = stack.pop();
            min = minValue.pop();
            max = maxValue.pop();
            if (p == null) {
                continue;
            }
            if (min != null && p.val <= min) {
                return false;
            }
            if (max != null && p.val >= max) {
                return false;
            }
            pushStack(stack, minValue, maxValue, p.right, p.val, max);
            pushStack(stack, minValue, maxValue, p.left, min, p.val);

        }
        return true;
    }
    
    private void pushStack(Stack s1, Stack s2, Stack s3, TreeNode t1, Integer m2, Integer m3) {
        s1.push(t1);
        s2.push(m2);
        s3.push(m3);
    }

```

