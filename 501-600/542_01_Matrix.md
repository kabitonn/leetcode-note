
# 542. 01 Matrix(M)

[542. 01 矩阵](https://leetcode-cn.com/problems/01-matrix/)

## 题目描述()

给定一个由 0 和 1 组成的矩阵，找出每个元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。

示例 1:
```
输入:

0 0 0
0 1 0
0 0 0
输出:

0 0 0
0 1 0
0 0 0
```

示例 2:
```
输入:
0 0 0
0 1 0
1 1 1
输出:

0 0 0
0 1 0
1 2 1
```

**注意**:
- 给定矩阵的元素个数不超过 10000。
- 给定矩阵中至少有一个元素是 0。
- 矩阵中的元素只在四个方向上相邻: 上、下、左、右。

## 思路

- 动态规划
- BFS
- DFS

## 解决方法

### 动态规划

对于一个节点来说，它到 0 的距离可以通过邻居的最近距离计算，在这种情况下最近距离是邻居的距离 + 1。因此，这就让我们想到了动态规划！

对于每个 1，到 0 的最短路径可能从任意方向。所以我们需要检查所有 4 个方向。在从上到下的迭代中，我们需要检查左边和上方的最短路径；我们还需要另一个从下往上的循环，检查右边和下方的方向。



```java
    public int[][] updateMatrix(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) {
                    dp[i][j] = m + n;
                }
                if (i > 0) {
                    dp[i][j] = Math.min(dp[i][j], dp[i - 1][j] + 1);
                }
                if (j > 0) {
                    dp[i][j] = Math.min(dp[i][j], dp[i][j - 1] + 1);
                }
            }
        }
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (i < m - 1) {
                    dp[i][j] = Math.min(dp[i][j], dp[i + 1][j] + 1);
                }
                if (j < n - 1) {
                    dp[i][j] = Math.min(dp[i][j], dp[i][j + 1] + 1);
                }
            }
        }
        return dp;
    }
```

### BFS

- 对于广度优先搜索，保存一个队列 q 维护下一个需要检查的点。
- 我们将所有的 0 放入 q 中。
- 初始地，对于每个为 0 的节点距离都是 1，对于所有的 1 都是 INT_MAX，会根据广搜的结果更新。
- 弹出队列中的元素，检查它的邻居。如果对于邻居 {i,j} 新计算的距离更小，将 {i,j} 加入队列 q 中同时更新距离 dp[i][j]。



```java
    int[][] dir = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};

    public int[][] updateMatrix1(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int[][] dp = new int[m][n];
        Queue<int[]> queue = new LinkedList<>();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    queue.offer(new int[]{i, j});
                } else {
                    dp[i][j] = m + n;
                }
            }
        }
        while (!queue.isEmpty()) {
            int[] pos = queue.poll();
            int x = pos[0], y = pos[1];
            for (int[] d : dir) {
                int i = x + d[0];
                int j = y + d[1];
                if (i < 0 || i >= m || j < 0 || j >= n) {
                    continue;
                }
                if (dp[i][j] > dp[x][y] + 1) {
                    dp[i][j] = dp[x][y] + 1;
                    queue.offer(new int[]{i, j});
                }
            }
        }
        return dp;
    }

```

### DFS 四个方向

从0开始深度遍历，遍历过程更新到0的最短距离，若距离变短了，则以该点为起点继续深度遍历

```java
    int[][] dir = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};

    public int[][] updateMatrix2(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) {
                    dp[i][j] = m + n;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (dp[i][j] == 0) {
                    dfs2(dp, i, j);
                }
            }
        }
        return dp;
    }

    public void dfs2(int[][] dp, int x, int y) {
        for (int[] d : dir) {
            int i = x + d[0];
            int j = y + d[1];
            if (i < 0 || i >= dp.length || j < 0 || j >= dp[0].length) {
                continue;
            }
            if (dp[i][j] > dp[x][y] + 1) {
                dp[i][j] = dp[x][y] + 1;
                dfs2(dp, i, j);
            }
        }
    }
```

### DFS 两个方向

从1开始深度遍历，确定该位置距离0的最短距离
若1的邻接有0,则必然最近距离为1，否则根据周围的1选取最近距离，只向右和下两个方向递归，左和上两个方向的值若不为0则说明已更新到最近距离，若仍为0，当前点是由左和上两个方向的点深度遍历而来的，

```java
    public int[][] updateMatrix3(int[][] matrix) {
        int m = matrix.length, n = matrix[0].length;
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 1) {
                    dfs3(matrix, dp, i, j);
                }
            }
        }
        return dp;
    }

    public int dfs3(int[][] matrix, int[][] dp, int x, int y) {
        int m = matrix.length, n = matrix[0].length;
        if (x < 0 || x >= m || y < 0 || y >= n) {
            return m + n;
        }
        if (matrix[x][y] == 0) {
            return 0;
        }
        if(dp[x])
        if ((x > 0 && matrix[x - 1][y] == 0) ||
            (y > 0 && matrix[x][y - 1] == 0) ||
            (x < m - 1 && matrix[x + 1][y] == 0) ||
            (y < n - 1 && matrix[x][y + 1] == 0)) {
            dp[x][y] = 1;
            return 1;
        }
        int min = m + n;
        if (x > 0 && dp[x - 1][y] != 0) {
            min = Math.min(min, dp[x - 1][y]);
        }
        if (y > 0 && dp[x][y - 1] != 0) {
            min = Math.min(min, dp[x][y - 1]);
        }
        min = Math.min(min, dfs3(matrix, dp, x + 1, y));
        min = Math.min(min, dfs3(matrix, dp, x, y + 1));
        dp[x][y] = min + 1;
        return dp[x][y];
    }
```