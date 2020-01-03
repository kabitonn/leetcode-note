
# 560. Subarray Sum Equals K(M)
 
[560. 和为K的子数组](https://leetcode-cn.com/problems/subarray-sum-equals-k/)

## 题目描述(中等)

给定一个整数数组和一个整数 k，你需要找到该数组中和为 k 的连续的子数组的个数。

示例 1 :
```
输入:nums = [1,1,1], k = 2
输出: 2 , [1,1] 与 [1,1] 为两种不同的情况。
```

**说明**:
- 数组的长度为 [1, 20,000]。
- 数组中元素的范围是 [-1000, 1000] ，且整数 k 的范围是 [-1e7, 1e7]。


## 思路

前缀和 哈希计数

## 解决方法

### 哈希计数

```java
    public int subarraySum(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        int sum = 0;
        int count = 0;
        map.put(0, 1);
        for (int n : nums) {
            sum += n;
            if (map.containsKey(sum - k)) {
                count += map.get(sum - k);
            }
            map.put(sum, map.getOrDefault(sum, 0) + 1);
        }
        return count;
    }
```

### 桶计数


```java
    public int subarraySum1(int[] nums, int k) {
        int sum = 0;
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        for (int n : nums) {
            sum += n;
            min = Math.min(min, sum);
            max = Math.max(max, sum);
        }
        int[] sums = new int[max - min + 1];
        int count = 0;
        sum = 0;
        for (int n : nums) {
            sum += n;
            int target = sum - min - k;
            if (target >= 0 && target < sums.length) {
                count += sums[target];
            }
            sums[sum - min]++;
        }
        if (k - min >= 0 && k - min < sums.length) {
            count += sums[k - min];
        }
        return count;
    }
```

### 暴力遍历

```java
    public int subarraySum2(int[] nums, int k) {
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            int sum = 0;
            for (int j = i; j < nums.length; j++) {
                sum += nums[j];
                if (sum == k) {
                    count++;
                }
            }
        }
        return count;
    }
```