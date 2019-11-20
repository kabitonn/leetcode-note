# 306. Additive Number(M)

[306. 累加数](https://leetcode-cn.com/problems/additive-number/)

## 题目描述(中等)

累加数是一个字符串，组成它的数字可以形成累加序列。

一个有效的累加序列必须**至少**包含 3 个数。除了最开始的两个数以外，字符串中的其他数都等于它之前两个数相加的和。

给定一个只包含数字 '0'-'9' 的字符串，编写一个算法来判断给定输入是否是累加数。

说明: 累加序列里的数不会以 0 开头，所以不会出现 1, 2, 03 或者 1, 02, 3 的情况。

示例 1:
```
输入: "112358"
输出: true 
解释: 累加序列为: 1, 1, 2, 3, 5, 8 。1 + 1 = 2, 1 + 2 = 3, 2 + 3 = 5, 3 + 5 = 8
```
示例 2:
```
输入: "199100199"
输出: true 
解释: 累加序列为: 1, 99, 100, 199。1 + 99 = 100, 99 + 100 = 199
```
**进阶**:
你如何处理一个溢出的过大的整数输入?


## 思路

- 递归
- 迭代

## 解决方法

### 递归

假设已知前两个数，然后进行验证累加数的操作

遍历所有可能情况下的前两个数，进行DFS验证

- num1: [s1,s2)
- num2: [s2,s3)
- num3: [s3,?)

```java
    public boolean isAdditiveNumber(String num) {
        if (num.length() < 3) {
            return false;
        }
        int n = num.length();
        for (int i = 1; i <= n / 2; i++) {
            for (int j = i + 1; j + (j - i) <= n; j++) {
                if (isAdditiveNumber(num, 0, i, j)) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean isAdditiveNumber(String num, int s1, int s2, int s3) {
        if (s3 == num.length()) {
            return true;
        }
        if ((num.charAt(s1) == '0' && s2 - s1 > 1)) {
            return false;
        }
        if (num.charAt(s2) == '0' && s3 - s2 > 1) {
            return false;
        }
        long num1 = Long.valueOf(num.substring(s1, s2));
        long num2 = Long.valueOf(num.substring(s2, s3));
        String sum = String.valueOf(num1 + num2);
        if (num.indexOf(sum, s3) != s3) {
            return false;
        }
        return isAdditiveNumber(num, s2, s3, s3 + sum.length());
    }
```
### 迭代

假设已知前两个数，然后进行验证累加数的操作

遍历所有可能情况下的前两个数，向后遍历进行验证

```java
    public boolean isAdditiveNumber(String num) {
        if (num.length() < 3) {
            return false;
        }
        int n = num.length();
        for (int i = 1; i <= n / 2; i++) {
            for (int j = i + 1; j + (j - i) <= n; j++) {
                if (isAdditiveNumber(num, 0, i, j)) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean isAdditiveNumber(String num, int s1, int s2, int s3) {
        while (s3 != num.length()) {
            if ((num.charAt(s1) == '0' && s2 - s1 > 1)) {
                return false;
            }
            if (num.charAt(s2) == '0' && s3 - s2 > 1) {
                return false;
            }
            long num1 = Long.valueOf(num.substring(s1, s2));
            long num2 = Long.valueOf(num.substring(s2, s3));
            String sum = String.valueOf(num1 + num2);
            if (num.indexOf(sum, s3) != s3) {
                return false;
            }
            int next = s3 + sum.length();
            s1 = s2;
            s2 = s3;
            s3 = next;
        }
        return true;
    }
```