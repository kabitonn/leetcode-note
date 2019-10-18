## [111. Minimum Depth of Binary Tree](https://leetcode-cn.com/problems/minimum-depth-of-binary-tree/)

## 1. 题目描述(简单)

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note**: A leaf is a node with no children.

Example:
```
Given binary tree [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
return its minimum depth = 2.
```


## 2. 思路

1. 递归
2. 迭代

## 3. 解决方法

### 3.1 递归DFS


```java
    public int minDepth(TreeNode p) {
    	if(p==null) {return 0;}
		if(p.left==null&&p.right==null) {return 1;}
		if(p.left==null) {return 1+minDepth(p.right);}
		if(p.right==null) {return 1+minDepth(p.left);}
		return 1+Math.min(minDepth(p.left), minDepth(p.right));
    }
```
时间复杂度：O(n)，因为我们遍历整个输入树一次，所以总的运行时间为 O(n)，其中 n 是树中结点的总数。
空间复杂度：递归调用的次数受树的高度限制。在最糟糕情况下，树是线性的，其高度为 O(n)。因此，在最糟糕的情况下，由栈上的递归调用造成的空间复杂度为 O(n)。



### 3.2 迭代DFS


```java
    public int minDepth(TreeNode root) {
    	if(root==null) {return 0;}
        Deque<TreeNode> stack = new LinkedList<>();
        Deque<Integer> value = new LinkedList<>();
        stack.push(root);
        value.push(1);
        int depth = Integer.MAX_VALUE;
        while(!stack.isEmpty()) {
        	TreeNode pNode = stack.pop();
        	int curDepth = value.pop();
        	if(pNode.left==null&&pNode.right==null) {
        		depth = Math.min(depth, curDepth);
        	}
        	else {
        		if(pNode.left!=null) {
        			stack.push(pNode.left);
        			value.push(1+curDepth);
        		}
        		if(pNode.right!=null) {
        			stack.push(pNode.right);
        			value.push(1+curDepth);
        		}
			}
        	
        }
        return depth;
    }
```
时间复杂度：每个节点恰好被访问一遍，复杂度为 O(N)。
空间复杂度：最坏情况下我们会在栈中保存整棵树，此时空间复杂度为O(N)。

### 3.3 迭代BFS


```java
    public int minDepth(TreeNode root) {
    	if(root==null) {return 0;}
        Deque<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int depth = 0;
        while(!queue.isEmpty()) {
        	int size = queue.size();
        	depth++;
        	for(int i=0;i<size;i++) {
        		TreeNode pNode = queue.poll();
        		if(pNode.left==null&&pNode.right==null) {
        			return depth;
        		}
        		else {
        			if(pNode.left!=null) {queue.add(pNode.left);}
        			if(pNode.right!=null) {queue.add(pNode.right);}
        		}
        	}
        }
        return depth;
    }
```
时间复杂度：最坏情况下，这是一棵平衡树，我们需要按照树的层次一层一层的访问完所有节点，除去最后一层的节点。这样访问了 N/2 个节点，因此复杂度是O(N)。
空间复杂度：和时间复杂度相同，也是 O(N)。






