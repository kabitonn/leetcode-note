
# 524. Longest Word in Dictionary through Deleting(M)

[524. 通过删除字母匹配到字典里最长单词](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/)

## 题目描述(中等)

给定一个字符串和一个字符串字典，找到字典里面最长的字符串，该字符串可以通过删除给定字符串的某些字符来得到。如果答案不止一个，返回长度最长且字典顺序最小的字符串。如果答案不存在，则返回空字符串。

示例 1:
```
输入:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

输出: 
"apple"
```

示例 2:
```
输入:
s = "abpcplea", d = ["a","b","c"]

输出: 
"a"
```

**说明**:
- 所有输入的字符串只包含小写字母。
- 字典的大小不会超过 1000。
- 所有输入的字符串长度不会超过 1000。


## 思路

检查子序列，比较长度

## 解决方法

### 检查子序列，比较长度

```java
    public String findLongestWord(String s, List<String> d) {
        String longestWord = "";
        for (String str : d) {
            if (isSubsequence(str, s)) {
                if (str.length() > longestWord.length()) {
                    longestWord = str;
                } else if (str.length() == longestWord.length() && str.compareTo(longestWord) < 0) {
                    longestWord = str;
                }
            }
        }
        return longestWord;
    }

    public boolean isSubsequence(String s, String t) {
        if (s.length() > t.length()) {
            return false;
        } else if (s.length() == 0 || s.equals(t)) {
            return true;
        }
        char[] chars = s.toCharArray();
        int i = 0;
        for (char c : t.toCharArray()) {
            if (chars[i] == c) {
                i++;
                if (i == chars.length) {
                    return true;
                }
            }
        }
        return false;
    }
```

### 排序 检查子序列

```java
    public String findLongestWord1(String s, List<String> d) {
        Collections.sort(d, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                if (o1.length() < o2.length()) {
                    return 1;
                } else if (o1.length() == o2.length()) {
                    return o1.compareTo(o2);
                } else {
                    return -1;
                }
            }
        });
        for (String str : d) {
            if (isSubsequence(str, s)) {
                return str;
            }
        }
        return "";
    }
```

时间复杂度： O(n⋅xlogn+n⋅x) 。n 表示列表 d 中字符串的数目，x 表示字符串的平均长度。排序需要花费O(nlogn) 的时间， isSubsequence 函数需要花费 O(x) 的时间去检查一个字符串是否是另一个字符串的子序列。

空间复杂度： O(logn) 。排序平均需要 O(\log n)O(logn) 的空间。



### 比较长度 检查子序列

```java
    public String findLongestWord2(String s, List<String> d) {
        String longestWord = "";
        for (String str : d) {
            if (str.length() < longestWord.length()) {
                continue;
            }
            if (str.length() == longestWord.length() && longestWord.compareTo(str) < 0) {
                continue;
            }
            if (isSubsequence1(str, s)) {
                longestWord = str;
            }
        }
        return longestWord;
    }

    public boolean isSubsequence1(String s, String t) {
        if (s.length() == 0) {
            return true;
        }
        int prev = -1;
        for (char c : s.toCharArray()) {
            int index = t.indexOf(c, prev + 1);
            if (index == -1) {
                return false;
            }
            prev = index;
        }
        return true;
    }
```

时间复杂度： O(n⋅x) 。这里 n 是列表 d 中字符串的数目， x 是字符串平均长度。

空间复杂度： O(x) 。使用了变量 max_str 。

