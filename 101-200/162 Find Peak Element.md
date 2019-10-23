# 162. Find Peak Element(M)


[162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)


## 题目描述(中等)

A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.

Example 1:
```
Input: nums = [1,2,3,1]
Output: 2
Explanation: 3 is a peak element and your function should return the index number 2.
```
Example 2:
```
Input: nums = [1,2,1,3,5,6,4]
Output: 1 or 5 
Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
```

**Note**:

Your solution should be in logarithmic complexity.


## 思路



## 解决方法



### 遍历

边界值特殊考虑

```java
    public int findPeakElement(int[] nums) {
        int n = nums.length;
        int i = 0;
        for (; i < n; i++) {
            if (i == 0) {
                if (n > 1 && nums[i] > nums[i + 1]) {
                    break;
                } else if (n == 1) {
                    break;
                }
            } else if (i == n - 1) {
                if (nums[i] > nums[i - 1]) {
                    break;
                }
            } else {
                if (nums[i] > nums[i - 1] && nums[i] > nums[i + 1]) {
                    break;
                }
            }
        }
        return i;
    }
```




