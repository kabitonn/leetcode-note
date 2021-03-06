# 062. Unique Paths\(M\)

[062. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

## 题目描述\(中等\)

A robot is located at the top-left corner of a m x n grid \(marked 'Start' in the diagram below\).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid \(marked 'Finish' in the diagram below\).

How many possible unique paths are there?

![](../assets/leetcode-note/001-100/061-p-1.png)

Above is a 7 x 3 grid. How many possible unique paths are there?

Note: m and n will be at most 100.

Example 1:

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right
```

Example 2:

```
Input: m = 7, n = 3
Output: 28
```

## 思路

1. 组合数
2. 递归
3. 动态规划

## 解决方法

### 组合数计算公式

向右m-1步，向下n-1步，结果为C\(m+n-2,m-1\)=C\(m+n-2,n-1\)  
令N=m+n-2,k=m-1  
$$C_N^k = N!/(k!(N-k)!)=(N*(n-1)*(N-2)*...(N-k+1))/k! $$

```java
    public int uniquePaths(int m, int n) {
        if (m == 0 || n == 0) {
            return 0;
        }
        int steps = m + n - 2;
        int minStep = Math.min(m, n) - 1;
        long paths = 1;
        for (int i = 1; i <= minStep; i++) {
            paths = paths * (steps - i + 1) / i;
        }
        return (int) paths;
    }
```

时间复杂度：O\(min\(m, n\)\)。

空间复杂度：O\(1\)。

### 递归
```java
    public int uniquePaths(int m, int n) {
        return uniquePaths(m, n, new int[m + 1][n + 1]);
    }

    public int uniquePaths(int m, int n, int[][] path) {
        if (m == 0 || n == 0) {
            return 0;
        } else if (m == 1 || n == 1) {
            return 1;
        }
        if (path[m][n] > 0) {
            return path[m][n];
        }
        path[m][n] = uniquePaths(m - 1, n, path) + uniquePaths(m, n - 1, path);
        return path[m][n];
    }
```

### 动态规划

dp\[ i \]\[ j \] 是到达 i, j 最多路径

dp\[ i \]\[ j \] = dp\[ i - 1 \]\[ j \] + dp\[ i \]\[ j - 1 \]

```java
    public int uniquePaths1(int m, int n) {
        if (m == 0 || n == 0) {
            return 0;
        }
        int[][] paths = new int[m][n];
        for (int i = 0; i < m; i++) {
            paths[i][0] = 1;
        }
        for (int i = 0; i < n; i++) {
            paths[0][i] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                paths[i][j] = paths[i - 1][j] + paths[i][j - 1];
            }
        }
        return paths[m - 1][n - 1];
    }
```

时间复杂度：O\(m \* n\)。

空间复杂度：O\(m \* n\)。

空间优化

![](../assets/leetcode-note/001-100/062-s-3-2.png)

```java
public int uniquePaths2(int m, int n) {
        if (m == 0 || n == 0) {
            return 0;
        }
        int[] paths = new int[n];
        Arrays.fill(paths, 1);
        //从上向下更新所有行
        for (int i = 1; i < m; i++) {
            //从左向右更新所有列
            for (int j = 1; j < n; j++) {
                //path[j]为上,path[j-1]为左
                paths[j] = paths[j] + paths[j - 1];
            }
        }
        return paths[n - 1];
    }
```

时间复杂度：O\(m \* n\)。

空间复杂度：O\(n\)。

另一种思路

dp\[ i \]\[ j \] = dp\[ i + 1 \]\[ j \] + dp\[ i \]\[ j + 1 \]。

![](../assets/leetcode-note/001-100/062-s-3-3.png)

```java
    public int uniquePaths3(int m, int n) {
        if (m == 0 || n == 0) {
            return 0;
        }
        int[] paths = new int[m];
        Arrays.fill(paths, 1);
        //从右向左更新所有列
        for (int j = n - 2; j >= 0; j--) {
            //从下向上更新所有行
            for (int i = m - 2; i >= 0; i--) {
                //path[i]为右,path[i+1]为下
                paths[i] = paths[i] + paths[i + 1];
            }
        }
        return paths[0];
    }
```

时间复杂度：O\(m \* n\)。

空间复杂度：O\(m\)。

