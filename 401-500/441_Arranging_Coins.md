# 441. Arranging Coins(E)
[441. Arranging Coins](https://leetcode-cn.com/problems/arranging-coins/)

## 题目描述(简单)

You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.

Given n, find the total number of full staircase rows that can be formed.

n is a non-negative integer and fits within the range of a 32-bit signed integer.

Example 1:
```
n = 5

The coins can form the following rows:
¤
¤ ¤
¤ ¤

Because the 3rd row is incomplete, we return 2.
```

Example 2:
```
n = 8

The coins can form the following rows:
¤
¤ ¤
¤ ¤ ¤
¤ ¤

Because the 4th row is incomplete, we return 3.
```

## 思路

## 解决方法

### 遍历


```java
    public int arrangeCoins(int n) {
    	int stage = 1;
        while(n>=stage) {
        	n-=stage++;
        }
        return stage-1;
    }
```



### 二分法

求小于等于符合要求的最大值
求满足 $$i * (i + 1) / 2 <= n $$的最大的那个 i
```java
    public int arrangeCoins(int n) {
    	int low = 1;
    	int high = n;
    	while(low<high) {
    		int mid = low+(high-low+1)/2;
    		if(1L * mid*(mid + 1) /2 <= n){
                low = mid;
            }else {
                high = mid - 1;
            }
    	}
    	return low;
    }
```
### 解数学公式


```java
    public int arrangeCoins(int n) {
        return (int) ((-1 + Math.sqrt(1 + 8 * (long) n)) / 2);
    }
```




