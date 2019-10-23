# 151. Reverse Words in a String(M)


[151. 翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)


## 题目描述(中等)

Given an input string, reverse the string word by word.

Example 1:
```
Input: "the sky is blue"
Output: "blue is sky the"
```
Example 2:
```
Input: "  hello world!  "
Output: "world! hello"
Explanation: Your reversed string should not contain leading or trailing spaces.
```
Example 3:
```
Input: "a good   example"
Output: "example good a"
Explanation: You need to reduce multiple spaces between two words to a single space in the reversed string.
```

**Note**:

- A word is defined as a sequence of non-space characters.
- Input string may contain leading or trailing spaces. However, your reversed string should not contain leading or trailing spaces.
- You need to reduce multiple spaces between two words to a single space in the reversed string.
 

**Follow up**:

For C programmers, try to solve it in-place in O(1) extra space.


## 思路

- 分隔倒序


## 解决方法



### 分隔倒序

```java
    public String reverseWords(String s) {
        String[] strs = s.trim().split("\\s+");
        int i = 0, j = strs.length - 1;
        for (; i < j; i++, j--) {
            String tmp = strs[i];
            strs[i] = strs[j];
            strs[j] = tmp;
        }
        StringBuilder sb = new StringBuilder();
        for (int k = 0; k < strs.length; k++) {
            sb.append(strs[k]);
            if (k != strs.length - 1) {
                sb.append(" ");
            }
        }
        return sb.toString();
    }
```


```java
    public String reverseWords1(String s) {
        String[] strs = s.trim().split("\\s+");
        StringBuilder sb = new StringBuilder();
        for (int k = strs.length - 1; k >= 0; k--) {
            if (k == strs.length - 1) {
                sb.append(strs[k]);
            } else {
                sb.append(" " + strs[k]);
            }
        }
        return sb.toString();
    }
```

