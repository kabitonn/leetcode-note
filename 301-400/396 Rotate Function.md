# 396. Rotate Function(M)

[396. 旋转函数](https://leetcode-cn.com/problems/rotate-function/)

## 题目描述(中等)

给定一个长度为 n 的整数数组 A 。

假设 Bk 是数组 A 顺时针旋转 k 个位置后的数组，我们定义 A 的“旋转函数” F 为：

`F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1]`。

计算`F(0), F(1), ..., F(n-1)`中的最大值。

**注意**:
可以认为 n 的值小于 10^5。

示例:
```
A = [4, 3, 2, 6]

F(0) = (0 * 4) + (1 * 3) + (2 * 2) + (3 * 6) = 0 + 3 + 4 + 18 = 25
F(1) = (0 * 6) + (1 * 4) + (2 * 3) + (3 * 2) = 0 + 4 + 6 + 6 = 16
F(2) = (0 * 2) + (1 * 6) + (2 * 4) + (3 * 3) = 0 + 6 + 8 + 9 = 23
F(3) = (0 * 3) + (1 * 2) + (2 * 6) + (3 * 4) = 0 + 2 + 12 + 12 = 26

所以 F(0), F(1), F(2), F(3) 中的最大值是 F(3) = 26 。
```


## 思路

- 题意模拟过程
- 规律 公式

## 解决方法

### 模拟计算过程

`F(i) = sum(j*A[(j+i)]%n) for j in [0,n)`

```java
    public int maxRotateFunction(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        int n = A.length;
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < n; i++) {
            int f = 0;
            for (int j = 0; j < n; j++) {
                f += j * A[(j + i) % n];
            }
            max = Math.max(max, f);
        }
        return max;
    }
```
### 规律

`F(i)=F(i-1) + sum - n * A[n-i]`

```java
    public int maxRotateFunction1(int[] A) {
        if (A == null || A.length == 0) {
            return 0;
        }
        int n = A.length;
        int f = 0;
        int sum = 0;
        for (int i = 0; i < n; i++) {
            f += i * A[i];
            sum += A[i];
        }
        int max = f;
        //数组循环右移1次，
        for (int i = 1; i < n; i++) {
            f = f + sum - (n) * A[n - i];
            max = Math.max(max, f);
        }
        return max;
    }
```
