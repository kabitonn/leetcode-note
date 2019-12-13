
# 474. Ones and Zeroes(M)

[474. 一和零](https://leetcode-cn.com/problems/ones-and-zeroes/)

## 题目描述(中等)

在计算机界中，我们总是追求用有限的资源获取最大的收益。

现在，假设你分别支配着 m 个 0 和 n 个 1。另外，还有一个仅包含 0 和 1 字符串的数组。

你的任务是使用给定的 m 个 0 和 n 个 1 ，找到能拼出存在于数组中的字符串的最大数量。每个 0 和 1 至多被使用一次。

**注意**:
- 给定 0 和 1 的数量都不会超过 100。
- 给定字符串数组的长度不会超过 600。

示例 1:
```
输入: Array = {"10", "0001", "111001", "1", "0"}, m = 5, n = 3
输出: 4

解释: 总共 4 个字符串可以通过 5 个 0 和 3 个 1 拼出，即 "10","0001","1","0" 。
```

示例 2:
```
输入: Array = {"10", "0", "1"}, m = 1, n = 1
输出: 2

解释: 你可以拼出 "10"，但之后就没有剩余数字了。更好的选择是拼出 "0" 和 "1" 。
```

## 思路

有限背包问题：m个0，n个1 可以看作是背包，而字符串数组strs是物品列表

- 递归 记忆化
- 动态规划

## 解决方法

### 递归 记忆化



自顶向下递归求解，由于递归过程中重复的子问题，采取记忆化，否则会超时；

memo[index][m]n]记录[index,length)区间的字符串在拥有m个0，n个1时，能构成几个字符串；

```java
    public int findMaxForm(String[] strs, int m, int n) {
        return dfs(strs, 0, m, n, new Integer[strs.length][m + 1][n + 1]);
    }

    public int dfs(String[] strs, int index, int m, int n, Integer[][][] memo) {
        if (index == strs.length) {
            return 0;
        }
        if (memo[index][m][n] != null) {
            return memo[index][m][n];
        }
        int one = countOne(strs[index]);
        int zero = strs[index].length() - one;
        int sum1 = 0;
        if (m >= zero && n >= one) {
            sum1 = 1 + dfs(strs, index + 1, m - zero, n - one, memo);
        }
        int sum0 = dfs(strs, index + 1, m, n, memo);
        return memo[index][m][n] = Math.max(sum0, sum1);
    }

    public int countOne(String s) {
        int count = 0;
        for (char c : s.toCharArray()) {
            if (c == '1') {
                count++;
            }
        }
        return count;
    }
```

### 动态规划

dp[i][j][k] : 前i个字符串中，在拥有 j个0，k个1时，能构成几个字符串；

dp[i][j][k] = Math.max(dp[i - 1][j][k], 1 + dp[i - 1][j - zero[i]][k - one[i]]); (当能构成当前字符串时)

```java
    public int findMaxForm1(String[] strs, int m, int n) {
        int num = strs.length;
        int[] zero = new int[num + 1];
        int[] one = new int[num + 1];
        for (int i = 0; i < num; i++) {
            for (char c : strs[i].toCharArray()) {
                if (c == '0') {
                    zero[i + 1]++;
                }
            }
            one[i + 1] = strs[i].length() - zero[i + 1];
        }
        int[][][] dp = new int[num + 1][m + 1][n + 1];
        for (int i = 1; i <= num; i++) {
            for (int j = 0; j <= m; j++) {
                for (int k = 0; k <= n; k++) {
                    if (j < zero[i] || k < one[i]) {
                        dp[i][j][k] = dp[i - 1][j][k];
                    } else {
                        dp[i][j][k] = Math.max(dp[i - 1][j][k], 1 + dp[i - 1][j - zero[i]][k - one[i]]);
                    }
                }
            }
        }
        return dp[num][m][n];
    }
```

### 动态规划 优化

dp[i][][] 只和 dp[i-1][][] 有关，可以对空间进行优化；在状态变化时，若不能构成当前字符串，状态即为上一状态；

在更新j,k时，需要从大到小进行枚举，更新较大的j,k时需要用到小的j,k。

```java
    public int findMaxForm2(String[] strs, int m, int n) {
        int[][] dp = new int[m + 1][n + 1];
        for (String s : strs) {
            int[] count = new int[2];
            for (char c : s.toCharArray()) {
                count[c - '0']++;
            }
            for (int j = m; j >= count[0]; j--) {
                for (int k = n; k >= count[1]; k--) {
                    dp[j][k] = Math.max(dp[j][k], 1 + dp[j - count[0]][k - count[1]]);
                }
            }
        }
        return dp[m][n];
    }

```
