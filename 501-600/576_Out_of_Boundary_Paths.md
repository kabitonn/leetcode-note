
# 576. Out of Boundary Paths(M)
 
[576. 出界的路径数](https://leetcode-cn.com/problems/out-of-boundary-paths/)

## 题目描述(中等)

给定一个 m × n 的网格和一个球。球的起始坐标为 (i,j) ，你可以将球移到相邻的单元格内，或者往上、下、左、右四个方向上移动使球穿过网格边界。但是，你最多可以移动 N 次。找出可以将球移出边界的路径数量。答案可能非常大，返回 结果 mod(10^9 + 7) 的值。

 
示例 1：
```
输入: m = 2, n = 2, N = 2, i = 0, j = 0
输出: 6
解释:
```
示例 2：
```
输入: m = 1, n = 3, N = 3, i = 0, j = 1
输出: 12
解释:
```
 

说明:

- 球一旦出界，就不能再被移动回网格内。
- 网格的长度和高度在 [1,50] 的范围内。
- N 在 [0,50] 的范围内。

## 思路

- 递归
- 动态规划

## 解决方法

### 递归 记忆化

```java
    final int mod = (int) (Math.pow(10, 9) + 7);

    public int findPaths1(int m, int n, int N, int i, int j) {
        return findPaths1(m, n, N, i, j, new Integer[N][m][n]);
    }

    public int findPaths1(int m, int n, int N, int i, int j, Integer[][][] memo) {
        if (i < 0 || i >= m || j < 0 || j >= n) {
            return 1;
        }
        if (N == 0) {
            return 0;
        }
        if (memo[N - 1][i][j] != null) {
            return memo[N - 1][i][j];
        }
        long path = 0;
        path += findPaths1(m, n, N - 1, i + 1, j, memo);
        path += findPaths1(m, n, N - 1, i - 1, j, memo);
        path += findPaths1(m, n, N - 1, i, j + 1, memo);
        path += findPaths1(m, n, N - 1, i, j - 1, memo);
        memo[N - 1][i][j] = (int) (path % mod);
        return memo[N - 1][i][j];
    }
```

此处采用int二维数组速度快于Integer二维数组

```java
    public int findPaths2(int m, int n, int N, int i, int j) {
        int[][][] memo = new int[m][n][N];
        for (int r = 0; r < m; r++) {
            for (int c = 0; c < n; c++) {
                Arrays.fill(memo[r][c], -1);
            }
        }
        return findPaths2(m, n, N, i, j, memo);
    }

    public int findPaths2(int m, int n, int N, int i, int j, int[][][] memo) {
        if (i < 0 || i >= m || j < 0 || j >= n) {
            return 1;
        }
        if (N == 0) {
            return 0;
        }
        if (memo[i][j][N - 1] != -1) {
            return memo[i][j][N - 1];
        }
        long path = 0;
        path += findPaths2(m, n, N - 1, i + 1, j, memo);
        path += findPaths2(m, n, N - 1, i - 1, j, memo);
        path += findPaths2(m, n, N - 1, i, j + 1, memo);
        path += findPaths2(m, n, N - 1, i, j - 1, memo);
        memo[i][j][N - 1] = (int) (path % mod);
        return memo[i][j][N - 1];
    }
```

### 动态规划

出边界才算返回，所以采用边界+2，避免边界判断

dp[k][i][j] = dp[k-1][i-1][j] + dp[k-1][i+1][j] + dp[k-1][i][j-1] + dp[k-1][i][j+1]; 问题在于如何避免边界的情况，可以在首尾各加一行一列，这样每个点都有四个邻居。

```java
    public int findPaths3(int m, int n, int N, int i, int j) {
        if (i - N >= 0 && i + N < m && j - N >= 0 && j + N < n) {
            return 0;
        }
        int[][][] dp = new int[N + 1][m + 2][n + 2];
        for (int k = 0; k <= N; k++) {
            for (int r = 0; r <= m + 1; r++) {
                for (int c = 0; c <= n + 1; c++) {
                    if (r == 0 || c == 0 || r == m + 1 || c == n + 1) {
                        dp[k][r][c] = 1;
                    }
                }
            }
        }
        for (int k = 1; k <= N; k++) {
            for (int r = 1; r <= m; r++) {
                for (int c = 1; c <= n; c++) {
                    dp[k][r][c] = (dp[k][r][c] + dp[k - 1][r - 1][c]) % mod;
                    dp[k][r][c] = (dp[k][r][c] + dp[k - 1][r + 1][c]) % mod;
                    dp[k][r][c] = (dp[k][r][c] + dp[k - 1][r][c - 1]) % mod;
                    dp[k][r][c] = (dp[k][r][c] + dp[k - 1][r][c + 1]) % mod;
                }
            }
        }
        return dp[N][i + 1][j + 1] % mod;
    }

```