# 152. Maximum Product Subarray(M)


[152. 乘积最大子序列](https://leetcode-cn.com/problems/maximum-product-subarray/)


## 题目描述(中等)

Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

Example 1:
```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```
Example 2:
```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```


## 思路



## 解决方法



### 暴力遍历

```java
    public int maxProduct(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int maxProduct = Integer.MIN_VALUE;
        for (int i = 0; i < n; i++) {
            int prodcut = 1;
            for (int j = i; j < n; j++) {
                prodcut *= nums[j];
                if (prodcut > maxProduct) {
                    maxProduct = prodcut;
                }
            }
        }
        return maxProduct;
    }
```



### 动态规划

max, min 存储以当前数字为结尾的子串的最大值最小值

对当前数字num判断
- num < 0 ：max, min = min * num, max * num (同时和num比较)
- num > 0 ：max, min = max * num, min * num (同时和num比较)

```java
    public int maxProduct1(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int maxProduct = nums[0];
        int max = nums[0], min = nums[0];
        for (int i = 1; i < n; i++) {
            if (nums[i] < 0) {
                int tmp = max;
                max = min;
                min = tmp;
            }
            max = Math.max(max * nums[i], nums[i]);
            min = Math.min(min * nums[i], nums[i]);
            maxProduct = Math.max(maxProduct, max);
        }
        return maxProduct;
    }
```


```java

```
