## [107. Binary Tree Level Order Traversal II](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

## 1. 题目描述(简单)

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
```
Given binary tree [3,9,20,null,null,15,7],
    3
   / \
  9  20
    /  \
   15   7
return its bottom-up level order traversal as:
[
  [15,7],
  [9,20],
  [3]
]
```


## 2. 思路
层次遍历BFS

## 3. 解决方法

### 3.1 迭代BFS


```java
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
    	LinkedList<List<Integer>> lines = new LinkedList<>();
    	if(root==null) {return lines;}
    	Deque<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while(!queue.isEmpty()) {
        	List<Integer> line = new ArrayList<>();
            int size = queue.size();
        	for(int i=0;i<size;i++) {
        		TreeNode pNode = queue.poll();
        		line.add(pNode.val);
        		if(pNode.left!=null) {	queue.add(pNode.left);}
        		if(pNode.right!=null) {	queue.add(pNode.right);}
        	}
        	//lines.add(line);
        	lines.push(line);
        }
        //Collections.reverse(lines);
        return lines;
    }
```


### 3.2 递归BFS


```java
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
    	List<List<Integer>> lines = new ArrayList<>();
    	if(root==null) {return lines;}
    	levelOrder(root, lines, 0);
    	Collections.reverse(lines);
    	return lines;
    }
    public void levelOrder(TreeNode p,List<List<Integer>> lines, int depth) {
    	if(p==null) {return;}
    	if(depth+1>lines.size()) {
    		lines.add(new ArrayList<>());
    	}
    	List<Integer> line = lines.get(depth);
    	line.add(p.val);
    	levelOrder(p.left, lines, depth+1);
    	levelOrder(p.right, lines, depth+1);
    	
    }
```



