
# 334. Increasing Triplet Subsequence(M)

[334. 递增的三元子序列](https://leetcode-cn.com/problems/increasing-triplet-subsequence/)

## 题目描述(中等)

给定一个未排序的数组，判断这个数组中是否存在长度为 3 的递增子序列。

数学表达式如下:

> 如果存在这样的 i, j, k,  且满足 `0 ≤ i < j < k ≤ n-1`，
使得 `arr[i] < arr[j] < arr[k]` ，返回 true ; 否则返回 false 。

**说明**: 要求算法的时间复杂度为 O(n)，空间复杂度为 O(1) 。

示例 1:
```
输入: [1,2,3,4,5]
输出: true
```
示例 2:
```
输入: [5,4,3,2,1]
输出: false
```
## 思路

- 找上升子序列
- 找到递增的三个数即可

## 解决方法

### 动态规划找上升子序列

```java
    public boolean increasingTriplet(int[] nums) {
        int n = nums.length;
        int[] dp = new int[n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[i], dp[j]);
                }
            }
            dp[i]++;
            if (dp[i] >= 3) {
                return true;
            }
        }
        return false;
    }
```

### 贪心 二分 找上升子序列

```java
    public boolean increasingTriplet1(int[] nums) {
        int n = nums.length;
        int[] tails = new int[n];
        int len = 0;
        int left, right;
        for (int num : nums) {
            left = 0;
            right = len;
            while (left < right) {
                int mid = (left + right) >>> 1;
                if (tails[mid] < num) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            tails[left] = num;
            len = left == len ? len + 1 : len;
        }
        return len >= 3;
    }

```

### 找三个递增的数字

3个连续递增子序列，有3个槽位，a,b,c满足条件 a < b < c，即可

```java
    public boolean increasingTriplet2(int[] nums) {
        Integer[] seq = new Integer[3];
        for (int num : nums) {
            if (seq[0] == null || seq[0] >= num) {
                seq[0] = num;
            } else if (seq[1] == null || (seq[1] >= num)) {
                seq[1] = num;
            } else if (seq[2] == null || (seq[2] >= num)) {
                seq[2] = num;
                return true;
            }
        }
        return false;
    }
```

```java
    public boolean increasingTriplet3(int[] nums) {
        int first = Integer.MAX_VALUE;
        int second = Integer.MAX_VALUE;

        for (int n : nums) {
            if (n <= first) {
                first = n;
            } else if (n <= second) {
                second = n;
            } else {
                return true;
            }
        }

        return false;
    }
```