# 337. House Robber III(M)

[337. 打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

## 题目描述(中等)

在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

示例 1:
```
输入: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

输出: 7 
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
```
示例 2:
```
输入: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.
```
## 思路

- 递归
- 动态规划

## 解决方法

### 递归 动态规划

dp[root] 表示当前节点为根的子树的最大收益
dp[root] = Max(dp[l]+dp[r], root.val+dp[ll]+dp[lr]+dp[rr]+dp[rl]);

```java
    public int rob(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int sumRoot = root.val;
        if (root.left != null) {
            sumRoot += rob(root.left.left) + rob(root.left.right);
        }
        if (root.right != null) {
            sumRoot += rob(root.right.left) + rob(root.right.right);
        }
        int sumChild = rob(root.left) + rob(root.right);
        return Math.max(sumRoot, sumChild);
    }
```

### 记忆化递归

```java
    public int rob1(TreeNode root) {
        return rob1(root, new HashMap<>());
    }

    public int rob1(TreeNode root, Map<TreeNode, Integer> memo) {
        if (root == null) {
            return 0;
        }
        if (memo.containsKey(root)) {
            return memo.get(root);
        }
        int sumRoot = root.val;
        if (root.left != null) {
            sumRoot += rob1(root.left.left, memo) + rob1(root.left.right, memo);
        }
        if (root.right != null) {
            sumRoot += rob1(root.right.left, memo) + rob1(root.right.right, memo);
        }
        int sumChild = rob1(root.left, memo) + rob1(root.right, memo);
        int max = Math.max(sumRoot, sumChild);
        memo.put(root, max);
        return max;
    }
```

### 动态规划

- dp[0]表示不选根节点的的max
  - 不包含根节点的最大收益 = 左子树的最大收益 + 右子树最大收益
- dp[1]表示选了根节点的max
  - 包含根节点的最大收益 = 不包含左子节点的左子树最大收益 + 根节点 + 不包含右子节点的最大收益

```java
    public int rob2(TreeNode root) {
        int[] dp = rob2Helper(root);
        return Math.max(dp[0], dp[1]);
    }

    public int[] rob2Helper(TreeNode root) {
        int[] dp = new int[2];
        if (root == null) {
            return dp;
        }
        int[] left = rob2Helper(root.left);
        int[] right = rob2Helper(root.right);
        //dp[0]=max{左子树选与不选根节点的最大值+右子树选与不选根节点的最大值}
        //不包含根节点，最大值为两个子树的最大值之和
        dp[0] = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);
        //dp[1]=左子树不选根节点的左孩子节点（因为选了根节点root,root.left不能再选了）+
        //右子树不选根节点的右孩子节点（因为选了根节点root,root.right不能再选了）+root.val
        //包含根节点，最大值为两个子树不包含根节点的最大值加上根节点的值
        dp[1] = left[0] + right[0] + root.val;
        return dp;
    }
```