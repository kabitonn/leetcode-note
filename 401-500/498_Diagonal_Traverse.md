
# 498. Diagonal Traverse(M)

[498. 对角线遍历](https://leetcode-cn.com/problems/diagonal-traverse/)

## 题目描述(中等)

给定一个含有 M x N 个元素的矩阵（M 行，N 列），请以对角线遍历的顺序返回这个矩阵中的所有元素，对角线遍历如下图所示。

示例:
```
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]

输出:  [1,2,4,7,5,3,6,8,9]

解释:
```
![](../assets/leetcode-note/401-500/498-p-1.png)

**说明**:
- 给定矩阵中的元素总数不会超过 100000 。

## 思路

两个方向遍历，碰到边界变换方向

## 解决方法

### 两个方向遍历

先获取访问数字，再根据当前方向确定下次遍历目标，并根据情况是否换向

```java
    public int[] findDiagonalOrder(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return new int[0];
        }
        boolean d = true;//右上
        int m = matrix.length, n = matrix[0].length;
        int[] order = new int[m * n];
        int i = 0, j = 0;
        for (int k = 0; k < m * n; k++) {
            order[k] = matrix[i][j];
            if (d) {
                //右上方向
                if (i - 1 >= 0 && j + 1 < n) {
                    i--;
                    j++;
                } else {
                    d = !d;
                    if (j + 1 < n) {
                        j++;//上方碰壁
                    } else {
                        i++;//右侧碰壁
                    }
                }
            } else {
                //左下方向
                if (i + 1 < m && j - 1 >= 0) {
                    i++;
                    j--;
                } else {
                    d = !d;
                    if (i + 1 < m) {
                        i++;//左侧碰壁
                    } else {
                        j++;//下方碰壁
                    }
                }
            }
        }
        return order;
    }
```

根据当前方向获取数字并向下遍历，若触碰边界依据情况重置访问下标

```java
    public int[] findDiagonalOrder2(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return new int[0];
        }
        int m = matrix.length, n = matrix[0].length;
        int[] order = new int[m * n];
        int i = 0, j = 0, k = 0;
        while (k < m * n) {
            while (i >= 0 && j < n) {
                order[k++] = matrix[i--][j++];
            }
            if (j < n) {
                i = 0;//上方碰壁
            } else {
                i += 2;//右侧碰壁
                j = n - 1;
            }
            while (i < m && j >= 0) {
                order[k++] = matrix[i++][j--];
            }
            if (i < m) {
                j = 0;//左侧碰壁
            } else {
                j += 2;//下方碰壁
                i = m - 1;
            }
        }
        return order;
    }
```

### 根据循环趟数确定方向

`lines = m + n + 2`为对角线的个数，根据顺序确定遍历方向

对角线上的下标规律： `i+j=line`

```java
    public int[] findDiagonalOrder1(int[][] matrix) {
        if (matrix.length == 0 || matrix[0].length == 0) {
            return new int[0];
        }
        int m = matrix.length, n = matrix[0].length;
        int[] order = new int[m * n];
        int lines = m + n + 2;
        int i = 0, j = 0, k = 0;
        for (int line = 0; line < lines; line++) {
            if (line % 2 == 0) {
                i = i < m ? i : m - 1;
                j = line - i;
                while (i >= 0 && j < n) {
                    order[k++] = matrix[i--][j++];
                }
            } else {
                j = j < n ? j : n - 1;
                i = line - j;
                while (i < m && j >= 0) {
                    order[k++] = matrix[i++][j--];
                }
            }
        }
        return order;
    }
```