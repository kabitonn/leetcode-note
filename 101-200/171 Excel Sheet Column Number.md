# 171. Excel Sheet Column Number(E)
[171. Excel Sheet Column Number](https://leetcode-cn.com/problems/excel-sheet-column-number/)

## 题目描述(简单)

Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:
```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```
Example 1:
```
Input: "A"
Output: 1
```
Example 2:
```
Input: "AB"
Output: 28
```
Example 3:
```
Input: "ZY"
Output: 701
```


## 思路
26进制转10进制

## 解决方法

### 1

```java
    public int titleToNumber(String s) {
        int num = 0;
    	for(char c:s.toCharArray()) {
        	int n = c-'A'+1;
        	num = num*26+n;
        }
    	return num;
    }
```




