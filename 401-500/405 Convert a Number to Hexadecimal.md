# [405. Convert a Number to Hexadecimal(E)
[405. Convert a Number to Hexadecimal](https://leetcode-cn.com/problems/convert-a-number-to-hexadecimal/)

## 题目描述(简单)

Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

**Note:**

1. All letters in hexadecimal (a-f) must be in lowercase.
2. The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.
3. The given number is guaranteed to fit within the range of a 32-bit signed integer.
4. You must not use any method provided by the library which converts/formats the number to hex directly.

Example 1:
```
Input:
26

Output:
"1a"
```
Example 2:
```
Input:
-1

Output:
"ffffffff"
```


## 思路
10进制转16进制，取后四位bit转换，右移4位
## 解决方法

### 无符号右移4位 位与运算

```java
    public String toHex(int num) {
    	if(num==0) {return "0";}
    	char[] map = new char[] {'0','1','2','3','4','5','6','7','8','9','a','b','c','d','e','f'};
        StringBuilder sb =  new StringBuilder();
    	while(num!=0) {
        	int last = num&15;
        	sb.append(map[last]);
        	num>>>=4;
        }
    	return sb.reverse().toString();
    }
```


```java
    public String toHex(int num) {
    	if(num==0) {return "0";}
    	char[] map = new char[] {'0','1','2','3','4','5','6','7','8','9','a','b','c','d','e','f'};
        String string = "";
    	while(num!=0) {
    		string = map[(num&0xf)]+string;
        	num>>>=4;
        }
    	return string;
    }
```


