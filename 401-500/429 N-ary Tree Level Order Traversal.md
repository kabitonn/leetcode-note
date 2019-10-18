## [429. N-ary Tree Level Order Traversal](https://leetcode-cn.com/problems/n-ary-tree-level-order-traversal/)

## 1. 题目描述\(简单\)

Given an n-ary tree, return the level order traversal of its nodes' values. \(ie, from left to right, level by level\).

For example, given a 3-ary tree:

![](/assets/401-500/429-problem-1.png)

```
We should return its level order traversal:

[
     [1],
     [3,2,4],
     [5,6]
]
```

**Note:**

1. The depth of the tree is at most 1000.
2. The total number of nodes is at most 5000.

## 2. 思路

层次遍历BFS

## 3. 解决方法

### 3.1 迭代

队列实现层次遍历BFS
```java
	public List<List<Integer>> levelOrder(Node root) {
		Deque<Node> queue = new LinkedList<>();
		List<List<Integer>> lists = new ArrayList<>();
		if(root==null) {return lists;}
		queue.add(root);
		while(!queue.isEmpty()) {
			int size = queue.size();
			List<Integer> list = new ArrayList<>();
			for(int i=0;i<size;i++) {
				Node node = queue.poll();
				list.add(node.val);
				for(Node n:node.children) {
					queue.add(n);
				}
			}
			lists.add(list);
		}
		return lists;
	}
```



### 3.2 递归


```java
	public List<List<Integer>> levelOrder(Node root) {
		List<List<Integer>> lists = new ArrayList<>();
		if(root==null) {return lists;}
		getLists(lists,0,root);
		return lists;
	}
	public void getLists(List<List<Integer>> lists, int depth, Node pNode) {
		if(depth==lists.size()) {
			lists.add(new ArrayList<>());
		}
		lists.get(depth).add(pNode.val);
		for(Node n:pNode.children) {
			getLists(lists, depth+1, n);
		}
	}
```




