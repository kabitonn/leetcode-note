# 172. Factorial Trailing Zeroes(E)
[172. 阶乘后的零](https://leetcode-cn.com/problems/factorial-trailing-zeroes/)

## 题目描述(简单)

Given an integer n, return the number of trailing zeroes in n!.

Example 1:
```
Input: 3
Output: 0
Explanation: 3! = 6, no trailing zero.
```
Example 2:
```
Input: 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```
**Note**: Your solution should be in logarithmic time complexity.




## 思路
组成阶乘的数中共有多少对 2 和 5 的组合即可。又因为 5 的个数一定比 2 少，问题简化为计算 5 的个数

## 解决方法

### 迭代


```java
    public int trailingZeroes(int n) {
    	int count = 0;
    	while(n/5!=0) {
    		count += n/5;
    		n/=5;
    	}
    	return count;
    }
```



### 递归


```java
    public int trailingZeroes(int n) {
		return n == 0 ? 0 : n / 5 + trailingZeroes(n / 5);
	}
```



