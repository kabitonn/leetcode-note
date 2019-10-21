# 095. Unique Binary Search Trees II(M)
[095. 不同的二叉搜索树 II](https://leetcode-cn.com/problems/unique-binary-search-trees-ii/)

## 题目描述(中等)

Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1 ... n.

Example:
```
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

## 思路

- 递归分治

- 动态规划


## 解决方法


### 递归分治

```java
    public List<TreeNode> generateTrees(int n) {
        List<TreeNode> list = new ArrayList<>();
        if (n == 0) {
            return list;
        }
        return generateTrees(1, n);
    }

    private List<TreeNode> generateTrees(int start, int end) {
        List<TreeNode> list = new ArrayList<>();
        if (start > end) {
            list.add(null);
            return list;
        }
        if (start == end) {
            TreeNode t = new TreeNode(start);
            list.add(t);
            return list;
        }
        for (int i = start; i <= end; i++) {
            List<TreeNode> leftList = generateTrees(start, i - 1);
            List<TreeNode> rightList = generateTrees(i + 1, end);
            for (TreeNode leftTree : leftList) {
                for (TreeNode rightTree : rightList) {
                    TreeNode r = new TreeNode(i);
                    r.left = leftTree;
                    r.right = rightTree;
                    list.add(r);
                }
            }

        }
        return list;
    }
```

### 动态规划

```java
    public List<TreeNode> generateTrees1(int n) {
        List<TreeNode>[] dp = new ArrayList[n + 1];
        dp[0] = new ArrayList<>();
        if (n == 0) {
            return dp[0];
        }
        dp[0].add(null);
        for (int len = 1; len <= n; len++) {
            dp[len] = new ArrayList<>();
            for (int root = 1; root <= len; root++) {
                int leftLen = root - 1;
                int rightLen = len - root;
                for (TreeNode l : dp[leftLen]) {
                    for (TreeNode r : dp[rightLen]) {
                        TreeNode treeNode = new TreeNode(root);
                        treeNode.left = l;
                        treeNode.right = clone(r, root);
                        dp[len].add(treeNode);
                    }
                }

            }
        }
        return dp[n];
    }
    
    private TreeNode clone(TreeNode t, int offset) {
        if (t == null) {
            return null;
        }
        TreeNode root = new TreeNode(t.val + offset);
        root.left = clone(t.left, offset);
        root.right = clone(t.right, offset);
        return root;
    }
```


### 动态规划空间优化

```java

```