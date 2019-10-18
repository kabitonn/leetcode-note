# 226. Invert Binary Tree(E)
[226. Invert Binary Tree](https://leetcode-cn.com/problems/invert-binary-tree/)

## 题目描述(简单)

Invert a binary tree.

Example:
```
Input:

     4
   /   \
  2     7
 / \   / \
1   3 6   9
Output:

     4
   /   \
  7     2
 / \   / \
9   6 3   1
```
**Trivia**:
This problem was inspired by this original tweet by Max Howell:

> Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.


## 思路

1. 递归
2. 迭代
## 解决方法

### 递归
反转一颗空树结果还是一颗空树。对于一颗根为 r，左子树为 $${right}$$， 右子树为 $${left}$$ 的树来说，它的反转树是一颗根为 r，左子树为 $${right}$$ 的反转树，右子树为 $${left}$$ 的反转树的树。

```java
    public TreeNode invertTree(TreeNode p) {
        if(p==null) {return null;}
        if(p.left==null&&p.right==null) {return p;}
        TreeNode left = p.left;
        TreeNode right = p.right;
        p.right = invertTree(left);
        p.left = invertTree(right);
        return p;
    }
```
时间复杂度: O(n)，其中 n 是树中节点的个数。
空间复杂度: O(n)，在最坏情况下栈内需要存放 O(h) 个方法调用，其中 h 是树的高度。由于 $$h\in O(n)$$

### 迭代BFS
核心在于遍历节点，只要能接触到每一个节点，就能反转它的左右孩子，至于遍历方式反而不重要，DFS也可

```java
    public TreeNode invertTree(TreeNode root) {
    	Deque<TreeNode> queue = new LinkedList<>();
    	if(root!=null) {queue.add(root);}
    	while(!queue.isEmpty()) {
    		int size = queue.size();
    		for(int i=0;i<size;i++) {
    			TreeNode p = queue.poll();
    			TreeNode left = p.left;
    			TreeNode right = p.right;
    			p.left = right;
    			p.right = left;
    			if(left!=null) {queue.add(left);}
    			if(right!=null) {queue.add(right);}
    		}
    	}
        return root;
    }
```



