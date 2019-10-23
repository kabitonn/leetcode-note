# 200. Number of Islands(M)

[200. 岛屿数量](https://leetcode-cn.com/problems/number-of-islands/)


## 题目描述(中等)


Given a 2d grid map of **'1'**s (land) and **'0'**s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example 1:
```
Input:
11110
11010
11000
00000

Output: 1
```
Example 2:
```
Input:
11000
11000
00100
00011

Output: 3
```

## 思路

- DFS
- BFS
- 并查集

## 解决方法



### DFS

从1的位置开始连通，数目+1，不通后则继续遍历

```java
    public int numIslands(char[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length, n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        int num = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1' && !visited[i][j]) {
                    num++;
                    connect(grid, i, j, visited);
                }
            }
        }
        return num;
    }

    public void connect(char[][] grid, int i, int j, boolean[][] visited) {
        if (i < 0 || i == grid.length || j < 0 || j == grid[0].length) {
            return;
        }
        if (visited[i][j]) {
            return;
        }
        if (grid[i][j] == '1') {
            visited[i][j] = true;
            connect(grid, i + 1, j, visited);
            connect(grid, i, j + 1, visited);
            connect(grid, i - 1, j, visited);
            connect(grid, i, j - 1, visited);
        }
    }
```
时间复杂度 : O(m×n)，其中 m 和 n 分别为行数和列数。

空间复杂度 : O(m×n)



**使用原数组标记**


```java
    public int numIslands1(char[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length, n = grid[0].length;
        int num = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    num++;
                    connect1(grid, i, j);
                }
            }
        }
        return num;
    }

    public void connect1(char[][] grid, int i, int j) {
        if (i < 0 || i == grid.length || j < 0 || j == grid[0].length) {
            return;
        }

        if (grid[i][j] == '0') {
            return;
        }
        grid[i][j] = '0';
        connect1(grid, i + 1, j);
        connect1(grid, i, j + 1);
        connect1(grid, i - 1, j);
        connect1(grid, i, j - 1);
    }
```

时间复杂度 : O(m×n)，其中 m 和 n 分别为行数和列数。

空间复杂度 : 最坏情况下为 O(m×n)，此时整个网格均为陆地，深度优先搜索的深度达到 (m×n)。



### BFS

```java
    public int numIslands2(char[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length, n = grid[0].length;
        Queue<int[]> queue = new LinkedList<>();
        boolean[][] visited = new boolean[m][n];
        int[] dx = {1, 0, -1, 0};
        int[] dy = {0, 1, 0, -1};
        int num = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (!visited[i][j] && grid[i][j] == '1') {
                    num++;
                    queue.add(new int[]{i, j});
                    visited[i][j] = true;
                    while (!queue.isEmpty()) {
                        int[] point = queue.poll();
                        for (int k = 0; k < 4; k++) {
                            int x = point[0] + dx[k];
                            int y = point[1] + dy[k];
                            if (x >= 0 && x < m && y >= 0 && y < n && !visited[x][y] && grid[x][y] == '1') {
                                queue.add(new int[]{x, y});
                                //入队时就标记为已访问，若出队时标记造成节点重复入队导致会超时
                                visited[x][y] = true;
                            }
                        }
                    }
                }
            }
        }
        return num;
    }
```


### 并查集

