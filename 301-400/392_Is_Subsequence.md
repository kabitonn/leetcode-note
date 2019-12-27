# 392. Is Subsequence(M)

[392. 判断子序列](https://leetcode-cn.com/problems/is-subsequence/)

## 题目描述(中等)

给定字符串 `s` 和 `t` ，判断 s 是否为 t 的子序列。

你可以认为 s 和 t 中仅包含英文小写字母。字符串 t 可能会很长（长度 ~= 500,000），而 s 是个短字符串（长度 <=100）。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，"ace"是"abcde"的一个子序列，而"aec"不是）。

示例 1:
```
s = "abc", t = "ahbgdc"

返回 true.
```
示例 2:
```
s = "axc", t = "ahbgdc"

返回 false.
```
**后续挑战** :

如果有大量输入的 S，称作S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

## 思路

- 索引
- 遍历匹配

## 解决方法

### 向后查找索引

依次查找子序列字符在母串中前一个字符的索引向后搜索是否能搜索到

```java
    public boolean isSubsequence(String s, String t) {
        if (s.length() <= 0) {
            return true;
        }
        int preIndex = -1;
        for (char c : s.toCharArray()) {
            int index = t.indexOf(c, preIndex + 1);
            if (index == -1) {
                return false;
            }
            preIndex = index;
        }
        return true;
    }
```

### 向后遍历是否存在子序列字符

依次向后遍历子序列中字符是否按索引顺序出现在母串中

```java
    public boolean isSubsequence1(String s, String t) {
        if (s.length() <= 0) {
            return true;
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

### 后续：大批量输入量

- 创建母串每种字母的索引序列
- 对待判断的子序列，从前向后判断每种字母在母串中的索引是否符合要求
  - 索引为大于前一个字母索引的左边界

```java
    List<Integer>[] index = new List[26];

    public boolean isSubsequence2(String s, String t) {
        for (int i = 0; i < 26; i++) {
            index[i] = new ArrayList<>();
        }
        for (int i = 0; i < t.length(); i++) {
            index[t.charAt(i) - 'a'].add(i);
        }
        int preIndex = -1;
        for (char c : s.toCharArray()) {
            List<Integer> list = index[c - 'a'];
            int left = 0, right = list.size();
            while (left < right) {
                int mid = (left + right) >>> 1;
                if (list.get(mid) <= preIndex) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            if (left == list.size()) {
                return false;
            }
            preIndex = list.get(left);
        }
        return true;
    }
```