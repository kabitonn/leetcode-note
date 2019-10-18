# 258. Add Digits
[258. Add Digits](https://leetcode-cn.com/problems/add-digits/)

## 题目描述(简单)

Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

**Example:**
```
Input: 38
Output: 2 
Explanation: The process is like: 3 + 8 = 11, 1 + 1 = 2. 
             Since 2 has only one digit, return it.
```
**Follow up:**
> 
Could you do it without any loop/recursion in O(1) runtime?

## 思路

## 解决方法

### 暴力法


```java
	public int addDigits(int num) {
		while(num/10!=0) {
			int sum = 0;
			while(num!=0) {
				sum+=num%10;
				num/=10;
			}
			num=sum;
		}
		return num;
	}
```




### 规律
假设一个三位数整数$$n=100*a+10*b+c$$,变化后$$add(n)=a+b+c$$； 两者的差值$$n-add(n)=99a+9b$$，差值可以被9整除，说明每次缩小9的倍数 那么我们可以对res=num%9，若不为0则返回res，为0则返回9


```java
    public int addDigits1(int num) {
        if (num == 0) {
            return 0;
        }
        num %= 9;
        return num == 0 ? 9 : num;
    }
```



