# 050. Pow(x, n)(M)
[050. Pow(x, n)](https://leetcode-cn.com/problems/powx-n/)
## 题目描述(中等)

Implement pow(x, n), which calculates x raised to the power n $$(x^n)$$.

Example 1:
```
Input: 2.00000, 10
Output: 1024.00000
```
Example 2:
```
Input: 2.10000, 3
Output: 9.26100
```
Example 3:
```
Input: 2.00000, -2
Output: 0.25000
Explanation: 2-2 = 1/22 = 1/4 = 0.25
```
**Note**:

-100.0 < x < 100.0
n is a 32-bit signed integer, within the range $$ [−2^{31}, 2^{31} − 1] $$

## 思路
1. 累乘
2. 指数幂相乘
3. 二进制位确定累乘数
4. 递归


## 解决方法


### 暴力 循环累乘

```java
    public double myPow(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double r = 1;
        for (long i = 0; i < N; i++) {
            r = r * x;
        }
        return r;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。

### 指数幂累乘

```java
    public double myPow1(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double r = 1, temp = x;
        long num = 0, times = 1;
        while (num < N) {
            r *= temp;
            num += times;
            times <<= 1;
            temp *= temp;
            if (num + times >= N) {
                temp = x;
                times = 1;
            }

        }
        return r;
    }
```
时间复杂度：log(n)。

空间复杂度：O(1)。

### 二进制位确定累乘数

从右向左依此判断该位是否为1

```java
    public double myPow2(double x, int n) {
        long N = n;
        if (N < 0) {
            x = 1 / x;
            N = -N;
        }
        double r = 1, temp = x;
        while (N != 0) {
            if ((N & 1) == 1) {
                r *= temp;
            }
            temp *= temp;
            N >>= 1;
        }
        return r;
    }
```

时间复杂度：log(n)。

空间复杂度：O(1)。

### 递归

```java
    public double myPow(double x, int n) {
        if (n == 0) {
            return 1;
        } else if (n == 1) {
            return x;
        } else if (n == -1) {
            return 1 / x;
        }
        double half = myPow(x, n / 2);
        double remain = myPow(x, n % 2);
        return remain * half * half;
    }
```

时间复杂度：log(n)。

空间复杂度：