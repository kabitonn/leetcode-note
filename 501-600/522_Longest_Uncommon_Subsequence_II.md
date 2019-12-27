
# 522. Longest Uncommon Subsequence II(M)

[522. 最长特殊序列 II](https://leetcode-cn.com/problems/longest-uncommon-subsequence-ii/)

## 题目描述(中等)

给定字符串列表，你需要从它们中找出最长的特殊序列。最长特殊序列定义如下：该序列为某字符串独有的最长子序列（即不能是其他字符串的子序列）。

子序列可以通过删去字符串中的某些字符实现，但不能改变剩余字符的相对顺序。空序列为所有字符串的子序列，任何字符串为其自身的子序列。

输入将是一个字符串列表，输出是最长特殊序列的长度。如果最长特殊序列不存在，返回 -1 。


示例：
```
输入: "aba", "cdc", "eae"
输出: 3
```

**提示**：
- 所有给定的字符串长度不会超过 10 。
- 给定字符串列表的长度将在 [2, 50 ] 之间。


## 思路

## 解决方法

### 遍历 判断

若一个字符串不是其余字符串的子序列，则该字符串可以作为特殊序列，遍历每个字符串判断

```java
    public int findLUSlength(String[] strs) {
        int n = strs.length;
        int maxLen = -1;
        for (int i = 0; i < n; i++) {
            boolean flag = true;
            for (int j = 0; j < n; j++) {
                if (i != j && isSubsequence(strs[i], strs[j])) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                maxLen = Math.max(maxLen, strs[i].length());
            }
        }
        return maxLen;
    }

    public boolean isSubsequence(String s, String t) {
        if (s.length() <= 0) {
            return true;
        }
        if (s.length() > t.length()) {
            return false;
        }
        int i = 0;
        for (int j = 0; j < t.length(); j++) {
            if (s.charAt(i) == t.charAt(j)) {
                i++;
                if (i == s.length()) {
                    return true;
                }
            }
        }
        return false;
    }
```

### 排序 遍历 判断

经过排序后，从最长字符串开始判断，若找到特殊序列即为最长特殊序列

```java
    public int findLUSlength1(String[] strs) {
        Arrays.sort(strs, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o2.length() - o1.length();
            }
        });
        int n = strs.length;
        for (int i = 0; i < n; i++) {
            boolean flag = true;
            for (int j = 0; j < n; j++) {
                if (i != j && isSubsequence(strs[i], strs[j])) {
                    flag = false;
                    break;
                }
            }
            if (flag) {
                return strs[i].length();
            }
        }
        return -1;
    }
```