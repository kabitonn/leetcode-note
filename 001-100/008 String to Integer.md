## [8. String to Integer \(atoi\)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

## 1. 题目描述\(中等\)

Implement atoi which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

**Note:**

> * Only the space character ' ' is considered as whitespace character.  
> * Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: $$[-2^{31} , 2^{31}-1]$$. If the numerical value is out of the range of representable values, INT\_MAX $$(2^{31}-1)$$ or INT\_MIN $$(-2^{31})$$ is returned.

Example 1:

```
Input: "42"  
Output: 42
```

Example 2:

```
Input: "-42"  
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.  
             Then take as many numerical digits as possible, which gets 42.
```

Example 3:

```
Input: "4193 with words"  
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.
```

Example 4:

```
Input: "words and 987"  
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical digit or a +/- sign.        
             Therefore no valid conversion could be performed.
```

Example 5:

```
Input: "-91283472332"  
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−2^31)is returned.
```

## 2. 思路

从左遍历字符串，可以遇到空格，直到遇到 ' + ' 或者数字或者 ' - ' 就表示要转换的数字开始，如果之后遇到除了数字的其他字符（包括空格）就结束遍历，输出结果

如果遇到空格或者 ' + ' 或者数字或者 ' - ' 之前遇到了其他字符，就直接输出 0 ，例如 " we1332"。

如果转换的数字超出了 int ，就返回 intMax 或者 intMin。

## 3. 解决方法

### 3.1 溢出预处理

```java
    public int myAtoi(String str) {
        int num = 0;
        boolean isMinus = false;
        String s = str.trim();
        int start=0;
        if(s.length()==0)
            return 0;
        if(s.charAt(start)=='-') {
            isMinus = true;
            start++;
        }
        else if(s.charAt(start)=='+') {
            start++;
        }
        else if(s.charAt(start)>='0'&&s.charAt(start)<='9') {

        }
        else {
            return 0;
        }
        for(int i=start;i<s.length();i++) {
            if(s.charAt(i)>='0' && s.charAt(i)<='9') {
                int n = s.charAt(i)-'0';
                if( num>Integer.MAX_VALUE/10||(num==Integer.MAX_VALUE/10 && n >=8)) {
                    if(isMinus)
                        return Integer.MIN_VALUE;
                    return Integer.MAX_VALUE;
                }

                num = num*10+n;
            }
            else {
                break;
            }
        }
        if(isMinus) {
            num = -num;
        }
        return num;
    }
```

### 3.2 转换数据类型

```java
    public int myAtoi(String str) {
        long num = 0;
        boolean isMinus = false;
        String s = str.trim();
        int start=0;
        if(s.length()==0)
            return 0;
        if(s.charAt(start)=='-') {
            isMinus = true;
            start++;
        }
        else if(s.charAt(start)=='+') {
            start++;
        }
        else if(s.charAt(start)>='0'&&s.charAt(start)<='9') {

        }
        else {
            return 0;
        }
        for(int i=start;i<s.length();i++) {
            if(s.charAt(i)>='0' && s.charAt(i)<='9') {
                int n = s.charAt(i)-'0';
                num = num*10+n;
                if(num>Integer.MAX_VALUE) {
                    if(isMinus)
                        return Integer.MIN_VALUE;
                    else
                        return Integer.MAX_VALUE;

                }
            }
            else {
                break;
            }
        }
        if(isMinus) {
            num = -num;
        }

        return (int)num;
    }
```



