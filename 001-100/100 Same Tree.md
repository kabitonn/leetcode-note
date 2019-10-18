# 100. Same Tree
[100. Same Tree](https://leetcode-cn.com/problems/same-tree/)

## 题目描述(简单)

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

Example 1:
```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true
```
Example 2:
```
Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false
```
Example 3:
```
Input:     1         1
          / \       / \
         2   1     1   2

        [1,2,1],   [1,1,2]

Output: false
```


## 思路

1. 递归
2. 迭代

## 解决方法

### 递归
只要把两个树同时遍历一下，遍历过程中判断数值是否相等或者同时为 null 即可
而遍历的方法，当然可以选择 DFS 里的先序遍历，中序遍历，后序遍历，或者 BFS。

```java
	public boolean isSameTree(TreeNode p, TreeNode q) {
		if(p==null&&q==null) {return true;}
		else if(p==null||q==null) {return false;}
        if(p.val!=q.val) {return false;}
        else {return isSameTree(p.left, q.left)&&isSameTree(p.right, q.right);}
    }
```
时间复杂度：O(N)。对每个节点进行了访问。

空间复杂度：O(h)，h 是树的高度，也就是压栈所耗费的空间。当然 h 最小为 log(N)，最大就等于 N。
```
最好情况例子

      1
    /  \
   2    3
  / \  / \
 4  5 6   7

最差情况例子
     1
      \
       2
        \
         3
          \
           4
```
### 迭代

自定义栈

```java
	public boolean isSameTree(TreeNode p, TreeNode q) {
		Deque<TreeNode> stackP = new LinkedList<>();
		Deque<TreeNode> stackQ = new LinkedList<>();
		stackP.push(p);
		stackQ.push(q);
		while(!stackP.isEmpty()&&!stackQ.isEmpty()) {
			p = stackP.pop();
			q = stackQ.pop();
			if(p==null&q==null) {continue;}
			if(!isSame(p, q)) {return false;}
			stackP.push(p.left);
			stackP.push(p.right);
			stackQ.push(q.left);
			stackQ.push(q.right);
		}
        return stackP.isEmpty()&&stackQ.isEmpty();
    }
	public boolean isSame(TreeNode p, TreeNode q) {
		if(p==null&&q==null) {return true;}
		else if(p==null||q==null) {return false;}
        if(p.val!=q.val) {return false;}
        return true;
	}
```

