# 201. Bitwise AND of Numbers Range(M)

[201. 数字范围按位与](https://leetcode-cn.com/problems/bitwise-and-of-numbers-range/)

## 题目描述(中等)

Given a range [m, n] where 0 <= m <= n <= 2147483647, return the bitwise AND of all numbers in this range, inclusive.

Example 1:
```
Input: [5,7]
Output: 4
```
Example 2:
```
Input: [0,1]
Output: 0
```

## 思路

- 暴力遍历


## 解决方法

### 暴力遍历

```java
    public int rangeBitwiseAnd(int m, int n) {
        int and = Integer.MAX_VALUE;
        for (int i = m; i <= n && i >= 0 && and != 0; i++) {
            and &= i;
        }
        return and;
    }

```

### 统计移位次数

获取最高位相同的部分即为结果
两数同时右移，直到两数相等，忽略右侧不等的部分，因为右侧不等部分的连续值相与必然等于0

```java
    public int rangeBitwiseAnd2(int m, int n) {
        // 统计移位次数
        int count = 0;
        while (m != n) {
            m >>= 1;
            n >>= 1;
            count++;
        }
        n <<= count;
        return n;
    }
```

