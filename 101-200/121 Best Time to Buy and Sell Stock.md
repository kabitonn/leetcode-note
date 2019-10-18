## [121. Best Time to Buy and Sell Stock](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

## 题目描述(简单)

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

**Note **that you cannot sell a stock before you buy one.

Example 1:
```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```
Example 2:
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```


## 思路

1. 二次遍历
2. 一次遍历 动态规划

## 解决方法

### 二次遍历
遍历所有情况，求出最大利益

```java
    public int maxProfit(int[] prices) {
        int max = 0;
        for(int i=0;i<prices.length-1;i++) {
        	for(int j=i+1;j<prices.length;j++) {
        		if(prices[j]-prices[i]>max) {
        			max = prices[j]-prices[i];
        		}
        	}
        }
        return max;
    }
```
时间复杂度：$$O(n^2)$$，循环运行$$n(n-1)/2$$次
空间复杂度：O(1)


### 一次遍历 动态规划
正向遍历，保存当前最小值，计算最大利益

```java
    public int maxProfit(int[] prices) {
        int maxProfit = 0;
        int minPrice = Integer.MAX_VALUE;
        for(int i=0;i<prices.length;i++) {
        	if(prices[i]<minPrice) {minPrice=prices[i];}
        	else if(prices[i] - minPrice > maxProfit) {
        		maxProfit = prices[i] - minPrice;
        	}
        }
        return maxProfit;
    }
```
时间复杂度：O(n)
空间复杂度：O(1)



