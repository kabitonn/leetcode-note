## [404. Sum of Left Leaves](https://leetcode-cn.com/problems/sum-of-left-leaves/)

## 1. 题目描述(简单)

Find the sum of all left leaves in a given binary tree.

Example:
```
    3
   / \
  9  20
    /  \
   15   7

There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
```

## 2. 思路
判断左节点是叶子结点
## 3. 解决方法

### 3.1 递归


```java
	public int sumOfLeftLeaves(TreeNode root) {
		if(root==null) {return 0;}
		if(root.left!=null&&root.left.left==null&&root.left.right==null) {return root.left.val+sumOfLeftLeaves(root.right);}
		return sumOfLeftLeaves(root.left)+sumOfLeftLeaves(root.right);
	}
```



### 3.2 迭代


```java
	public int sumOfLeftLeaves(TreeNode root) {
        if(root==null) {return 0;}
		Stack<TreeNode> stack = new Stack<>();
		stack.push(root);
		int sum = 0;
		while(!stack.isEmpty()) {
			TreeNode pNode = stack.pop();
			if(pNode==null) {continue;}
			if(pNode.left!=null&&pNode.left.left==null&&pNode.left.right==null) {
				sum+=pNode.left.val;
				stack.push(pNode.right);
			}
			else {
				stack.push(pNode.left);
				stack.push(pNode.right);
			}

		}
		return sum;
    }
```


```java
	public int sumOfLeftLeaves(TreeNode root) {
		if(root==null) {return 0;}
		Stack<TreeNode> stack = new Stack<>();
		stack.push(root);
		int sum = 0;
		while(!stack.isEmpty()) {
			TreeNode pNode = stack.pop();
			if(pNode.left!=null) {
				if(pNode.left.left==null&&pNode.left.right==null) {
					sum+=pNode.left.val;
				}
				else {
					stack.push(pNode.left);
				}
			}
			if(pNode.right!=null) {
				stack.push(pNode.right);
			}

		}
		return sum;
	}
```



