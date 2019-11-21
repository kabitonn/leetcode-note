
# 413. Arithmetic Slices(M)

[413. 等差数列划分](https://leetcode-cn.com/problems/arithmetic-slices/)

## 题目描述(中等)

如果一个数列至少有三个元素，并且任意两个相邻元素之差相同，则称该数列为等差数列。

例如，以下数列为等差数列:
```
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
```
以下数列不是等差数列。
```
1, 1, 2, 5, 7
```

数组 A 包含 N 个数，且索引从0开始。数组 A 的一个子数组划分为数组 `(P, Q)`，P 与 Q 是整数且满足 `0<=P<Q<N` 。

如果满足以下条件，则称子数组(P, Q)为等差数组：

元素 `A[P], A[p + 1], ..., A[Q - 1], A[Q]` 是等差的。并且 `P + 1 < Q` 。

函数要返回数组 A 中所有为等差数组的子数组个数。

示例:
```
A = [1, 2, 3, 4]

返回: 3, A 中有三个子等差数组: [1, 2, 3], [2, 3, 4] 以及自身 [1, 2, 3, 4]。
```
## 思路

- 暴力遍历
- 动态规划

## 解决方法

### 暴力遍历

遍历[i,j]是否为等差数列

```java
    public int numberOfArithmeticSlices0(int[] A) {
        int n = A.length;
        int count = 0;
        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 2; j < n; j++) {
                int d = A[i + 1] - A[i];
                if (A[j] - A[j - 1] == d) {
                    count++;
                } else {
                    break;
                }
            }
        }
        return count;
    }
```
时间复杂度：O(n^2)
空间复杂度：O(1)

```java
    public int numberOfArithmeticSlices(int[] A) {
        int n = A.length;
        boolean[][] dp = new boolean[n][n];
        int count = 0;
        for (int i = 0; i < n - 1; i++) {
            dp[i][i + 1] = true;
        }
        for (int len = 3; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1;
                if (dp[i][j - 1] && A[j] - A[j - 1] == A[j - 1] - A[j - 2]) {
                    count++;
                    dp[i][j] = true;
                }
            }
        }
        return count;
    }
```

时间复杂度：O(n^2)
空间复杂度：O(n^2)

### 滑动窗口

- 若[i,j]是等差数列，则可以出现新的等差数列数目为 `j-i+1-2=j-i-1`
- 若[i,j]不是等差数列，i可以继续从j-1处向后判断

```java
    public int numberOfArithmeticSlices1(int[] A) {
        int n = A.length;
        if (n < 3) {
            return 0;
        }
        int count = 0;
        int i = 0, j = 2;
        while (j < n) {
            if (A[j] - A[j - 1] == A[j - 1] - A[j - 2]) {
                count += j - i - 1;
            } else {
                i = j - 1;
            }
            j++;
        }
        return count;
    }

```

### 动态规划

dp[i]: 以i为结尾的等差数列的个数

dp[i] = dp[i-1] + 1 若A[i] 和之前两个数字构成等差数列

```java
    public int numberOfArithmeticSlices2(int[] A) {
        int n = A.length;
        if (n < 3) {
            return 0;
        }
        int[] dp = new int[n];
        int count = 0;
        for (int i = 2; i < n; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                dp[i] = dp[i - 1] + 1;
                count += dp[i];
            }
        }
        return count;
    }
```

动态规划 空间优化

```java
    public int numberOfArithmeticSlices3(int[] A) {
        int n = A.length;
        if (n < 3) {
            return 0;
        }
        int dp = 0;
        int count = 0;
        for (int i = 2; i < n; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                dp = dp + 1;
                count += dp;
            } else {
                dp = 0;
            }
        }
        return count;
    }
```
### 求一段等差数列最大长度

若一段等差数列长度为 count + 2, 可以构造出不同的等差数列数目为 (count+1) * count / 2

```java
    public int numberOfArithmeticSlices4(int[] A) {
        int count = 0;
        int sum = 0;
        for (int i = 2; i < A.length; i++) {
            if (A[i] - A[i - 1] == A[i - 1] - A[i - 2]) {
                count++;
            } else {
                sum += (count + 1) * (count) / 2;
                count = 0;
            }
        }
        sum += count * (count + 1) / 2;
        return sum;
    }

```

