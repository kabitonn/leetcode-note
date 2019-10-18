## [257. Binary Tree Paths](https://leetcode-cn.com/problems/binary-tree-paths/)

## 1. 题目描述(简单)

Given a binary tree, return all root-to-leaf paths.

Note: A leaf is a node with no children.

Example:
```
Input:

   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]

Explanation: All root-to-leaf paths are: 1->2->5, 1->3
```

## 2. 思路

## 3. 解决方法

### 3.1 递归DFS


```java
public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        if(root==null) {return result;}
        String string = ""+root.val;
        binaryTreePath(root, result, string);
        
        return result;
    }
    public void binaryTreePath(TreeNode p,List<String> list,String str) {
		if(p.left==null&&p.right==null) {
			list.add(str);
		}
		if(p.left!=null){
			binaryTreePath(p.left, list, str+"->"+p.left.val);
		}
		if(p.right!=null) {
			binaryTreePath(p.right, list, str+"->"+p.right.val);
		}
	}
```


```java
	public List<String> binaryTreePaths(TreeNode root) {
		List<String> result = new ArrayList<>();
		if(root==null) {return result;}
		binaryTreePath1(root, result, "");

		return result;
	}
	public void binaryTreePath1(TreeNode p,List<String> list,String str) {
		if(p.left==null&&p.right==null) {
			list.add(str+p.val);
			return;
		}
		str += p.val;
		if(p.left!=null){
			binaryTreePath1(p.left, list, str+"->");
		}
		if(p.right!=null) {
			binaryTreePath1(p.right, list, str+"->");
		}
	}
```



```java
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        binaryTreePath2(root, result, "");

        return result;
    }

    public void binaryTreePath2(TreeNode p, List<String> list, String str) {
        if (p == null) {
            return;
        }
        str += p.val;
        if (p.left == null && p.right == null) {
            list.add(str);
        } else {
            binaryTreePath2(p.left, list, str + "->");
            binaryTreePath2(p.right, list, str + "->");
        }
    }
```
时间复杂度：每个节点只会被访问一次，因此时间复杂度为 $$O(N)$$，其中 N 表示节点数目。
空间复杂度：$$O(N)$$。这里不考虑存储答案 paths 使用的空间，仅考虑额外的空间复杂度。额外的空间复杂度为递归时使用的栈空间，在最坏情况下，当二叉树中每个节点只有一个孩子节点时，递归的层数为 NN，此时空间复杂度为 $$O(N)$$。在最好情况下，当二叉树为平衡二叉树时，它的高度为 $$\log(N)$$，此时空间复杂度为 $$\log(N)$$。


### 3.2 迭代DFS


```java
    public List<String> binaryTreePaths3(TreeNode root) {
        LinkedList<String> paths = new LinkedList<>();
        if (root == null)
            return paths;
        LinkedList<TreeNode> node_stack = new LinkedList<>();
        LinkedList<String> path_stack = new LinkedList<>();
        node_stack.add(root);
        path_stack.add("" + root.val);
        TreeNode node;
        String path;
        while (!node_stack.isEmpty()) {
            node = node_stack.pollLast();
            path = path_stack.pollLast();
            if ((node.left == null) && (node.right == null))
                paths.add(path);
            if (node.left != null) {
                node_stack.add(node.left);
                path_stack.add(path + "->" + node.left.val);
            }
            if (node.right != null) {
                node_stack.add(node.right);
                path_stack.add(path + "->" + node.right.val);
            }
        }
        return paths;
    }
```



```java
    public List<String> binaryTreePaths(TreeNode root) {
        LinkedList<String> paths = new LinkedList<>();
        if (root == null)
            return paths;
        LinkedList<TreeNode> node_stack = new LinkedList<>();
        LinkedList<String> path_stack = new LinkedList<>();
        node_stack.add(root);
        path_stack.add("");
        TreeNode node;
        String path = null;
        while (!node_stack.isEmpty()) {
            node = node_stack.pollLast();
            path = path_stack.pollLast();
            path += node.val;
            if ((node.left == null) && (node.right == null))
                paths.add(path);
            if (node.left != null) {
                node_stack.add(node.left);
                path_stack.add(path + "->");
            }
            if (node.right != null) {
                node_stack.add(node.right);
                path_stack.add(path + "->");
            }
        }
        return paths;
    }
```


时间复杂度：$$O(N)$$，每个节点只会被访问一次。
空间复杂度：$$O(N)$$，在最坏情况下，队列中有 N 个节点。

