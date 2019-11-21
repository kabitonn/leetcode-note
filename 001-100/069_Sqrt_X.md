# 069. Sqrt(x)(E)
[069. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)

## 题目描述(简单)

Implement int sqrt(int x).

Compute and return the square root of x, where x is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

Example 1:
```
Input: 4
Output: 2
```
Example 2:
```
Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.
```
## 思路
求小于等于算术平方根的最大值
1. 遍历
2. 二分
3. 牛顿法

## 解决方法

### 遍历



```java
    public int mySqrt(int x) {
    	for(int i=1;i<=x/2+1;i++) {
    		if(i==x/i) {return i;}
    		else if(i>x/i) {return i-1;}
    	}
    	return 0;
        
    }
```

时间复杂度：$$O(\sqrt{x})$$。

空间复杂度：O(1)。



### 二分

求小于等于算术平方根的右边界


```java 
    public int mySqrt1(int x) {
    	if(x==0) {return 0;}
    	int left = 1;
    	int right = x/2+1;
    	//int res = 0;
    	while(left<=right) {
    		int mid = left+(right-left)/2;
    		if(mid == x/mid) {return mid;}
    		else if(mid > x/mid) {right = mid-1;}
    		else {
				//res = mid;
				left = mid+1;
    		}
    	}
    	return right;
    }
```
时间复杂度：O(log (x))。

空间复杂度：O(1)。



### 改进二分

求小于等于算术平方根的右边界

```java
	public int mySqrt(int x) {
		if(x==0) {return 0;}
		int left = 1;
		int right = x/2+1;
		while(left<right){
			int mid = left + (right-left + 1)/2;
			if(mid > x / mid){	right = mid - 1;}
			else  if(mid <= x/mid){	left = mid;}
		}
		return left;
	}
```
时间复杂度：O(log (x))。

空间复杂度：O(1)。


### 公式-牛顿法

$$x_{k+1} = x_k - f(x_k)/f'(x_k)$$
$$f(x_n) = x^2 - n$$
$$x_{k+1} = x_k - (x_k^2 - n)/2x_k = (x_k^2 + n)/2x_k = (x_k + n/x_k)/2$$

```java
    public int mySqrt(int x) {
        long a = x;
        while (a * a > x) {
            a = (a + x / a) / 2;
        }
        return (int) a;
    }
```

时间复杂度：O(log (x))。

空间复杂度：O(1)。


