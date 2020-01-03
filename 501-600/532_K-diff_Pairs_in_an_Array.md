
# 532. K-diff Pairs in an Array(E)

[532. 数组中的K-diff数对](https://leetcode-cn.com/problems/k-diff-pairs-in-an-array/)

## 题目描述(简单)

给定一个整数数组和一个整数 k, 你需要在数组里找到不同的 k-diff 数对。这里将 k-diff 数对定义为一个整数对 (i, j), 其中 i 和 j 都是数组中的数字，且两数之差的绝对值是 k.

示例 1:
```
输入: [3, 1, 4, 1, 5], k = 2
输出: 2
解释: 数组中有两个 2-diff 数对, (1, 3) 和 (3, 5)。
尽管数组中有两个1，但我们只应返回不同的数对的数量。
```

示例 2:
```
输入:[1, 2, 3, 4, 5], k = 1
输出: 4
解释: 数组中有四个 1-diff 数对, (1, 2), (2, 3), (3, 4) 和 (4, 5)。
```

示例 3:
```
输入: [1, 3, 1, 5, 4], k = 0
输出: 1
解释: 数组中只有一个 0-diff 数对，(1, 1)。
```

**注意**:
- 数对 (i, j) 和数对 (j, i) 被算作同一数对。
- 数组的长度不超过10,000。
- 所有输入的整数的范围在 [-1e7, 1e7]。


## 思路

排序 去重

## 解决方法

### 顺序遍历

有序数组中顺序遍历查找数对(nums[i],nums[j])，有且只有一个(nums[i],nums[j])，其余的数字相同为重复数对

```java
    public int findPairs(int[] nums, int k) {
        if (k < 0) {
            return 0;
        }
        int n = nums.length;
        int count = 0;
        Arrays.sort(nums);
        for (int i = 0; i < n - 1; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < n; j++) {
                if (nums[j] - nums[i] == k) {
                    count++;
                    break;
                } else if (nums[j] - nums[i] > k) {
                    break;
                }
            }
        }
        return count;
    }
```

### 二分查找


有序数组中，二分搜索满足nums[i]的数对nums[j]

```java
    public int findPairs1(int[] nums, int k) {
        if (k < 0) {
            return 0;
        }
        int n = nums.length;
        int count = 0;
        Arrays.sort(nums);
        for (int i = 0; i < n - 1; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = n - 1;
            int target = nums[i] + k;
            while (left <= right) {
                int mid = (left + right) >>> 1;
                if (nums[mid] > target) {
                    right = mid - 1;
                } else if (nums[mid] < target) {
                    left = mid + 1;
                } else {
                    count++;
                    break;
                }
            }
        }
        return count;
    }
```

### 双指针 滑动窗口

当前数对差值较大，左边界右移，若此时右边界和左边界相等则右边界右移否则不变

```java
    public int findPairs2(int[] nums, int k) {
        if (k < 0) {
            return 0;
        }
        int n = nums.length;
        int count = 0;
        Arrays.sort(nums);
        int i = 0;
        int prev = Integer.MAX_VALUE;
        for (int j = 1; j < n; j++) {
            if (nums[i] == prev || nums[j] - nums[i] > k) {
                i++;
                if (i < j) {
                    j--;
                }
            } else if (nums[j] - nums[i] == k) {
                count++;
                prev = nums[i];
                i++;
            }
        }
        return count;
    }
```