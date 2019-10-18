# 342. Power of Four
[342. Power of Four](https://leetcode-cn.com/problems/power-of-four/)

## 题目描述(简单)

Given an integer (signed 32 bits), write a function to check whether it is a power of 4.

Example 1:
```
Input: 16
Output: true
```
Example 2:
```
Input: 5
Output: false
```
**Follow up**: Could you solve it without loops/recursion?


## 思路

## 解决方法

### 暴力法 循环迭代


```java
	public boolean isPowerOfFour(int n) {
		if(n<=0) {return false;}
		while(n%4==0) {
			n/=4;
		}
		return n==1;
	}
```



### 公式法


```java
	public boolean isPowerOfFour(int n) {
		//double r = Math.log10(n)/Math.log10(3);
		//return r==(int)r;
		return (Math.log10(n) / Math.log10(4)) % 1 == 0;
	}
```
### 位运算



$$(n \& (n-1))==0$$ 确保仅有最高位为1
$$(0xaaaaaaaa \& n)==0$$ 确保1在奇数位上


```java
	public boolean isPowerOfFour3(int n){
		return n>0 && (n&(n-1))==0 && (0xaaaaaaaa&n)==0;
	}
```




