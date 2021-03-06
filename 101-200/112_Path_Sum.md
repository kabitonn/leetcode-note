# 112. Path Sum(E)
[112. Path Sum](https://leetcode-cn.com/problems/path-sum/)

## 题目描述(简单)

Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

**Note**: A leaf is a node with no children.

Example:
```
Given the below binary tree and sum = 22,

      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
```


## 思路

1. 递归
2. 迭代

## 解决方法

### 递归DFS


```java
    public boolean hasPathSum(TreeNode p, int sum) {
        if (p == null) {
            return false;
        }
        if (p.left == null && p.right == null && sum == p.val) {
            return true;
        } else {
            return hasPathSum(p.left, sum - p.val) || hasPathSum(p.right, sum - p.val);
        }
    }
```
时间复杂度：O(n)，因为我们遍历整个输入树一次，所以总的运行时间为 O(n)，其中 n 是树中结点的总数。
空间复杂度：递归调用的次数受树的高度限制。在最糟糕情况下，树是线性的，其高度为 O(n)。因此，在最糟糕的情况下，由栈上的递归调用造成的空间复杂度为 O(n)。



### 迭代DFS


```java
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root==null) {return false;}
        Deque<TreeNode> stack = new LinkedList<>();
        Deque<Integer> leftSumStack = new LinkedList<>();
        stack.push(root);
        leftSumStack.push(sum);
        while(!stack.isEmpty()) {
        	TreeNode pNode = stack.pop();
        	int leftSum = leftSumStack.pop();
        	if(pNode.left==null&&pNode.right==null&&leftSum==pNode.val) {
        		return true;
        	}
        	if(pNode.left!=null) {
        		stack.push(pNode.left);
        		leftSumStack.push(leftSum-pNode.val);
        	}
        	if(pNode.right!=null) {
        		stack.push(pNode.right);
        		leftSumStack.push(leftSum-pNode.val);
        	}
        }
        return false;
    }
```
时间复杂度：和递归方法相同是  O(n)。
空间复杂度：当树不平衡的最坏情况下是  O(n) 。在最好情况（树是平衡的）下是 O(log(N))。

### 迭代DFS 中序 / 后序

```java
//中序DFS
    public boolean hasPathSum2(TreeNode root, int sum) {
        if (root == null) {
            return false;
        }
        Stack<TreeNode> stack = new Stack<>();
        Stack<Integer> leftSumStack = new Stack<>();
        TreeNode p = root;
        int leftSum = sum;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                stack.push(p);
                leftSum -= p.val;
                leftSumStack.push(leftSum);
                p = p.left;
            }
            p = stack.pop();
            leftSum = leftSumStack.pop();
            if (leftSum == 0 && p.left == null && p.right == null) {
                return true;
            }
            p = p.right;
        }
        return false;
    }

    //后序DFS
    public boolean hasPathSum3(TreeNode root, int sum) {
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        TreeNode preVisited = null;
        int leftSum = sum;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                stack.push(p);
                leftSum -= p.val;
                p = p.left;
            }
            p = stack.peek();
            if (leftSum == 0 && p.left == null && p.right == null) {
                return true;
            }
            if (p.right == null || p.right == preVisited) {
                preVisited = p;
                leftSum += p.val;
                stack.pop();
                p = null;
            } else {
                p = p.right;
            }
        }
        return false;
    }
```
