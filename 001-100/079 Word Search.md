# 079. Word Search(M)
[079. 单词搜索](https://leetcode-cn.com/problems/word-search/)


## 题目描述(中等)

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

Example:
```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

## 思路

DFS

## 解决方法


### DFS

```
    public boolean exist0(char[][] board, String word) {
        if (board.length == 0) {
            return false;
        }
        if (word.length() == 0) {
            return true;
        }
        int m = board.length, n = board[0].length;

        byte[][] visited = new byte[m][n];

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (existStart0(board, visited, word, i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean existStart0(char[][] board, byte[][] visited, String word, int i, int j, int index) {
        int m = board.length, n = board[0].length;
        if (i < 0 || i >= m || j < 0 || j >= n || visited[i][j] == 1 || board[i][j] != word.charAt(index)) {
            return false;
        }
        if (index == word.length() - 1) {
            return true;
        }
        visited[i][j] = 1;
        boolean found = existStart0(board, visited, word, i + 1, j, index + 1)
            || existStart0(board, visited, word, i - 1, j, index + 1)
            || existStart0(board, visited, word, i, j + 1, index + 1)
            || existStart0(board, visited, word, i, j - 1, index + 1);
        visited[i][j] = 0;
        return found;
    }

```


### 回溯 标记 