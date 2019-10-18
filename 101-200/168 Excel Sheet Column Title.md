## [168. Excel Sheet Column Title](https://leetcode-cn.com/problems/excel-sheet-column-title/)

## 1. 题目描述(简单)

Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:
```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
```
Example 1:
```
Input: 1
Output: "A"
```
Example 2:
```
Input: 28
Output: "AB"
```
Example 3:
```
Input: 701
Output: "ZY"
```


## 2. 思路
进制转换

## 3. 解决方法

### 3.1

```java
    public String convertToTitle(int n) {
        StringBuilder sb = new StringBuilder();
        while(n!=0) {
        	int r = n%26;
        	char c = r==0?'Z':(char) ((r-1)+'A');
        	n = r==0?(n-26)/26:n/26;
        	sb.append(c);
        }
        return sb.reverse().toString();
    }
```

### 3.2


```java
    public String convertToTitle(int n) {
    	StringBuilder sb = new StringBuilder();
    	while(n!=0) {
    		n--;
    		int r = n%26;
    		char c = (char) (r+'A');
    		n/=26;
    		sb.append(c);
    	}
    	return sb.reverse().toString();
    }
```


