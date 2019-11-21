# 067. Add Binary(E)
[067. 二进制求和](https://leetcode-cn.com/problems/add-binary/)

## 题目描述(简单)

Given two binary strings, return their sum (also a binary string).

The input strings are both non-empty and contains only characters 1 or 0.

Example 1:
```
Input: a = "11", b = "1"
Output: "100"
```
Example 2:
```
Input: a = "1010", b = "1011"
Output: "10101"
```

## 思路
1. 低位相加插入头部
2. 低位相加插入尾部reverse

## 解决方法

### 低位相加插入头部



```java
	public String addBinary(String a, String b) {
		StringBuilder sb = new StringBuilder();
		int i = a.length() - 1;
		int j = b.length() - 1;
		int carry = 0;
		while (i >= 0 || j >= 0) {
			int num1 = i >= 0 ? a.charAt(i) - 48 : 0;
			int num2 = j >= 0 ? b.charAt(j) - 48 : 0;
			int sum = num1 + num2 + carry;
			carry = 0;
			if (sum >= 2) {
				sum = sum % 2;
				carry = 1;
			}
			sb.insert(0, sum);
			i--;
			j--;

		}
		if (carry == 1) {
			sb.insert(0, 1);
		}
		return sb.toString();
	}
```
时间复杂度：O(max (m, n)) 。m 和 n 分别是字符串 a 和 b 的长度。

空间复杂度：O(1) 。

### 低位相加插入尾部reverse

多次 insert 会很耗费时间，不如最后直接 reverse

```java
    public String addBinary(String a, String b) {
    	int i = a.length()-1;
    	int j = b.length()-1;
    	char[] stra = a.toCharArray();
    	char[] strb = b.toCharArray();
    	int carry = 0;
    	StringBuilder sb = new StringBuilder();
    	while(i>=0||j>=0||carry!=0) {
    		int c = 0 + carry;
    		if(i>=0) {	c+=stra[i--] - '0';}
    		if(j>=0) {	c+=strb[j--] - '0';}
    		carry = c/2;
    		c %= 2;
    		sb.append((char)(c+'0'));
    		//sb.insert(0,(char)(c+'0'));
    	}
        return sb.reverse().toString();
    }
```
时间复杂度：O(max (m, n)) 。m 和 n 分别是字符串 a 和 b 的长度。

空间复杂度：O(1) 。



