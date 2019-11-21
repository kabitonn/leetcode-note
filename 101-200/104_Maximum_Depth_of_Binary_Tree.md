#  Maximum Depth of Binary Tree(E)
[104. Maximum Depth of Binary Tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

##  题目描述(简单)

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Note**: A leaf is a node with no children.

Example:
```
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
return its depth = 3.
```


##  思路

1. 递归
2. 迭代

##  解决方法

### 递归DFS
DFS深度遍历

```java
    public int maxDepth(TreeNode p) {
    	if(p==null) {return 0;}
        return 1+Math.max(maxDepth(p.left),maxDepth(p.right));
    }
```


### 迭代DFS


```java
    public int maxDepth(TreeNode root) {
    	if(root==null) {return 0;}
        Deque<TreeNode> queue = new LinkedList<>();
        Deque<Integer> value = new LinkedList<>();
        queue.push(root);
        value.push(1);
        int depth = 0;
        while(!queue.isEmpty()) {
        	TreeNode pNode = queue.pop();
        	int curDepth = value.pop();
        	depth = Math.max(depth, curDepth);
        	if(pNode.left!=null) {
        		queue.push(pNode.left);
        		value.push(1+curDepth);
        	}
        	if(pNode.right!=null) {
        		queue.push(pNode.right);
        		value.push(1+curDepth);
        	}
        	
        }
        return depth;
    }
```

### 迭代BFS
层次遍历层次即深度

```java
    public int maxDepth(TreeNode root) {
    	if(root==null) {return 0;}
        Deque<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int depth = 0;
        while(!queue.isEmpty()) {
        	int size = queue.size();
        	depth++;
        	for(int i=0;i<size;i++) {
        		TreeNode pNode = queue.poll();
        		if(pNode.left!=null) {queue.add(pNode.left);}
        		if(pNode.right!=null) {queue.add(pNode.right);}
        	}
        }
        return depth;
    }
```