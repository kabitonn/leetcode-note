# 062. Unique Paths\(M\)

[062. 不同路径](https://leetcode-cn.com/problems/unique-paths/)

## 题目描述\(中等\)

A robot is located at the top-left corner of a m x n grid \(marked 'Start' in the diagram below\).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid \(marked 'Finish' in the diagram below\).

How many possible unique paths are there?

![](/assets/001-100/061-p-1.png)

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
2. 

## 解决方法

### 组合数计算公式

向右m-1步，向下n-1步，结果为C(m+n-2,m-1)=C(m+n-2,n-1)
令N=m+n-2,k=m-1
$$C_N^k = N!/(k!(N−k)!)=(N∗(n−1)∗(N−2)∗...(N−k+1))/k! $$

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

时间复杂度：O(min(m, n))。

空间复杂度：O(1)。

### 动态规划


