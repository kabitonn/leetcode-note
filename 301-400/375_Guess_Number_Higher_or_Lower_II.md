
# 375. Guess Number Higher or Lower II(M)

[375. 猜数字大小 II](https://leetcode-cn.com/problems/guess-number-higher-or-lower-ii/)

## 题目描述(中等)

我们正在玩一个猜数游戏，游戏规则如下：

我从 **1** 到 **n** 之间选择一个数字，你来猜我选了哪个数字。

每次你猜错了，我都会告诉你，我选的数字比你的大了或者小了。

然而，当你猜了数字 **x** 并且猜错了的时候，你需要支付金额为 **x** 的现金。直到你猜到我选的数字，你才算赢得了这个游戏。

示例:
```
n =10, 我选择了8.

第一轮: 你猜我选择的数字是5，我会告诉你，我的数字更大一些，然后你需要支付5块。
第二轮: 你猜是7，我告诉你，我的数字更大一些，你支付7块。
第三轮: 你猜是9，我告诉你，我的数字更小一些，你支付9块。

游戏结束。8 就是我选的数字。

你最终要支付 5 + 7 + 9 = 21 块钱。
```

给定 n ≥ 1，计算你至少需要拥有多少现金才能确保你能赢得这个游戏。

## 思路

## 解决方法

### 递归 记忆化

求最大值中的最小值

cost(1,n)为1到n间选择一个数猜到为止所花费的最少现金,需要考虑最坏情况下的代价

cost(1,n)=min(i+max(cost(1,i−1),cost(i+1,n)))

对于范围 [i, j] 中的每一个数字，都需要分别考虑选为当前的第一个猜测的代价，然后再分别考虑左右两个区间内的代价。但一个重要的发现是如果从范围 [i, (i+j)/2) 内选择数字作为第一次尝试，右边区间都比左边区间大，所以我们只需要从右边区间获取最大开销即可，因为它的开销肯定比左边区间的要大。为了减少这个开销，我们第一次尝试肯定从[(i+j)/2,j)中进行选数。这样子，两个区间的开销会更接近且总体开销会更小。

所以，不需要从 i 到 j 遍历每个数字，只需要从 (i+j)/2 到 j 遍历，且找到暴力解的最小开销即可。

`cost(i,j)=min(k+max(cost(i,k−1),cost(k+1,j))) //(i+j)/2 <= k < j`

```java
    public int getMoneyAmount(int n) {
        return cost(1, n, new Integer[n + 1][n + 1]);
    }

    public int cost(int i, int j, Integer[][] memo) {
        if (i >= j) {
            return 0;
        }
        if (memo[i][j] != null) {
            return memo[i][j];
        }
        int min = Integer.MAX_VALUE;
        for (int k = (i + j) >>> 1; k < j; k++) {
            int max = k + Math.max(cost(i, k - 1, memo), cost(k + 1, j, memo));
            min = Math.min(min, max);
        }
        memo[i][j] = min;
        return memo[i][j];
    }
```

时间复杂度 O(n!)

### 动态规划

dp[i][j] 为[i,j]选择一个数猜到为止所花费的最少现金

dp[i][j] = min(k+max(dp[i][k-1],dp[k+1][j]))

```java
    public int getMoneyAmount1(int n) {
        int[][] dp = new int[n + 1][n + 1];
        for (int len = 2; len <= n; len++) {
            for (int i = 1; i <= n - len + 1; i++) {
                int min = Integer.MAX_VALUE;
                for (int k = i; k < i + len - 1; k++) {
                    int max = k + Math.max(dp[i][k - 1], dp[k + 1][i + len - 1]);
                    min = Math.min(min, max);
                }
                dp[i][i + len - 1] = min;
            }
        }
        return dp[1][n];
    }
```

时间复杂度 O(n^3)

尝试使用 [i,j) 中的每一个数作为第一个选的数。但由于方法 1 中提到的原因，我们只需要从 [i+(len−1)/2,j) 中选第一个数就可以了，其中 len 是当前区间的长度。因此转移方程式为


```java
    public int getMoneyAmount2(int n) {
        int[][] dp = new int[n + 1][n + 1];
        for (int len = 2; len <= n; len++) {
            for (int i = 1; i <= n - len + 1; i++) {
                int min = Integer.MAX_VALUE;
                for (int k = i + ((len - 1) >>> 1); k < i + len - 1; k++) {
                    int max = k + Math.max(dp[i][k - 1], dp[k + 1][i + len - 1]);
                    min = Math.min(min, max);
                }
                dp[i][i + len - 1] = min;
            }
        }
        return dp[1][n];
    }
```

时间复杂度 O(n^3)