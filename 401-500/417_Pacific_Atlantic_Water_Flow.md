
# 417. Pacific Atlantic Water Flow(M)

[417. 太平洋大西洋水流问题](https://leetcode-cn.com/problems/pacific-atlantic-water-flow/)

## 题目描述(中等)

给定一个 `m x n` 的非负整数矩阵来表示一片大陆上各个单元格的高度。“太平洋”处于大陆的左边界和上边界，而“大西洋”处于大陆的右边界和下边界。

规定水流只能按照上、下、左、右四个方向流动，且只能从高到低或者在同等高度上流动。

请找出那些水流既可以流动到“太平洋”，又能流动到“大西洋”的陆地单元的坐标。

**提示：**
- 输出坐标的顺序不重要
- m 和 n 都小于150 

示例：
```
给定下面的 5x5 矩阵:

  太平洋 ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * 大西洋

返回:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (上图中带括号的单元).
```


## 思路

连通性问题
- DFS
- BFS

## 解决方法

### DFS

```java
  public List<List<Integer>> pacificAtlantic(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return new ArrayList<>();
        }
        int m = matrix.length, n = matrix[0].length;
        List<List<Integer>> listList = new ArrayList<>();
        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];
        for (int j = 0; j < n; j++) {
            dfs(matrix, m, n, 0, j, pacific);
            dfs(matrix, m, n, m - 1, j, atlantic);

        }
        for (int i = 0; i < m; i++) {
            dfs(matrix, m, n, i, 0, pacific);
            dfs(matrix, m, n, i, n - 1, atlantic);
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] && atlantic[i][j]) {
                    listList.add(Arrays.asList(i, j));
                }
            }
        }
        return listList;
    }

    public void dfs(int[][] matrix, int m, int n, int i, int j, boolean[][] visited) {
        if (visited[i][j]) {
            return;
        }
        visited[i][j] = true;
        if (i + 1 < m && matrix[i + 1][j] >= matrix[i][j]) {
            dfs(matrix, m, n, i + 1, j, visited);
        }
        if (i - 1 >= 0 && matrix[i - 1][j] >= matrix[i][j]) {
            dfs(matrix, m, n, i - 1, j, visited);
        }
        if (j + 1 < n && matrix[i][j + 1] >= matrix[i][j]) {
            dfs(matrix, m, n, i, j + 1, visited);
        }
        if (j - 1 >= 0 && matrix[i][j - 1] >= matrix[i][j]) {
            dfs(matrix, m, n, i, j - 1, visited);
        }
    }
```
### BFS

```java
    public List<List<Integer>> pacificAtlantic1(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return new ArrayList<>();
        }
        int m = matrix.length, n = matrix[0].length;
        List<List<Integer>> listList = new ArrayList<>();
        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];
        Queue<int[]> queuePacific = new LinkedList<>();
        Queue<int[]> queueAtlantic = new LinkedList<>();

        for (int j = 0; j < n; j++) {
            queuePacific.add(new int[]{0, j});
            pacific[0][j] = true;
            queueAtlantic.add(new int[]{m - 1, j});
            atlantic[m - 1][j] = true;
        }
        for (int i = 1; i < m; i++) {
            queuePacific.add(new int[]{i, 0});
            pacific[i][0] = true;
            queueAtlantic.add(new int[]{i - 1, n - 1});
            atlantic[i - 1][n - 1] = true;
        }
        bfs(matrix, m, n, queuePacific, pacific);
        bfs(matrix, m, n, queueAtlantic, atlantic);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (pacific[i][j] && atlantic[i][j]) {
                    listList.add(Arrays.asList(i, j));
                }
            }
        }
        return listList;
    }

    public void bfs(int[][] matrix, int m, int n, Queue<int[]> queue, boolean[][] visited) {
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int k = 0; k < size; k++) {
                int[] pos = queue.poll();
                int i = pos[0], j = pos[1];
                if (i + 1 < m && !visited[i + 1][j] && matrix[i + 1][j] >= matrix[i][j]) {
                    queue.add(new int[]{i + 1, j});
                    visited[i + 1][j] = true;
                }
                if (i - 1 >= 0 && !visited[i - 1][j] && matrix[i - 1][j] >= matrix[i][j]) {
                    queue.add(new int[]{i - 1, j});
                    visited[i - 1][j] = true;
                }
                if (j + 1 < n && !visited[i][j + 1] && matrix[i][j + 1] >= matrix[i][j]) {
                    queue.add(new int[]{i, j + 1});
                    visited[i][j + 1] = true;
                }
                if (j - 1 >= 0 && !visited[i][j - 1] && matrix[i][j - 1] >= matrix[i][j]) {
                    queue.add(new int[]{i, j - 1});
                    visited[i][j - 1] = true;
                }
            }
        }
    }
```

### BFS 位运算

|=1 表示和太平洋连通
|=2 表示和大西洋连通



```java
    public List<List<Integer>> pacificAtlantic2(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return new ArrayList<>();
        }
        int m = matrix.length, n = matrix[0].length;
        List<List<Integer>> listList = new ArrayList<>();
        int[][] connection = new int[m][n];
        Queue<int[]> queue = new LinkedList<>();
        for (int j = 0; j < n; j++) {
            queue.add(new int[]{0, j});
            connection[0][j] |= 1;
            queue.add(new int[]{m - 1, j});
            connection[m - 1][j] |= 2;
        }
        for (int i = 1; i < m; i++) {
            queue.add(new int[]{i, 0});
            connection[i][0] |= 1;
            queue.add(new int[]{i - 1, n - 1});
            connection[i - 1][n - 1] |= 2;
        }
        bfs2(matrix, m, n, queue, connection);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (connection[i][j] == 3) {
                    listList.add(Arrays.asList(i, j));
                }
            }
        }

        return listList;
    }

    public void bfs2(int[][] matrix, int m, int n, Queue<int[]> queue, int[][] connection) {
        while (!queue.isEmpty()) {
            int[] p = queue.poll();
            int i = p[0], j = p[1];
            if (i + 1 < m && connection[i + 1][j] != connection[i][j] && matrix[i + 1][j] >= matrix[i][j]) {
                queue.add(new int[]{i + 1, j});
                connection[i + 1][j] |= connection[i][j];
            }
            if (i - 1 >= 0 && connection[i - 1][j] != connection[i][j] && matrix[i - 1][j] >= matrix[i][j]) {
                queue.add(new int[]{i - 1, j});
                connection[i - 1][j] |= connection[i][j];
            }
            if (j + 1 < n && connection[i][j + 1] != connection[i][j] && matrix[i][j + 1] >= matrix[i][j]) {
                queue.add(new int[]{i, j + 1});
                connection[i][j + 1] |= connection[i][j];
            }
            if (j - 1 >= 0 && connection[i][j - 1] != connection[i][j] && matrix[i][j - 1] >= matrix[i][j]) {
                queue.add(new int[]{i, j - 1});
                connection[i][j - 1] |= connection[i][j];
            }

        }
    }
```