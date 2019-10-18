# 415. Add Strings(E)
[415. Add Strings](https://leetcode-cn.com/problems/add-strings/)

## 题目描述(简单)

Given two non-negative integers num1 and num2 represented as string, return the sum of num1 and num2.

**Note:**

1. The length of both num1 and num2 is < 5100.
2. Both num1 and num2 contains only digits 0-9.
3. Both num1 and num2 does not contain any leading zero.
4. You must not use any built-in BigInteger library or convert the inputs to integer directly.



## 思路

## 解决方法

### 双指针


```java
    public String addStrings(String num1, String num2) {
    	String str = "";
    	int carry = 0;
    	int i=0,len1=num1.length(),len2=num2.length();
    	while(carry!=0||i<len1||i<len2) {
    		int a = 0,b = 0;
    		if(i<len1) {
    			a = num1.charAt(len1-1-i)-'0';
    		}
    		if(i<len2) {
    			b = num2.charAt(len2-1-i)-'0';
    		}
    		int sum = a+b+carry;
    		carry = sum/10;
    		str = sum%10+str;
    		i++;
    	}
    	
        return str;
    }
```

### 双指针

```java
    public String addStrings(String num1, String num2) {
        StringBuilder res = new StringBuilder("");
        int i = num1.length() - 1, j = num2.length() - 1, carry = 0;
        while(i >= 0 || j >= 0||carry!=0){
            int n1 = i >= 0 ? num1.charAt(i) - '0' : 0;
            int n2 = j >= 0 ? num2.charAt(j) - '0' : 0;
            int tmp = n1 + n2 + carry;
            carry = tmp / 10;
            res.append(tmp % 10);
            i--; j--;
        }
        return res.reverse().toString();
    }
```




### 2

