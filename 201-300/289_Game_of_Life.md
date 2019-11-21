# 289. Game of Life(M)

[](https://leetcode-cn.com/problems/game-of-life/)

## 题目描述(中等)

根据百度百科，生命游戏，简称为生命，是英国数学家约翰·何顿·康威在1970年发明的细胞自动机。

给定一个包含 m × n 个格子的面板，每一个格子都可以看成是一个细胞。每个细胞具有一个初始状态 live（1）即为活细胞， 或 dead（0）即为死细胞。每个细胞与其八个相邻位置（水平，垂直，对角线）的细胞都遵循以下四条生存定律：
> 
1. 如果活细胞周围八个位置的活细胞数少于两个，则该位置活细胞死亡；
2. 如果活细胞周围八个位置有两个或三个活细胞，则该位置活细胞仍然存活；
3. 如果活细胞周围八个位置有超过三个活细胞，则该位置活细胞死亡；
4. 如果死细胞周围正好有三个活细胞，则该位置死细胞复活；

根据当前状态，写一个函数来计算面板上细胞的下一个（一次更新后的）状态。下一个状态是通过将上述规则同时应用于当前状态下的每个细胞所形成的，其中细胞的出生和死亡是同时发生的。

示例:
```
输入: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
输出: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```
**进阶:**

- 你可以使用原地算法解决本题吗？请注意，面板上所有格子需要同时被更新：你不能先更新某些格子，然后使用它们的更新后的值再更新其他格子。
- 本题中，我们使用二维数组来表示面板。原则上，面板是无限的，但当活细胞侵占了面板边界时会造成问题。你将如何解决这些问题？



## 思路


## 解决方法

### 原地算法

依次判断当前位置细胞下一轮是否继续存活,统计当前细胞周围活细胞数目

board[i][j] 含义：
- 1     当前轮存活，下一轮仍然存活
- -1    当前轮存活，下一轮死亡
- 2     下一轮存活
- 0     当前轮死亡

```java
    public void gameOfLife(int[][] board) {
        if (board.length == 0 || board[0].length == 0) {
            return;
        }
        int[] dx = {0, 1, 0, -1, 1, 1, -1, -1};
        int[] dy = {1, 0, -1, 0, 1, -1, 1, -1};
        int m = board.length, n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 1) {
                    if (!isAlive(board, dx, dy, m, n, i, j)) {
                        board[i][j] = -1;
                    }
                } else {
                    if (isAlive(board, dx, dy, m, n, i, j)) {
                        board[i][j] = 2;
                    }
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] > 1) {
                    board[i][j] = 1;
                } else if (board[i][j] < 0) {
                    board[i][j] = 0;
                }
            }
        }
    }

    public boolean isAlive(int[][] board, int[] dx, int[] dy, int m, int n, int i, int j) {
        int count = 0;
        for (int k = 0; k < dx.length; k++) {
            int x = dx[k] + i;
            int y = dy[k] + j;
            if (x < m && x >= 0 && y < n && y >= 0 && (board[x][y] == 1 || board[x][y] == -1)) {
                count++;
            }
        }
        if (board[i][j] == 1) {
            return count == 2 || count == 3;
        } else {
            return count == 3;
        }
    }

```

时间复杂度：O(m*n)
空间复杂度：O(1)

### 原地算法多余位存储状态

利用最低位保存当前的细胞状态（0或1），利用高位保存四周细胞状态的和

x&1就能得到当前状态，x>>1就能得到四周细胞状态的和

```java
    public void gameOfLife2(int[][] board) {
        if (board.length == 0 || board[0].length == 0) {
            return;
        }
        int[] dx = {0, 1, 0, -1, 1, 1, -1, -1};
        int[] dy = {1, 0, -1, 0, 1, -1, 1, -1};
        int m = board.length, n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < dx.length; k++) {
                    int x = dx[k] + i;
                    int y = dy[k] + j;
                    if (x < m && x >= 0 && y < n && y >= 0) {
                        board[i][j] += (board[x][y] & 1) << 1;
                    }
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int next = board[i][j] >> 1;
                if (next < 2 || next > 3) {
                    board[i][j] = 0;
                } else if (next == 3) {
                    board[i][j] = 1;
                } else {
                    board[i][j] &= 1;
                }
            }
        }
    }

```
时间复杂度：O(m*n)
空间复杂度：O(1)

### 辅助空间

利用辅助空间依次判断下一轮细胞存活状态

```java
    public void gameOfLife1(int[][] board) {
        if (board.length == 0 || board[0].length == 0) {
            return;
        }
        int m = board.length, n = board[0].length;
        int[][] newBoard = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                newBoard[i][j] = isAlive(board, m, n, i, j) ? 1 : 0;
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = newBoard[i][j];
            }
        }
    }

    public boolean isAlive(int[][] board, int m, int n, int i, int j) {
        int count = 0;
        for (int x = i - 1; x <= i + 1; x++) {
            for (int y = j - 1; y <= j + 1; y++) {
                if ((x == i && y == j) || (x < 0 || x >= m || y < 0 || y >= n)) {
                    continue;
                }
                if ((board[x][y] == 1 || board[x][y] == -1)) {
                    count++;
                }
            }
        }
        if (board[i][j] == 1) {
            return count == 2 || count == 3;
        } else {
            return count == 3;
        }
    }

```
时间复杂度：O(m*n)
空间复杂度：O(m*n)