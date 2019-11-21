# 091. Decode Ways(M)
[091. 解码方法](https://leetcode-cn.com/problems/decode-ways/)

## 题目描述(中等)

A message containing letters from A-Z is being encoded to numbers using the following mapping:
```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```
Given a non-empty string containing only digits, determine the total number of ways to decode it.

Example 1:
```
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).
```
Example 2:
```
Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).
```

## 思路

## 解决方法

- 递归

- 动态规划

### 递归


```java
    public int numDecodings1(String s) {
        return numDecoding(s, 0);
    }

    private int numDecoding(String s, int index) {
        if (index == s.length()) {
            return 1;
        }
        if (s.charAt(index) == '0') {
            return 0;
        }
        int sum1 = numDecoding(s, index + 1);
        int sum2 = 0;
        if (index < s.length() - 1) {
            int n2 = (s.charAt(index) - '0') * 10 + s.charAt(index + 1) - '0';
            if (n2 <= 26) {
                sum2 = numDecoding(s, index + 2);
            }
        }
        return sum1 + sum2;
    }
```

### 递归 + 记忆化

```java
    public int numDecodings1_1(String s) {
        Map<Integer, Integer> memoization = new HashMap<>();
        return numDecodings1_1(s, 0, memoization);
    }

    private int numDecodings1_1(String s, int index, Map<Integer, Integer> memoization) {
        if (index == s.length()) {
            return 1;
        }
        if (s.charAt(index) == '0') {
            return 0;
        }
        //判断之前是否计算过
        int m = memoization.getOrDefault(index, -1);
        if (m != -1) {
            return m;
        }
        int sum1 = numDecodings1_1(s, index + 1, memoization);
        int sum2 = 0;
        if (index < s.length() - 1) {
            int n2 = (s.charAt(index) - '0') * 10 + s.charAt(index + 1) - '0';
            if (n2 <= 26) {
                sum2 = numDecodings1_1(s, index + 2, memoization);
            }
        }
        //将结果保存
        memoization.put(index, sum1 + sum2);
        return sum1 + sum2;
    }
```


### 动态规划

如果 s [ i ] 和 s [ i + 1 ] 组成的数字小于等于 26，那么

dp [ i ] = dp[ i + 1 ] + dp [ i + 2 ]

```java
    public int numDecodings2(String s) {
        int len = s.length();
        int[] numDecoding = new int[len + 1];
        numDecoding[len] = 1;
        for (int i = len - 1; i >= 0; i--) {
            if (s.charAt(i) == '0') {
                numDecoding[i] = 0;
                continue;
            }
            if (i == len - 1) {
                numDecoding[i] = 1;
            } else {
                int n = (s.charAt(i) - '0') * 10 + s.charAt(i + 1) - '0';
                if (n <= 26) {
                    numDecoding[i] += numDecoding[i + 2];
                }
                numDecoding[i] += numDecoding[i + 1];
            }
        }
        return numDecoding[0];
    }

```
### 动态规划空间优化

每次循环中只用到dp[ i + 1 ] 和 dp [ i + 2 ]
两个变量 cur 和 last


```java
    public int numDecodings3(String s) {
        int len = s.length();
        int last = 1, cur = 0;
        if (s.charAt(len - 1) != '0') {
            cur = 1;
        } else {
            cur = 0;
        }
        for (int i = len - 2; i >= 0; i--) {
            if (s.charAt(i) == '0') {
                last = cur;
                cur = 0;
                continue;
            }
            int n = (s.charAt(i) - '0') * 10 + s.charAt(i + 1) - '0';
            int tmp = cur;
            if (n <= 26) {
                cur += last;
            }
            last = tmp;
        }
        return cur;
    }
```