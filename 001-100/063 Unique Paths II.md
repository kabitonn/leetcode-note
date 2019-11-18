# 063. Unique Paths II\(M\)

[063. 不同路径 II](https://leetcode-cn.com/problems/unique-paths-ii/)

## 题目描述\(中等\)

A robot is located at the top-left corner of a m x n grid \(marked 'Start' in the diagram below\).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid \(marked 'Finish' in the diagram below\).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

![](../assets/001-100/063-p-1.png)

An obstacle and empty space is marked as 1 and 0 respectively in the grid.

Note: m and n will be at most 100.

Example 1:

```
Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
1. Right -> Right -> Down -> Down
2. Down -> Down -> Right -> Right
```

## 思路

动态规划


## 解决方法

### 动态规划

dp[i][j] = obstacleGrid[i][j] == 1 ? 0 : dp[i][j-1] + dp[i-1][j]

```java
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[][] paths = new int[m][n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (obstacleGrid[i][j] == 1) {
                    paths[i][j] = 0;
                } else if (i == 0 && j == 0) {
                    paths[i][j] = 1;
                } else if (i == 0) {
                    paths[i][j] = paths[i][j - 1];
                } else if (j == 0) {
                    paths[i][j] = paths[i - 1][j];
                } else {
                    paths[i][j] = paths[i - 1][j] + paths[i][j - 1];
                }
            }
        }

        return paths[m - 1][n - 1];
    }
```
时间复杂度 ： O(m \* n) 。长方形网格的大小是 m * n，而访问每个格点恰好一次。
空间复杂度 ： O(m \* n)。

### 动态规划空间优化

```java
    public int uniquePathsWithObstacles1(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        int[] paths = new int[n + 1];
        for (int i = 0; i < n; i++) {
            if (obstacleGrid[0][i] == 1) {
                paths[i] = 0;
                break;
            }
            paths[i] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (obstacleGrid[i][j] == 1) {
                    paths[j] = 0;
                } else if (j != 0) {
                    paths[j] = paths[j] + paths[j - 1];
                }
            }
        }
        return paths[n - 1];
    }
```

时间复杂度 ： O(m \* n) 。长方形网格的大小是 m * n，而访问每个格点恰好一次。
空间复杂度 ： O( n)。



### 动态规划利用原数组


```java
    public int uniquePathsWithObstacles2(int[][] obstacleGrid) {

        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;

        if (obstacleGrid[0][0] == 1) {
            return 0;
        }
        obstacleGrid[0][0] = 1;

        // 初始化第一列
        for (int i = 1; i < m; i++) {
            obstacleGrid[i][0] = (obstacleGrid[i][0] == 0 && obstacleGrid[i - 1][0] == 1) ? 1 : 0;
        }
        // 初始化第一行
        for (int i = 1; i < n; i++) {
            obstacleGrid[0][i] = (obstacleGrid[0][i] == 0 && obstacleGrid[0][i - 1] == 1) ? 1 : 0;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 0) {
                    obstacleGrid[i][j] = obstacleGrid[i - 1][j] + obstacleGrid[i][j - 1];
                } else {
                    obstacleGrid[i][j] = 0;
                }
            }
        }
        return obstacleGrid[m - 1][n - 1];
    }

```
时间复杂度 ： O(m \* n) 。长方形网格的大小是 m * n，而访问每个格点恰好一次。
空间复杂度 ： O(1)。我们利用 obstacleGrid 作为 DP 数组，因此不需要额外的空间。

