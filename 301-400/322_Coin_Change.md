
# 322. Coin Change(M)

[322. 零钱兑换](https://leetcode-cn.com/problems/coin-change/)

## 题目描述(中等)

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

示例 1:
```
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```
示例 2:
```
输入: coins = [2], amount = 3
输出: -1
```
**说明**:
- 你可以认为每种硬币的数量是无限的。



## 思路

背包问题

- 递归回溯

## 解决方法

### 递归回溯(超时)


```java
    public int coinChange(int[] coins, int amount) {
        if (amount < 0) {
            return Integer.MAX_VALUE;
        }
        if (amount == 0) {
            return 0;
        }
        int min = Integer.MAX_VALUE;
        for (int coin : coins) {
            int count = coinChange(coins, amount - coin);
            if (count != -1) {
                min = Math.min(min, count);
            }
        }
        return min != Integer.MAX_VALUE ? min + 1 : -1;
    }
```

### 记忆化 递归



```java
    public int coinChange0(int[] coins, int amount) {
        Integer[] memo = new Integer[amount + 1];
        memo[0] = 0;
        coinChange0(coins, amount, memo);
        return memo[amount] != Integer.MAX_VALUE ? memo[amount] : -1;
    }

    public int coinChange0(int[] coins, int amount, Integer[] memo) {
        if (amount < 0) {
            return Integer.MAX_VALUE;
        }
        if (amount == 0) {
            return 0;
        }
        if (memo[amount] != null) {
            return memo[amount];
        }
        int min = Integer.MAX_VALUE;
        for (int coin : coins) {
            min = Math.min(min, coinChange0(coins, amount - coin, memo));
        }
        memo[amount] = min != Integer.MAX_VALUE ? min + 1 : min;
        return memo[amount];
    }

```

### 动态规划

状态：dp[i] 表示凑齐i的硬币个数，若为MAX_VALUE表示无法凑齐

状态转移：dp[i]=min(dp[i-coin]+1 for coin in coins)

```java
    public int coinChange1(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        for (int i = 1; i <= amount; i++) {
            int min = Integer.MAX_VALUE;
            for (int coin : coins) {
                if (i - coin < 0) {
                    continue;
                }
                min = Math.min(dp[i - coin], min);
            }
            dp[i] = min != Integer.MAX_VALUE ? min + 1 : min;
        }
        return dp[amount] != Integer.MAX_VALUE ? dp[amount] : -1;
    }
```


### BFS

BFS遍历搜索得到可以凑齐的金额，对于凑齐的金额若之前已凑出则不再关注

```java
    public int coinChange2(int[] coins, int amount) {
        Queue<Integer> leftQueue = new LinkedList<>();
        boolean[] visited = new boolean[amount + 1];
        leftQueue.add(amount);
        visited[amount] = true;
        int count = 0;
        while (!leftQueue.isEmpty()) {
            int size = leftQueue.size();
            for (int i = 0; i < size; i++) {
                int left = leftQueue.poll();
                if (left == 0) {
                    return count;
                }
                for (int coin : coins) {
                    int tmp = left - coin;
                    if (tmp >= 0 && !visited[tmp]) {
                        leftQueue.add(tmp);
                        visited[tmp] = true;
                    }
                }
            }
            count++;
        }
        return -1;
    }
```