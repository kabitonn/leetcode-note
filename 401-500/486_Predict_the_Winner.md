# 486. Predict the Winner(M)


[486. 预测赢家](https://leetcode-cn.com/problems/predict-the-winner/)

## 题目描述(中等)

给定一个表示分数的非负整数数组。 玩家1从数组任意一端拿取一个分数，随后玩家2继续从剩余数组任意一端拿取分数，然后玩家1拿，……。每次一个玩家只能拿取一个分数，分数被拿取之后不再可取。直到没有剩余分数可取时游戏结束。最终获得分数总和最多的玩家获胜。

给定一个表示分数的数组，预测玩家1是否会成为赢家。你可以假设每个玩家的玩法都会使他的分数最大化。

示例 1:
```
输入: [1, 5, 2]
输出: False
解释: 一开始，玩家1可以从1和2中进行选择。
如果他选择2（或者1），那么玩家2可以从1（或者2）和5中进行选择。如果玩家2选择了5，那么玩家1则只剩下1（或者2）可选。
所以，玩家1的最终分数为 1 + 2 = 3，而玩家2为 5。
因此，玩家1永远不会成为赢家，返回 False。
```

示例 2:
```
输入: [1, 5, 233, 7]
输出: True
解释: 玩家1一开始选择1。然后玩家2必须从5和7中进行选择。无论玩家2选择了哪个，玩家1都可以选择233。
最终，玩家1（234分）比玩家2（12分）获得更多的分数，所以返回 True，表示玩家1可以成为赢家。
```

**注意**:
- 1 <= 给定的数组长度 <= 20.
- 数组里所有分数都为非负数且不会大于10000000。
- 如果最终两个玩家的分数相等，那么玩家1仍为赢家。


## 思路

- DFS 判断自己胜利且对方不能胜利
- 回溯
- 递归 记忆化
- 动态规划

## 解决方法

### DFS


- 偶数个数的数组，先手必胜
> 对于偶数个数字的数组，玩家1一定获胜。因为如果玩家1选择拿法A，玩家2选择拿法B，玩家1输了。则玩家1换一种拿法选择拿法B，因为玩家1是先手，所以玩家1一定可以获胜。

所以只需考虑奇数个数的数组

```java
    public boolean PredictTheWinner(int[] nums) {
        if (nums.length % 2 == 0) {
            return true;
        }
        return canWin(nums, 0, nums.length - 1, 0, 0);
    }

    public boolean canWin(int[] nums, int i, int j, int sum1, int sum2) {
        if (i > j) {
            return sum1 > sum2 || (sum1 == sum2 && nums.length % 2 == 0);
        }
        if (!canWin(nums, i + 1, j, sum2, sum1 + nums[i]) || !canWin(nums, i, j - 1, sum2, sum1 + nums[j])) {
            return true;
        }
        return false;
    }
```
### 递归回溯

`score(int[] nums, int i, int j)`:此时在`[i,j]`之间先手比对方多的分数

```java
    public boolean PredictTheWinner1(int[] nums) {
        return score(nums, 0, nums.length - 1) >= 0;
    }

    public int score(int[] nums, int i, int j) {
        if (i == j) {
            return nums[i];
        }
        int left = nums[i] - score(nums, i + 1, j);
        int right = nums[j] - score(nums, i, j - 1);
        return Math.max(left, right);
    }

```

### 递归 记忆化

`memo[i][j]`:此时在`[i,j]`之间先手比对方多的分数

```java
    public boolean PredictTheWinner2(int[] nums) {
        if (nums.length % 2 == 0) {
            return true;
        }
        return score(nums, 0, nums.length - 1, new Integer[nums.length][nums.length]) >= 0;
    }

    public int score(int[] nums, int i, int j, Integer[][] memo) {
        if (i == j) {
            return nums[i];
        }
        if (memo[i][j] != null) {
            return memo[i][j];
        }
        int left = nums[i] - score(nums, i + 1, j, memo);
        int right = nums[j] - score(nums, i, j - 1, memo);
        memo[i][j] = Math.max(left, right);
        return memo[i][j];
    }
```

### 动态规划

- 状态：`dp[i][j]`:此时在`[i,j]`之间先手比对方多的分数
- 状态转移：`dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1])`
  - 对于`dp[i][j]`，如果先手拿了`nums[i]`，则另一位玩家比先手多`dp[i+1][j], dp[i][j] = nums[i]-dp[i+1][j]`，
  - 如果先手拿了`nums[j]`，则另一位玩家比先手多`dp[i][j-1], dp[i][j] = nums[j]-dp[i][j-1]`

```java
    public boolean PredictTheWinner3(int[] nums) {
        int n = nums.length;
        if (n % 2 == 0) {
            return true;
        }
        int[][] dp = new int[n][n];
        for (int i = n - 1; i >= 0; i--) {
            dp[i][i] = nums[i];
            for (int j = i + 1; j < n; j++) {
                dp[i][j] = Math.max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1]);
            }
        }
        return dp[0][n - 1] >= 0;
    }
```


### 动态规划 空间优化

`dp[i][j]` 只和 `dp[i+1][j], dp[i][j-1]`有关

```java
    public boolean PredictTheWinner4(int[] nums) {
        int n = nums.length;
        if (n % 2 == 0) {
            return true;
        }
        int[] dp = new int[n];
        for (int i = n - 1; i >= 0; i--) {
            dp[i] = nums[i];
            for (int j = i + 1; j < n; j++) {
                dp[j] = Math.max(nums[i] - dp[j], nums[j] - dp[j - 1]);
            }
        }
        return dp[n - 1] >= 0;
    }
```