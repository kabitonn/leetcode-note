# 054. Spiral Matrix\(M\)

[054. Spiral Matrix](https://leetcode-cn.com/problems/spiral-matrix/)

## 题目描述\(中等\)

Given a matrix of m x n elements \(m rows, n columns\), return all elements of the matrix in spiral order.

Example 1:

```
Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]
```

Example 2:

```
Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]
```

## 思路

1. 模拟：四个方向，触碰边界变换方向

## 解决方法

### 模拟

四个方向，触碰边界变换方向

```java
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> list = new ArrayList<>();
        if (matrix.length == 0) {
            return list;
        }
        int m = matrix.length, n = matrix[0].length;
        boolean[][] visited = new boolean[m][n];
        int i = 0, j = 0, d = 0;
        int[] di = {0, 1, 0, -1};
        int[] dj = {1, 0, -1, 0};
        for (int k = 0; k < m * n; k++) {
            list.add(matrix[i][j]);
            visited[i][j] = true;
            int iTemp = i + di[d];
            int jTemp = j + dj[d];
            if (0 <= iTemp && iTemp < m && 0 <= jTemp && jTemp < n && !visited[iTemp][jTemp]) {
                i = iTemp;
                j = jTemp;
            } else {
                d = (d + 1) % 4;
                i = i + di[d];
                j = j + dj[d];
            }
        }
        return list;
    }
```

时间复杂度：O\(m \* n\)，m 和 n 是数组的长宽。

空间复杂度：O\(1\)。

### 按层模拟

依此四个方向为一圈遍历

![](/assets/001-100/054-s-2-1.png)

```java
    public List<Integer> spiralOrder1(int[][] matrix) {
        List<Integer> list = new ArrayList<>();
        if (matrix.length == 0) {
            return list;
        }
        int r1 = 0, r2 = matrix.length - 1;
        int c1 = 0, c2 = matrix[0].length - 1;

        while (r1 <= r2 && c1 <= c2) {
            for (int c = c1; c < c2; c++) {
                list.add(matrix[r1][c]);
            }
            for (int r = r1; r <= r2; r++) {
                list.add(matrix[r][c2]);
            }
            if (r1 < r2 && c1 < c2) {
                for (int c = c2 - 1; c > c1; c--) {
                    list.add(matrix[r2][c]);
                }
                for (int r = r2; r > r1; r--) {
                    list.add(matrix[r][c1]);
                }
            }
            r1++;
            r2--;
            c1++;
            c2--;
        }
        return list;
    }
```



