# 064. Minimum Path Sum(M)
[064. 最小路径和](https://leetcode-cn.com/problems/minimum-path-sum/)


## 题目描述(中等)

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

Note: You can only move either down or right at any point in time.

Example:
```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```


## 思路

回溯
动态规划

## 解决方法


### 回溯

```java
    public int minPathSum(int[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int[] min = new int[]{Integer.MAX_VALUE};
        minPathSum(grid, grid.length, grid[0].length, 0, 0, grid[0][0], min);
        return min[0];
    }

    private void minPathSum(int[][] grid, int m, int n, int i, int j, int sum, int[] min) {
        if (i == m - 1 && j == n - 1) {
            min[0] = Math.min(min[0], sum);
            return;
        }
        if (i < m - 1) {
            minPathSum(grid, m, n, i + 1, j, sum + grid[i + 1][j], min);
        }
        if (j < n - 1) {
            minPathSum(grid, m, n, i, j + 1, sum + grid[i][j + 1], min);
        }
    }
```

```java
    public int minPathSum0(int[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        return minPathSum(grid, 0, 0);
    }

    private int minPathSum(int[][] grid, int i, int j) {
        if (i == grid.length || j == grid[0].length) {
            return Integer.MAX_VALUE;
        }
        if (i == grid.length - 1 && j == grid[0].length - 1) {
            return grid[i][j];
        }
        return grid[i][j] + Math.min(minPathSum(grid, i + 1, j), minPathSum(grid, i, j + 1));

    }
```

时间复杂度 ：$$O(2^{m+n})$$。每次移动最多可以有两种选择。
空间复杂度 ：O(m+n)。递归的深度是 m+n。

### 二维动态规划

dp(i,j)=grid(i,j)+min(dp(i-1,j),dp(i,j-1))

```java
    public int minPathSum1(int[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length, n = grid[0].length;
        int[][] pathSum = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) {
                    pathSum[i][j] = grid[i][j];
                } else if (i == 0) {
                    pathSum[i][j] = pathSum[i][j - 1] + grid[i][j];
                } else if (j == 0) {
                    pathSum[i][j] = pathSum[i - 1][j] + grid[i][j];
                } else {
                    pathSum[i][j] = Math.min(pathSum[i - 1][j], pathSum[i][j - 1]) + grid[i][j];
                }
            }
        }
        return pathSum[m - 1][n - 1];
    }
```

时间复杂度 ：O(m*n)。遍历整个矩阵恰好一次。
空间复杂度 ：O(m*n)。额外的一个同大小矩阵。

### 一维动态规划

dp(j)=grid(i,j)+min(dp(j),dp(j-1))

```java
    public int minPathSum2(int[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length, n = grid[0].length;
        int[] pathSum = new int[n];
        pathSum[0] = grid[0][0];
        for (int i = 1; i < n; i++) {
            pathSum[i] = pathSum[i - 1] + grid[0][i];
        }
        for (int i = 1; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (j == 0) {
                    pathSum[j] = pathSum[j] + grid[i][j];
                } else {
                    pathSum[j] = Math.min(pathSum[j], pathSum[j - 1]) + grid[i][j];
                }
            }
        }
        return pathSum[n - 1];
    }
```

时间复杂度 ：O(m*n)。遍历整个矩阵恰好一次。
空间复杂度 ：O(n)。

### 动态规划(不需要额外存储空间)
grid(i,j)=grid(i,j)+min(grid(i+1,j),grid(i,j+1))

```java

```
