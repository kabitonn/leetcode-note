# 434. Number of Segments in a String(E)
[434. Number of Segments in a String](https://leetcode-cn.com/problems/number-of-segments-in-a-string/)

## 题目描述(简单)

Count the number of segments in a string, where a segment is defined to be a contiguous sequence of non-space characters.

Please note that the string does not contain any non-printable characters.

Example:
```
Input: "Hello, my name is John"
Output: 5
```

## 思路

## 解决方法

### trim split


```java 
    public int countSegments(String s) {
    	return s.trim().length() == 0 ? 0 : s.trim().split("\\s+").length; 
    }
```




### 2
遇到非空格，判断上一个是不是空格，若是则新单词

```java 
    public int countSegments(String s) {
    	s = s.trim();
    	int count = 0;
    	for(int i=0;i<s.length();i++) {
    		if(s.charAt(i)!=' '&&(i==0||s.charAt(i-1)==' ')) {
    			count++;
    		}
    	}
    	return count;
    }
```


