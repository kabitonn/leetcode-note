
# 541. Reverse String II(E)

[541. 反转字符串 II](https://leetcode-cn.com/problems/reverse-string-ii/)

## 题目描述(简单)

给定一个字符串和一个整数 k，你需要对从字符串开头算起的每个 2k 个字符的前k个字符进行反转。如果剩余少于 k 个字符，则将剩余的所有全部反转。如果有小于 2k 但大于或等于 k 个字符，则反转前 k 个字符，并将剩余的字符保持原样。

示例:
```
输入: s = "abcdefg", k = 2
输出: "bacdfeg"
```

**要求**:
- 该字符串只包含小写的英文字母。
给定字符串的长度和 k 在[1, 10000]范围内。

## 思路

分组倒序

## 解决方法

### 分组倒序

每 2*k 个的前 k 个逆序

```java
    public String reverseStr(String s, int k) {
        char[] str = s.toCharArray();
        int n = s.length();
        for (int i = 0; i < n; i += k * 2) {
            int l = i;
            int r = i + k - 1;
            r = r < n ? r : n - 1;
            char c;
            while (l < r) {
                c = str[l];
                str[l++] = str[r];
                str[r--] = c;
            }
        }
        return new String(str);
    }
```