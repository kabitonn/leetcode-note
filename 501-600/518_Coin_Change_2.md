
# 518. Coin Change 2(M)

[518. 零钱兑换 II](https://leetcode-cn.com/problems/coin-change-2/)

## 题目描述(中等)

给定不同面额的硬币和一个总金额。写出函数来计算可以凑成总金额的硬币组合数。假设每一种面额的硬币有无限个。 


示例 1:
```
输入: amount = 5, coins = [1, 2, 5]
输出: 4
解释: 有四种方式可以凑成总金额:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

示例 2:
```
输入: amount = 3, coins = [2]
输出: 0
解释: 只用面额2的硬币不能凑成总金额3。
示例 3:

输入: amount = 10, coins = [10] 
输出: 1
```


## 思路

背包问题
- 回溯
- 动态规划

## 解决方法

### 回溯 剪枝

!> 超时

```java
    public int change(int amount, int[] coins) {
        Arrays.sort(coins);
        return change(coins, amount, 0);
    }

    public int change(int[] coins, int remain, int start) {
        if (remain == 0) {
            return 1;
        }
        int sum = 0;
        for (int i = start; i < coins.length && remain >= coins[i]; i++) {
            sum += change(coins, remain - coins[i], i);
        }
        return sum;
    }

    public int change0(int amount, int[] coins) {
        Arrays.sort(coins);
        return change0(coins, amount, 0, new Integer[coins.length + 1][amount + 1]);
    }
```

### 递归 记忆化

memo[start][remain]:记录nums[start:length)内的数字组合成remain的个数

```java
    public int change0(int amount, int[] coins) {
        Arrays.sort(coins);
        return change0(coins, amount, 0, new Integer[coins.length + 1][amount + 1]);
    }

    public int change0(int[] coins, int remain, int start, Integer[][] memo) {
        if (remain == 0) {
            return 1;
        }
        if (memo[start][remain] != null) {
            return memo[start][remain];
        }
        int sum = 0;
        for (int i = start; i < coins.length && remain >= coins[i]; i++) {
            sum += change0(coins, remain - coins[i], i, memo);
        }
        memo[start][remain] = sum;
        return sum;
    }
```


### 动态规划

可以看作是背包问题
- dp[i][j]代表仅利用前i种(从1开始数起)钱币能组成和为j的情况数
- 状态转移方程为：dp[i][j] = dp[i - 1][j] + dp[i][j - coins[i - 1]];
。

```java
    public int change_1(int amount, int[] coins) {
        int n = coins.length;
        Arrays.sort(coins);
        int[][] dp = new int[n + 1][amount + 1];
        dp[0][0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j <= amount; j++) {
                dp[i][j] = dp[i - 1][j];
                if (j >= coins[i - 1]) {
                    dp[i][j] += dp[i][j - coins[i - 1]];
                }
            }
        }
        return dp[n][amount];
    }
```
 
### 动态规划 空间优化

```java
    public int change1(int amount, int[] coins) {
        Arrays.sort(coins);
        int[] dp = new int[amount + 1];
        dp[0] = 1;
        for (int coin : coins) {
            for (int j = coin; j <= amount; j++) {
                dp[j] += dp[j - coin];
            }
        }
        return dp[amount];
    }
```
