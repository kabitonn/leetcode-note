
# 557. Reverse Words in a String III(E)
 
[557. 反转字符串中的单词 III](https://leetcode-cn.com/problems/reverse-words-in-a-string-iii/)

## 题目描述(简单)

给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

示例 1:
```
输入: "Let's take LeetCode contest"
输出: "s'teL ekat edoCteeL tsetnoc" 
注意：在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。
```

## 思路

## 解决方法

### 拆分反转合并

```java
    public String reverseWords(String s) {
        String[] strs = s.split(" ");
        StringBuilder sb = new StringBuilder();
        for (String str : strs) {
            char[] chars = str.toCharArray();
            for (int i = 0, j = chars.length - 1; i < j; i++, j--) {
                char c = chars[i];
                chars[i] = chars[j];
                chars[j] = c;
            }
            sb.append(chars);
            sb.append(" ");
        }
        sb.deleteCharAt(sb.length() - 1);
        return sb.toString();
    }
```

### 双指针区间 反转

```java
    public String reverseWords1(String s) {
        char[] str = s.toCharArray();
        int n = s.length();
        int i = 0;
        while (i < n) {
            int j = i;
            while (j < n && str[j] != ' ') {
                j++;
            }
            for (int l = i, r = j - 1; l < r; l++, r--) {
                char c = str[l];
                str[l] = str[r];
                str[r] = c;
            }
            i = j + 1;
        }
        return new String(str);
    }
```