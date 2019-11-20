# 357. Count Numbers with Unique Digits(M)


[357. 计算各个位数不同的数字个数](https://leetcode-cn.com/problems/count-numbers-with-unique-digits/)

## 题目描述(中等)

给定一个**非负**整数 n，计算各位数字都不同的数字 x 的个数，其中 0 ≤ x < 10^n。

示例:
```
输入: 2
输出: 91 
解释: 答案应为除去 11,22,33,44,55,66,77,88,99 外，在 [0,100) 区间内的所有数字。
```

## 思路

排列组合  
从高位依次选取可选数字，构造各位数字都不同的数

## 解决方法

### 按不同位数求解

- n=1,  9
- n=2,  9*9
- n=3,  9*9*8
- n=4,  9*9*8*7
- ...

```java
    public int countNumbersWithUniqueDigits(int n) {
        int sum = 0;
        for (int i = 0; i <= 2 && i <= n; i++) {
            int s = 1;
            for (int j = 0; j < i; j++) {
                s *= 9;
            }
            sum += s;
        }
        for (int i = 3; i <= n; i++) {
            int s = 9 * 9;
            for (int j = 2; j < i; j++) {
                s *= 9 - (i - j);
            }
            sum += s;
        }
        return sum;
    }

```

### 动态规划

dp[i]表示i位数以内符合要求的数的个数

dp[i]=dp[i-1]+mul;mul为i位数符合要求的个数

```java
    public int countNumbersWithUniqueDigits1(int n) {
        n = Math.min(n, 10);
        int[] dp = new int[n != 0 ? n + 1 : 2];
        dp[0] = 1;
        dp[1] = 10;
        int mul = 9;
        for (int i = 2; i <= n; i++) {
            mul *= (10 - i + 1);
            dp[i] = dp[i - 1] + mul;
        }
        return dp[n];
    }
```

```java
    public int countNumbersWithUniqueDigits2(int n) {
        n = Math.min(n, 10);
        int[] counts = {9, 9, 8, 7, 6, 5, 4, 3, 2, 1};
        int count = 1;
        int product = 1;
        for (int i = 0; i < n; i++) {
            product *= counts[i];
            count += product;
        }
        return count;
    }
```