
# 516. Longest Palindromic Subsequence(M)

[516. 最长回文子序列](https://leetcode-cn.com/problems/longest-palindromic-subsequence/)

## 题目描述(中等)

给定一个字符串s，找到其中最长的回文子序列。可以假设s的最大长度为1000。

**示例 1**:
```
输入:

"bbbab"
输出:

4
一个可能的最长回文子序列为 "bbbb"。
```

**示例 2**:
```
输入:

"cbbd"
输出:

2
一个可能的最长回文子序列为 "bb"。
```


## 思路

动态规划

## 解决方法

### 动态规划

- 状态：`dp[i][j`:表示 s 的第 i 个第 j 个字符组成的子串中，最长的回文子序列长度
- 状态转移：
  - `s[i]==s[j]：dp[i][j] = 2 + dp[i + 1][j - 1]`
  - `s[i]!=s[j]：dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])`
  - i 从最后一个字符开始往前遍历，j 从 i + 1 开始往后遍历，这样可以保证每个子问题都已经计算
- 初始化：`dp[i][i] = 1`

```java
        public int longestPalindromeSubseq(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int n = s.length();
        int[][] dp = new int[n][n];
        for (int i = n - 1; i >= 0; i--) {
            dp[i][i] = 1;
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    dp[i][j] = 2 + dp[i + 1][j - 1];
                } else {
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][n - 1];
    }
```

### 动态规划 空间优化

在计算`dp[i][j](i!=j)`时只用到`dp[i+1][j],dp[i][j-1]或dp[i+1][j-1]`，采用新的dp数组来记录当前状态

```java
    public int longestPalindromeSubseq2(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int n = s.length();
        int[] dp = new int[n];
        for (int i = n - 1; i >= 0; i--) {
            int[] next = new int[n];
            next[i] = 1;
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    next[j] = 2 + dp[j - 1];
                } else {
                    next[j] = Math.max(dp[j], next[j - 1]);
                }
            }
            dp = next;
        }
        return dp[n - 1];
    }
```

### 动态规划 空间优化2

在计算`dp[i][j](i!=j)`时只用到`dp[i+1][j],dp[i][j-1]或dp[i+1][j-1]`，只有dp[i+1][j-1]可能在使用前已被覆盖更新，所以额外记录这一个值即可；
- `s[i] == s[j]: dp[j] = prev + 2`
- `s[i] != s[j]: dp[j] = max(dp[j], dp[j-1])`

```java
    public int longestPalindromeSubseq1(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int n = s.length();
        int[] dp = new int[n];
        for (int i = n - 1; i >= 0; i--) {
            dp[i] = 1;
            int prev = 0;
            for (int j = i + 1; j < n; j++) {
                int temp = dp[j];
                if (s.charAt(i) == s.charAt(j)) {
                    dp[j] = 2 + prev;
                } else {
                    dp[j] = Math.max(dp[j], dp[j - 1]);
                }
                prev = temp;
            }
        }
        return dp[n - 1];
```
