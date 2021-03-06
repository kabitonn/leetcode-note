# 202. Happy Number(E)
[202. Happy Number](https://leetcode-cn.com/problems/happy-number/)

## 题目描述\(简单\)

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 \(where it will stay\), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example:

```
Input: 19
Output: true
Explanation: 
1^2 + 9^2 = 82
8^2 + 2^2 = 68
6^2 + 8^2 = 100
1^2 + 0^2 + 0^2 = 1
```

## 思路

迭代/递归

## 解决方法

### set判断重复

```java
    public boolean isHappy(int n) {
        Set<Integer> set = new HashSet<>();
    	while(n!=1) {
        	int num = 0;
        	while(n!=0) {
        		num += (n%10)*(n%10);
        		n/=10;
        	}
        	if(set.contains(num)) {
        		return false;
        	}
        	set.add(num);
        	n=num;
        }
    	return true;
    }
```

### 特殊值终止
10以内只有1和7是符合要求的数，其余可直接返回false

```java
    public boolean isHappy1(int n) {
        int x = n;
    	while(n!=1) {
        	int num = 0;
        	while(n!=0) {
        		num += (n%10)*(n%10);
        		n/=10;
        	}
        	if(num == x||(num/10==0&&num!=1&&num!=7)) {
        		return false;
        	}
        	n=num;
        }
    	return true;
    }
```


### 快慢指针

“快指针”每次走两步，“慢指针”每次走一步，当二者相等时，即为一个循环周期。此时，判断是不是因为1引起的循环，是的话就是快乐数，否则不是快乐数。

```java
    public boolean isHappy2(int n) {
        int slow = n;
        int fast = bitSquareSum(n);
        while (slow != fast) {
            slow = bitSquareSum(slow);
            fast = bitSquareSum(fast);
            fast = bitSquareSum(fast);
        }
        return slow == 1;
    }

    public int bitSquareSum(int n) {
        int num = 0;
        while (n != 0) {
            num += (n % 10) * (n % 10);
            n /= 10;
        }
        return num;
    }
```
