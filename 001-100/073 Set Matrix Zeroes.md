# 073. Set Matrix Zeroes(M)
[073. 矩阵置零](https://leetcode-cn.com/problems/set-matrix-zeroes/)

## 题目描述(中等)

Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in-place.

Example 1:
```
Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
```
Example 2:
```
Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
```
**Follow up**:

- A straight forward solution using O(mn) space is probably a bad idea.
- A simple improvement uses O(m + n) space, but still not the best solution.
- Could you devise a constant space solution?

## 思路

1. 记录有0的行和列
2. 行首列首作为标记位


## 解决方法


### 记录行列
```java
public void setZeroes(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return;
        }
        int m = matrix.length, n = matrix[0].length;
        boolean[] row = new boolean[m];
        boolean[] col = new boolean[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    row[i] = true;
                    col[j] = true;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            if (row[i]) {
                for (int j = 0; j < n; j++) {
                    matrix[i][j] = 0;
                }
            }
        }
        for (int j = 0; j < n; j++) {
            if (col[j]) {
                for (int i = 0; i < m; i++) {
                    matrix[i][j] = 0;
                }
            }
        }
    }
```
时间复杂度：O(m * n)，其中 m 和 n 分别对应行数和列数。
空间复杂度：O(m + n)。

### 行首列首标记

```java
    public void setZeroes1(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return;
        }
        int m = matrix.length, n = matrix[0].length;
        boolean row = false;
        boolean col = false;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    if (i == 0) {
                        //第一行需要置零
                        row = true;
                    }
                    if (j == 0) {
                        //第一列需要置零
                        col = true;
                    }
                    //第i行第一个元素置零，表示这一行需要全部置零
                    matrix[i][0] = 0;
                    //第j列第一个元素置零，表示这一列需要全部置零
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i < m; i++) {
            if (matrix[i][0] == 0) {
                for (int j = 1; j < n; j++) {
                    matrix[i][j] = 0;
                }
            }
        }
        for (int j = 1; j < n; j++) {
            if (matrix[0][j] == 0) {
                for (int i = 1; i < m; i++) {
                    matrix[i][j] = 0;
                }
            }
        }
        if (row) {
            for (int j = 0; j < n; j++) {
                matrix[0][j] = 0;
            }
        }
        if (col) {
            for (int i = 0; i < m; i++) {
                matrix[i][0] = 0;
            }
        }
    }
```

时间复杂度：O(m * n)，其中 m 和 n 分别对应行数和列数。
空间复杂度：O(1)。


