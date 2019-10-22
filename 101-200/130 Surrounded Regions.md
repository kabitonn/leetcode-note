# 130. Surrounded Regions(M)


[130. 被围绕的区域](https://leetcode-cn.com/problems/surrounded-regions/)


## 题目描述(中等)

Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

Example:
```
X X X X
X O O X
X X O X
X O X X
After running your function, the board should be:

X X X X
X X X X
X X X X
X O X X
Explanation:

Surrounded regions shouldn’t be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.
```

## 思路

- 当前节点DFS
- 边界节点开始DFS
- 并查集

## 解决方法



### DFS 递归 记忆化

相邻的'O'为连通图，从每一个'O'开始做DFS，

如果遍历完成后没有到达边界的 O ，我们就把当前 O 改成 X。

如果遍历过程中到达了边界的 O ，直接结束 DFS，当前的 O 就不用操作。

然后继续考虑下一个 O，继续做一次 DFS。

在DFS过程中记录能达到边界的'O'的位置，在每次DFS过程中若确定是连通边界的可以快速返回。

```java
    public void solve(char[][] board) {
        if (board.length == 0 || board[0].length == 0) {
            return;
        }
        int m = board.length, n = board[0].length;
        Set<String> connection = new HashSet<>();
        for (int i = 1; i < m - 1; i++) {
            for (int j = 1; j < n - 1; j++) {
                if (board[i][j] == 'X') {
                    continue;
                }
                if (connection.contains(i + "@" + j)) {
                    continue;
                }
                Set<String> visited = new HashSet<>();
                if (!connect(board, i, j, visited, connection)) {
                    board[i][j] = 'X';
                }
            }
        }
    }

    private boolean connect(char[][] board, int i, int j, Set<String> visited, Set<String> connection) {
        if (visited.contains(i + "@" + j)) {
            return false;
        }
        visited.add(i + "@" + j);
        if (board[i][j] == 'X') {
            return false;
        }
        if (connection.contains(i + "@" + j)) {
            return true;
        }
        if (i == 0 || i == board.length - 1 || j == 0 || j == board[0].length - 1) {
            connection.add(i + "@" + j);
            return true;
        }
        if (connect(board, i + 1, j, visited, connection) ||
            connect(board, i, j + 1, visited, connection) ||
            connect(board, i - 1, j, visited, connection) ||
            connect(board, i, j - 1, visited, connection)) {
            connection.add(i + "@" + j);
            return true;
        } else {
            return false;
        }
    }
```

结果超时


### 边界节点DFS

从边界的 O 做 DFS，然后把遇到的 O 都标记一下，这些 O 就是可以连通到边界的。然后把边界的所有的 O 都做一次 DFS ，把 DFS 过程的中的 O 做一下标记。最后我们只需要遍历节点，把没有标记过的 O 改成 X 就可以了。

```java
    public void solve1(char[][] board) {
        if (board.length == 0 || board[0].length == 0) {
            return;
        }
        int m = board.length, n = board[0].length;
        boolean[][] visited = new boolean[m][n];
        for (int j = 0; j < n; j++) {
            if (board[0][j] == 'O') {
                connect1(board, 0, j, visited);
            }
            if (board[m - 1][j] == 'O') {
                connect1(board, m - 1, j, visited);
            }
        }
        for (int i = 0; i < m; i++) {
            if (board[i][0] == 'O') {
                connect1(board, i, 0, visited);
            }
            if (board[i][n - 1] == 'O') {
                connect1(board, i, n - 1, visited);
            }
        }
        for (int i = 1; i < m - 1; i++) {
            for (int j = 1; j < n - 1; j++) {
                if (!visited[i][j]) {
                    board[i][j] = 'X';
                }
            }
        }

    }

    private void connect1(char[][] board, int i, int j, boolean[][] visited) {
        if (i < 0 || i == board.length || j < 0 || j == board[0].length) {
            return;
        }
        if (visited[i][j]) {
            return;
        }
        if (board[i][j] == 'O') {
            visited[i][j] = true;
            connect1(board, i + 1, j, visited);
            connect1(board, i, j + 1, visited);
            connect1(board, i - 1, j, visited);
            connect1(board, i, j - 1, visited);
        }
    }

```

### 边界节点DFS 空间优化

从边界的 O 做 DFS，然后把遇到的 O 都修改为 * ，最后遍历节点，恢复原状

```java
    public void solve2(char[][] board) {
        if (board.length == 0 || board[0].length == 0) {
            return;
        }
        int m = board.length, n = board[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // 从边缘o开始搜索
                boolean isEdge = i == 0 || j == 0 || i == m - 1 || j == n - 1;
                if (isEdge && board[i][j] == 'O') {
                    connect2(board, i, j);
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == '*') {
                    board[i][j] = 'O';
                } else {
                    board[i][j] = 'X';
                }
            }
        }
    }

    private void connect2(char[][] board, int i, int j) {
        if (i < 0 || i == board.length || j < 0 || j == board[0].length) {
            return;
        }
        if (board[i][j] == '*') {
            return;
        }
        if (board[i][j] == 'O') {
            board[i][j] = '*';
            connect2(board, i + 1, j);
            connect2(board, i, j + 1);
            connect2(board, i - 1, j);
            connect2(board, i, j - 1);
        }
    }
```

### 并查集


//TODO