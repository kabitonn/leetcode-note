# 463. Island Perimeter(E)


[463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

## 题目描述(中等)

给定一个包含 0 和 1 的二维网格地图，其中 1 表示陆地 0 表示水域。

网格中的格子水平和垂直方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

 

示例 :
```
输入:
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]

输出: 16

解释: 它的周长是下面图片中的 16 个黄色的边：
```

![](../assets/leetcode-note/401-500/463-p-1.png)

## 思路

- DFS
- BFS
- 计算陆地重合边
- 计算周长的一半边

## 解决方法

### DFS

依次判断小岛周围上下左右是否为湖，若是周长 +1，

```java
    public int islandPerimeter(int[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length;
        int n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        int perimeter = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0 || visited[i][j]) {
                    continue;
                }
                perimeter += dfs(grid, i, j, m, n, visited);
            }
        }
        return perimeter;
    }

    public int dfs(int[][] grid, int i, int j, int m, int n, boolean[][] visited) {
        visited[i][j] = true;
        int perimeter = 0;
        if (i - 1 < 0 || grid[i - 1][j] == 0) {
            perimeter++;
        } else if (!visited[i - 1][j]) {
            perimeter += dfs(grid, i - 1, j, m, n, visited);
        }
        if (i + 1 >= m || grid[i + 1][j] == 0) {
            perimeter++;
        } else if (!visited[i + 1][j]) {
            perimeter += dfs(grid, i + 1, j, m, n, visited);
        }
        if (j - 1 < 0 || grid[i][j - 1] == 0) {
            perimeter++;
        } else if (!visited[i][j - 1]) {
            perimeter += dfs(grid, i, j - 1, m, n, visited);
        }
        if (j + 1 >= n || grid[i][j + 1] == 0) {
            perimeter++;
        } else if (!visited[i][j + 1]) {
            perimeter += dfs(grid, i, j + 1, m, n, visited);
        }
        return perimeter;
    }
```

### BFS

```java
    public int islandPerimeter1(int[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length;
        int n = grid[0].length;
        boolean[][] visited = new boolean[m][n];
        int perimeter = 0;
        Queue<int[]> queue = new LinkedList<>();
        int[] dx = {1, 0, -1, 0};
        int[] dy = {0, 1, 0, -1};
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0 || visited[i][j]) {
                    continue;
                }
                queue.add(new int[]{i, j});
                visited[i][j] = true;
                while (!queue.isEmpty()) {
                    int[] point = queue.poll();
                    for (int k = 0; k < 4; k++) {
                        int x = point[0] + dx[k];
                        int y = point[1] + dy[k];
                        if (x < 0 || x >= m || y < 0 || y >= n || grid[x][y] == 0) {
                            perimeter++;
                        } else if (!visited[x][y]) {
                            queue.add(new int[]{x, y});
                            visited[x][y] = true;
                        }
                    }
                }
            }
        }
        return perimeter;
    }
```

### 计算陆地周围重合的边

遍历整个矩阵，每遍历到一个岛，则去看这个岛的上下左右有没有岛。可以发现如果一个岛附近每有一个岛，则这个岛贡献的周长会-1。

简化过程，可以对每个岛只判断一半方向即左(右)和上(下)，重合的边最后 * 2 即可

```java
    public int islandPerimeter2(int[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length;
        int n = grid[0].length;
        int perimeter = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    perimeter += 4;
                    if (i > 0 && grid[i - 1][j] == 1) {
                        perimeter -= 2;
                    }
                    if (j > 0 && grid[i][j - 1] == 1) {
                        perimeter -= 2;
                    }
                }
            }
        }
        return perimeter;
    }
```


### 计算周长一半的边

由于岛屿内没有湖,所以只需要求出 北面(或南面) + 西面(或东面)的长度再乘2即可

```java
    public int islandPerimeter3(int[][] grid) {
        if (grid.length == 0 || grid[0].length == 0) {
            return 0;
        }
        int m = grid.length;
        int n = grid[0].length;
        int perimeter = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    if (i == 0 || grid[i - 1][j] == 0) {
                        perimeter += 2;
                    }
                    if (j == 0 || grid[i][j - 1] == 0) {
                        perimeter += 2;
                    }
                }
            }
        }

        return perimeter;
    }
```