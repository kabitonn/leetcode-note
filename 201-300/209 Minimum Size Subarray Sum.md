# 209. Minimum Size Subarray Sum(M)

[209. 长度最小的子数组](https://leetcode-cn.com/problems/minimum-size-subarray-sum/)

## 题目描述(中等)

Given an array of n positive integers and a positive integer s, find the minimal length of a contiguous subarray of which the sum ≥ s. If there isn't one, return 0 instead.

Example: 
```
Input: s = 7, nums = [2,3,1,2,4,3]
Output: 2
```

**Follow up**:

- If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n). 


## 思路

- 暴力遍历
- 双指针 滑动窗口


## 解决方法

### 暴力遍历

以每个下标为首向后求和，直到sum>=s，得到该下标的最短子数组，依次遍历每个下标


```java
    public int minSubArrayLen(int s, int[] nums) {
        int min = nums.length + 1;
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                if (sum >= s) {
                    if (j - i + 1 < min) {
                        min = j - i + 1;
                        break;
                    }
                }
            }
        }
        return min != nums.length + 1 ? min : 0;
    }

```

时间复杂度：O(n^2)
空间复杂度：O(1) 。

### 暴力优化

从 i 到 j 的和计算方法：
sum=sums[j]−sums[i]+nums[i] ，sums[j]−sums[i] 是从第 i+1 个元素到第 j 个元素的和。


```java
public int minSubArrayLen0(int s, int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        int n = nums.length;
        int min = nums.length + 1;
        int[] sums = new int[n + 1];
        sums[0] = nums[0];
        for (int i = 1; i <= n; i++) {
            sums[i] = sums[i - 1] + nums[i - 1];
        }
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                int sum = sums[j + 1] - sums[i + 1] + nums[i];
                if (sum >= s) {
                    if (j - i + 1 < min) {
                        min = j - i + 1;
                        break;
                    }
                }
            }
        }
        return min != nums.length + 1 ? min : 0;
    }
```

时间复杂度：O(n^2)
空间复杂度：O(n) 。


### 双指针 滑动窗口

用 2 个指针，一个指向数组开始的位置，一个指向数组最后的位置，并维护区间内的和 sum 大于等于 s 同时数组长度最小。

循环条件sum>=s是因为可能减去当前子数组开始位置为最优解

```java
    public int minSubArrayLen1(int s, int[] nums) {
        int i = 0, j = 0;
        int min = nums.length + 1;
        int sum = 0;
        while (j < nums.length || sum >= s) {
            if (sum < s) {
                sum += nums[j++];
            } else {
                min = Math.min(j - i, min);
                sum -= nums[i++];
            }
        }
        return min != nums.length + 1 ? min : 0;
    }
```

找到满足的子数组后，将子数组不断向后缩短

```java
    public int minSubArrayLen2(int s, int[] nums) {
        int i = 0, j = 0;
        int min = nums.length + 1;
        int sum = 0;
        while (j < nums.length) {
            sum += nums[j++];
            while (sum >= s) {
                min = Math.min(j - i, min);
                sum -= nums[i++];
            }
        }
        return min != nums.length + 1 ? min : 0;
    }
```
时间复杂度：O(n)。每个指针移动都需要 O(n) 的时间。每个元素至多被访问两次，一次被右端点访问，一次被左端点访问。
空间复杂度：O(1)


### 二分搜索

在寻找子数组右侧下标时采用二分搜索查找符合条件的左边界

```java
    public int minSubArrayLen1(int s, int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        int n = nums.length;
        int min = nums.length + 1;
        int[] sums = new int[n + 1];
        for (int i = 1; i <= n; i++) {
            sums[i] = sums[i - 1] + nums[i - 1];
        }
        for (int i = 0; i < n; i++) {
            int left = i;
            int right = n;
            boolean ok = false;
            while (left < right) {
                int mid = (left + right) >>> 1;
                int sum = sums[mid + 1] - sums[i + 1] + nums[i];
                if (sum < s) {
                    left = mid + 1;
                } else {
                    right = mid;
                    ok = true;
                }
            }
            if (ok) {
                if (left - i + 1 < min) {
                    min = left - i + 1;
                }
            }
        }
        return min != nums.length + 1 ? min : 0;
    }
```

时间复杂度：O(nlog(n)) 
空间复杂度：O(n)


