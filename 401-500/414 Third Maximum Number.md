## [414. Third Maximum Number](https://leetcode-cn.com/problems/third-maximum-number/)

## 1. 题目描述(简单)

Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).

Example 1:
```
Input: [3, 2, 1]

Output: 1

Explanation: The third maximum is 1.
```
Example 2:
```
Input: [1, 2]

Output: 2

Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
```
Example 3:
```
Input: [2, 2, 3, 1]

Output: 1

Explanation: Note that the third maximum here means the third maximum distinct number.
             Both numbers with value 2 are both considered as second maximum.
```

## 2. 思路

## 3. 解决方法

### 3.1 排序


```java
	public int thirdMax(int[] nums) {
		Arrays.sort(nums);
		int len = nums.length;
		int count = 1;
		int third = nums[len-1];
		for(int i=len-2;i>=0&&count<3;i--) {
			if(nums[i]==nums[i+1]) {
				continue;
			}
			count++;
			third = nums[i];
		}
		if(count==3) {return third;}
		else {return nums[len-1];}
	}
```


### 3.2 只排序前三大


```java
	public int thirdMax(int[] nums) {
		int count = 0;
		int maxFirst = Integer.MIN_VALUE;
		int maxSecond = Integer.MIN_VALUE;
		int maxThird = Integer.MIN_VALUE;
		boolean minExsit = false;
		for(int n:nums) {
			if(n==Integer.MIN_VALUE&&!minExsit) {
				count++;
				minExsit = true;
			}
			if(n>maxFirst) {
				maxThird = maxSecond;
				maxSecond = maxFirst;
				maxFirst = n;
				count++;
			}
			else if (n>maxSecond&&n!=maxFirst) {
				maxThird = maxSecond;
				maxSecond = n;
				count++;
			}
			else if (n>maxThird&&n!=maxSecond&&n!=maxFirst) {
				maxThird = n;
				count++;
			}
		}
		if(count>=3) {return maxThird;}
		else {		return maxFirst;}
	}
```


```java
	public int thirdMax(int[] nums) {
		long maxFirst=Long.MIN_VALUE,maxSecond=Long.MIN_VALUE,maxThird=Long.MIN_VALUE;
		for(long num:nums){
			if(num>maxFirst){
				maxThird=maxSecond;
				maxSecond=maxFirst;
				maxFirst=num;
			}else if(num>maxSecond&&num<maxFirst){
				maxThird=maxSecond;
				maxSecond=num;
			}else if(num>maxThird&&num<maxSecond){
				maxThird=num;
			}
		}
		return (maxThird==Long.MIN_VALUE||maxThird==maxSecond)?(int)maxFirst:(int)maxThird;
	}
```


```java
	public int thirdMax(int[] nums) {
		Integer maxFirst = null;
		Integer maxSecond = null;
		Integer maxThird = null;
		for(int num:nums){
			if(maxFirst==null||num>maxFirst){
				maxThird=maxSecond;
				maxSecond=maxFirst;
				maxFirst=num;
			}else if(num<maxFirst&&(maxSecond==null||num>maxSecond)){
				maxThird=maxSecond;
				maxSecond=num;
			}else if(num<maxFirst&&num<maxSecond&&(maxThird==null||num>maxThird)){
				maxThird=num;
			}
		}
		return (maxThird==null)?maxFirst:maxThird;
	}
```





