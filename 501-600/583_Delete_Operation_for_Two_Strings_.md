
# 583. Delete Operation for Two Strings(M)
 
[583. 两个字符串的删除操作](https://leetcode-cn.com/problems/delete-operation-for-two-strings/)

## 题目描述(中等)

给定两个单词 word1 和 word2，找到使得 word1 和 word2 相同所需的最小步数，每步可以删除任意一个字符串中的一个字符。

示例 1:
```
输入: "sea", "eat"
输出: 2
解释: 第一步将"sea"变为"ea"，第二步将"eat"变为"ea"
```

**说明**:
- 给定单词的长度不超过500。
- 给定单词中的字符只含有小写字母。



## 思路

动态规划

- 最小编辑距离
- 最长公共子序列


## 解决方法

### 动态规划 最小编辑距离

dp[i][j] 表示 s1 串前 i 个字符和 s2 串前 j 个字符匹配的最少删除次数。
- s1[i−1] 和 s2[j-1] 匹配，那么我们只需要让 dp[i][j] 赋值为 dp[i-1][j-1] 即可。这是因为两个字符串能匹配的字符不需要被删除。
- 字符 s1[i-1] 和 s2[j-1]不匹配。这种情况下，需要考虑删除两个字符中的哪一个，同时需要增加一次删除操作。两种可能的选择是 dp[i-1][j]和 dp[i][j-1]。从中选择较小值。



```java
    public int minDistance(String s1, String s2) {
        int n1 = s1.length(), n2 = s2.length();
        int[][] dp = new int[n1 + 1][n2 + 1];
        for (int i = 0; i <= n1; i++) {
            for (int j = 0; j <= n2; j++) {
                if (i == 0 || j == 0) {
                    dp[i][j] = i + j;
                } else if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + 1;
                }
            }
        }
        return dp[n1][n2];
    }

```

### 动态规划 空间优化

dp[i][j]只和dp[i-1][j],dp[i][j-1]以及dp[i-1][j-1]有关，只需额外记录dp[i-1][j-1]即可

```java
    public int minDistance1(String s1, String s2) {
        int n1 = s1.length(), n2 = s2.length();
        int[] dp = new int[n2 + 1];
        for (int i = 0; i <= n1; i++) {
            int prev = 0;
            for (int j = 0; j <= n2; j++) {
                int tmp = dp[j];
                if (i == 0 || j == 0) {
                    dp[j] = i + j;
                } else if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    dp[j] = prev;
                } else {
                    dp[j] = Math.min(dp[j], dp[j - 1]) + 1;
                }
                prev = tmp;
            }
        }
        return dp[n2];
    }

```


### 最长公共子序列

求出两个字符串的最长公共子序列长度，总长度减去不需要删除的字符长度*2即代表需要删除的字符个数

```java
    public int minDistance2(String s1, String s2) {
        int n1 = s1.length(), n2 = s2.length();
        if (n1 == 0) {
            return n2;
        } else if (n2 == 0) {
            return n1;
        }
        char[] str1 = s1.toCharArray();
        char[] str2 = s2.toCharArray();
        int[] dp = new int[n2 + 1];
        for (int i = 1; i <= n1; i++) {
            int prev = 0;
            for (int j = 1; j <= n2; j++) {
                int tmp = dp[j];
                if (str1[i - 1] == str2[j - 1]) {
                    dp[j] = prev + 1;
                } else {
                    dp[j] = Math.max(dp[j], dp[j - 1]);
                }
                prev = tmp;
            }
        }
        return n1 + n2 - 2 * dp[n2];
    }

```