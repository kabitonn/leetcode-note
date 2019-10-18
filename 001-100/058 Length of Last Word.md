# 058. Length of Last Word
[link](https://leetcode-cn.com/problems/length-of-last-word/)

## 题目描述(简单)

Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

**Note**: A word is defined as a character sequence consists of non-space characters only.

Example:
```
Input: "Hello World"
Output: 5
```
## 思路

1. 过滤尾部空格，从后向前遍历，找到第一个不为字母的索引
2. 从后向前找到第一个不为空格的索引end，继续向前找到第一个部位字母的索引

## 解决方法

### 1

```java
    public int lengthOfLastWord(String s) {
        String string = s.trim();
        int i = string.length()-1;
        while(i>=0 ) {
        	if((string.charAt(i)<='z'&&string.charAt(i)>='a')||(string.charAt(i)<='Z'&&string.charAt(i)>='A')) {
        		i--;
        	}
        	else {break;}
        }
        return string.length()-i-1;
    }
```
时间复杂度：O(n)。

空间复杂度：O(1)。

### 


```java
    public int lengthOfLastWord(String s) {
        int end = s.length()-1;
        while(end>=0&&s.charAt(end)==' ') {end--;}
        int start = end;
        while(start>=0&&s.charAt(start)!=' ') {start--;}
        return end-start;
    }
```
时间复杂度：O(n)。

空间复杂度：O(1)

