
# 509. Fibonacci Number(E)

[509. 斐波那契数](https://leetcode-cn.com/problems/fibonacci-number/)

## 题目描述()

斐波那契数，通常用 F(n) 表示，形成的序列称为斐波那契数列。该数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。也就是：

```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
给定 N，计算 F(N)。
```
 
示例 1：
```
输入：2
输出：1
解释：F(2) = F(1) + F(0) = 1 + 0 = 1.
```

示例 2：
```
输入：3
输出：2
解释：F(3) = F(2) + F(1) = 1 + 1 = 2.
```

示例 3：
```
输入：4
输出：3
解释：F(4) = F(3) + F(2) = 2 + 1 = 3.
```

**提示**：
0 ≤ N ≤ 30


## 思路

- 递归
- 迭代
- 动态规划
- 公式
- 矩阵乘法

## 解决方法

### 递归

```java
    public int fib(int N) {
        if (N <= 1) {
            return N;
        }
        return fib(N - 1) + fib(N - 2);
    }
```

### 递归 记忆化

递归法简单，但是存在大量重复计算，可以用记忆化进行优化

```java
    public int fib_1(int N) {
        return fib(N, new Integer[N + 1]);
    }

    public int fib(int N, Integer[] memo) {
        if (N <= 1) {
            return N;
        }
        if (memo[N] != null) {
            return memo[N];
        }
        memo[N] = fib(N - 1, memo) + fib(N - 2, memo);
        return memo[N];
    }
```



### 动态规划

数组自底向上生成

```java
    public int fib1(int N) {
        if (N <= 1) {
            return N;
        }
        int[] F = new int[N + 1];
        F[0] = 0;
        F[1] = 1;
        for (int i = 2; i <= N; i++) {
            F[i] = F[i - 1] + F[i - 2];
        }
        return F[N];
    }
```

### 迭代 动态规划空间优化

要计算的状态只和前两个状态有关，只记录这两个状态，进一步优化空间

```java
    public int fib2(int N) {
        if (N <= 1) {
            return N;
        }
        int fn = 1;
        int prev = 0;
        int tmp;
        while (N-- > 1) {
            tmp = fn;
            fn = fn + prev;
            prev = tmp;
        }
        return fn;
    }
```


### 公式

```java
    public int fib3(int N) {
        double sqrt5 = Math.sqrt(5);
        return (int) ((Math.pow((1 + sqrt5) / 2, N) - Math.pow((1 - sqrt5) / 2, N)) / sqrt5);
    }
```

### 常规矩阵乘法



$$\begin{matrix} \left[ \begin{matrix} 1 & 0\\ 0 & 0 \end{matrix} \right] \left[ \begin{matrix} 1 & 1\\ 1 & 0 \end{matrix} \right] = \left[ \begin{matrix} 1+0 & 1+0\\ 0+0 & 0+0 \end{matrix} \right] = \left[ \begin{matrix} 1 & 1\\ 0 & 0 \end{matrix} \right] \end{matrix}$$

$$\begin{matrix} \left[ \begin{matrix} 1 & 1\\ 0 & 0 \end{matrix} \right] \left[ \begin{matrix} 1 & 1\\ 1 & 0 \end{matrix} \right] = \left[ \begin{matrix} 1+1 & 1+0\\ 0+0 & 0+0 \end{matrix} \right] = \left[ \begin{matrix} 2 & 1\\ 0 & 0 \end{matrix} \right] \end{matrix}$$

$$\begin{matrix} \left[ \begin{matrix} F_n & F_{n-1}\\ 0 & 0 \end{matrix} \right] \left[ \begin{matrix} 1 & 1\\ 1 & 0 \end{matrix} \right] = \left[ \begin{matrix} F_n+F_{n-1} & F_n\\ 0 & 0 \end{matrix} \right] = \left[ \begin{matrix} F_{n+1} & F_n\\ 0 & 0 \end{matrix} \right] \end{matrix}$$

用矩阵的第一行记录两个数，再和 $\left[ \begin{matrix} 1 & 1\\ 1 & 0 \end{matrix} \right]$ 相乘得出下一个矩阵



```java
    public int fib4(int N) {
        if (N <= 1) {
            return N;
        }
        int[][] matrix = {{1, 0}, {0, 0}};
        int[][] func = {{1, 1}, {1, 0}};
        for (int n = 2; n <= N; n++) {
            matrix = multiplyMatrix(matrix, func);
        }
        return matrix[0][0];
    }

    public int[][] multiplyMatrix(int[][] a, int[][] b) {
        int[][] matrix = new int[2][2];
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 2; j++) {
                for (int k = 0; k < 2; k++) {
                    matrix[i][j] += a[i][k] * b[k][j];
                }
            }
        }
        return matrix;
    }
```

### 优化矩阵乘法

上一个方法中，每次都是乘一个相同的矩阵；而同一数字多个相乘即幂运算，可以用二分法优化成快速幂，而矩阵也同样可以使用，先计算$M^{n/2}$

- 矩阵的起始乘积不再是 1，而是单位矩阵$\left[ \begin{matrix} 1 & 0\\ 0 & 1 \end{matrix} \right]$
- 要把矩阵初始为$\left[ \begin{matrix} 1 & 1\\ 1 & 0 \end{matrix} \right]$，即从F(2)开始，才可以使用，矩阵结构改为$\left[ \begin{matrix} F_n & F_{n-1}\\ F_{n-1} & F_{n-2} \end{matrix} \right]$ 。又因为下标 2 才是第一个，所以传入时参数需要减一

```java
    public int fib5(int N) {
        if (N <= 1) {
            return N;
        }
        int[][] matrix = {{1, 1}, {1, 0}};
        int[][] powMatrix = powMatrix(matrix, N - 1);
        return powMatrix[0][0];
    }

    public int[][] powMatrix(int[][] matrix, int n) {
        int[][] powMatrix = {{1, 0}, {0, 1}};
        while (n > 0) {
            if ((n & 1) != 0) {
                powMatrix = multiplyMatrix(matrix, powMatrix);
            }
            n >>>= 1;
            matrix = multiplyMatrix(matrix, matrix);
        }
        return powMatrix;
    }
```

### 查表

```java
    public int fib0(int N) {
        int[] F = {0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89,
            144, 233, 377, 610, 987, 1597, 2584, 4181, 6765,
            10946, 17711, 28657, 46368, 75025, 121393, 196418,
            317811, 514229, 832040};
        return F[N];
    }
```