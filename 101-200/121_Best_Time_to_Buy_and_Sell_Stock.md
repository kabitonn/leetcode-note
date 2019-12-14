# 121. Best Time to Buy and Sell Stock(E)
[121. Best Time to Buy and Sell Stock](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

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

1. 双重遍历
2. 一次遍历 动态规划

## 解决方法

### 暴力法 双重遍历
遍历所有情况，求出最大利益

```java
    public int maxProfit(int[] prices) {
        int max = 0;
        for (int i = 0; i < prices.length - 1; i++) {
            for (int j = i + 1; j < prices.length; j++) {
                max = Math.max(prices[j] - prices[i], max);
            }
        }
        return max;
    }
```
时间复杂度：$O(n^2)$，循环运行$n(n-1)/2$次
空间复杂度：O(1)


### 一次遍历 双指针
正向遍历，保存当前最小值，计算最大利益

```java
    public int maxProfit1(int[] prices) {
        int maxProfit = 0;
        int minPrice = Integer.MAX_VALUE;
        for (int price : prices) {
            if (price < minPrice) {
                minPrice = price;
            } else {
                maxProfit = Math.max(price - minPrice, maxProfit);
            }
        }
        return maxProfit;
    }
```
时间复杂度：O(n)
空间复杂度：O(1)

### 动态规划
对于数组 1 6 2 8，代表股票每天的价格。

定义一下转换规则，当前天的价格减去前一天的价格，第一天由于没有前一天，规定为 0，用来代表不操作。

数组就转换为 0 6-1 2-6 8-2，也就是 0 5 -4 6。现在的数组的含义就变成了股票相对于前一天的变化了。

现在我们只需要找出连续的和最大是多少就可以了

```java
    public int maxProfit2(int[] prices) {
        int n = prices.length;
        int dp = 0;
        int max = 0;
        for (int i = 1; i < n; i++) {
            int num = prices[i] - prices[i - 1];
            dp = Math.max(dp + num, num);
            max = Math.max(max, dp);
        }
        return max;
    }
```

这个算法其实叫做 Kadane 算法
如果序列中含有负数，并且可以不选择任何一个数，那么最小的和也肯定是 0，也就是上边的情况，这也是把我们把第一天的浮动当作是 0 的原因。所以 max初始化成了 0