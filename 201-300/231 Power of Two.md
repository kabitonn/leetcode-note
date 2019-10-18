# 231. Power of Two(E)
[231. Power of Two](https://leetcode-cn.com/problems/power-of-two/)

## 题目描述(简单)

Given an integer, write a function to determine if it is a power of two.

Example 1:
```
Input: 1
Output: true 
Explanation: 2^0 = 1
```
Example 2:
```
Input: 16
Output: true
Explanation: 2^4 = 16
```
Example 3:
```
Input: 218
Output: false
```
## 思路

## 解决方法

### 遍历每一位
遍历每一位只有最高位为1

```java
    public boolean isPowerOfTwo(int n) {
    	if(n<=0) {return false;}
    	while(n!=1) {
    		if((n&1)==1) {return false;}
    		n>>=1;
    	}
        return true;
    }
```



### 最低位即最高位的1

n&(n-1)可将最低位的1变为0，二次幂的最高位为1，其余为0

```java
    public boolean isPowerOfTwo1(int n) {
        return n>0&&(n&(n-1))==0;
    }
```



