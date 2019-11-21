# 374. Guess Number Higher or Lower(E)
[374. Guess Number Higher or Lower](https://leetcode-cn.com/problems/guess-number-higher-or-lower/)

## 题目描述(简单)

We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API guess(int num) which returns 3 possible results (-1, 1, or 0):

> -1 : My number is lower
>  1 : My number is higher
>  0 : Congrats! You got it!

Example :
```
Input: n = 10, pick = 6
Output: 6
```

## 思路

二分查找

## 解决方法

### 基本二分


```java
    public int guessNumber(int n) {
        int low = 1;
        int high = n;
        while(low<=high) {
        	int mid = low+(high-low)/2;
        	if(guess(mid)==0) {
        		return mid;
        	}
        	else if (guess(mid)==-1) {
				high = mid-1;
			}
        	else {
				low = mid+1;
			}
        }
        return 0;
    }
```


### 二分修改


```java
    public int guessNumber1(int n) {
        int low = 1;
        int high = n;
        while(low<high) {
        	int mid = low+(high-low+1)/2;
        	if (guess(mid)==-1) {
				high = mid-1;
			}
        	else {
				low = mid;
			}
        }
        return low;
    }
```
### 遍历


```java
	public int guessNumber(int n) {
		for (int i = 1; i < n; i++)
			if (guess(i) == 0)
				return i;
		return n;
	}
```




