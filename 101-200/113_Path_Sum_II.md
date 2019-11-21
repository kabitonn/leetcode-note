# 113. Path SumII(M)


[113. 路径总和 II](https://leetcode-cn.com/problems/path-sum-ii/)


## 题目描述(中等)

Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

Note: A leaf is a node with no children.

Example:
```
Given the below binary tree and sum = 22,

      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
Return:

[
   [5,4,11,2],
   [5,8,4,5]
]

```


## 思路

- 回溯 DFS
- 迭代回溯 DFS

## 解决方法



### 回溯 递归 DFS

```java
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> listList = new ArrayList<>();
        pathSum(listList, new ArrayList<>(), root, sum);
        return listList;
    }

    public void pathSum(List<List<Integer>> listList, List<Integer> list, TreeNode p, int sum) {
        if (p == null) {
            return;
        }
        list.add(p.val);
        if (sum == p.val && p.left == null && p.right == null) {
            listList.add(new ArrayList<>(list));
            list.remove(list.size() - 1);
            return;
        }
        pathSum(listList, list, p.left, sum - p.val);
        pathSum(listList, list, p.right, sum - p.val);
        list.remove(list.size() - 1);
    }
```

### 迭代回溯 DFS 

```java
    public List<List<Integer>> pathSum1(TreeNode root, int sum) {
        List<List<Integer>> listList = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        Stack<TreeNode> stack = new Stack<>();
        TreeNode p = root;
        TreeNode preVisited = null;
        int leftSum = sum;
        while (p != null || !stack.isEmpty()) {
            while (p != null) {
                stack.push(p);
                list.add(p.val);
                leftSum -= p.val;
                p = p.left;
            }
            p = stack.peek();
            if (leftSum == 0 && p.left == null && p.right == null) {
                listList.add(new ArrayList<>(list));
            }
            if (p.right == null || p.right == preVisited) {
                preVisited = p;
                leftSum += p.val;
                list.remove(list.size() - 1);
                stack.pop();
                p = null;
            } else {
                p = p.right;
            }
        }
        return listList;
    }
```



