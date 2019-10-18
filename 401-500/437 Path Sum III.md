# 437. Path Sum III(E)
[437. Path Sum III](https://leetcode-cn.com/problems/path-sum-iii/)

## 题目描述(简单)

You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

Example:
```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

Return 3. The paths that sum to 8 are:

1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11
```

## 思路

## 解决方法

### 双重递归
遍历节点作为起始点，计算sum

```java
    public int pathSum(TreeNode root, int sum) {
    	if(root==null) {return 0;}
        return getPathSum(root, sum)+pathSum(root.left,sum)+pathSum(root.right, sum);
    }

    public int getPathSum( TreeNode pNode, int sum) {
		if(pNode==null) {return 0;}
		sum -= pNode.val;
		return (sum==0?1:0)+getPathSum( pNode.left, sum)+getPathSum( pNode.right, sum);
	}
```



### HashMap缓存 回溯

取DFS加回溯，每次访问到一个节点，把该节点加入到当前的pathSum中
然后判断是否存在一个之前的前n项和，其值等于pathSum与sum之差
如果有，就说明现在的前n项和，减去之前的前n项和，等于sum，那么也就是说，这两个点之间的路径和，就是sum


```java
    public int pathSum(TreeNode root, int sum) {
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        map.put(0, 1);
        return getPathSum1(root, map, sum, 0);
    }
    
    int getPathSum(TreeNode pNode, HashMap<Integer, Integer> map, int target, int pathSum){
        int paths = 0;
        if(pNode == null) return 0;
        
        pathSum += pNode.val;
        paths += map.getOrDefault(pathSum - target, 0);
        map.put(pathSum, map.getOrDefault(pathSum, 0) + 1);
        paths = getPathSum(pNode.left, map, target, pathSum) + getPathSum(pNode.right, map, target, pathSum) + paths;
        map.put(pathSum, map.get(pathSum) - 1);
        return paths;
    }
```



