## [16. 3Sum Closest](https://leetcode-cn.com/problems/3sum-closest/)

## 1. 题目描述\(中等\)

Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

Example:

```
Given array nums = [-1, 2, 1, -4], and target = 1.
The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

## 2. 思路

1. 遍历所有三者之和，求出与目标差值，求最小值
   1. 对排序数组可改进，若当前差值已大于当前循环最小值，可跳出该层循环
2. 数组排序，选定一个元素，头尾双指针遍历

## 3. 解决方法

### 3.1 暴力遍历

```java
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int len = nums.length;
        int min = Integer.MAX_VALUE;
        int res = target;
        for(int i=0;i<len-2;i++) {
            for(int j=i+1;j<len-1;j++) {
                for(int k=j+1;k<len;k++) {
                    int sum = nums[i]+nums[j]+nums[k];
                    int dif = Math.abs(sum-target);
                    if(dif<min) {
                        min = dif;
                        res = sum;
                    }
                }
            }
        }
        return res;
    }
```

```java
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int len = nums.length;
        int min = Integer.MAX_VALUE;
        int res = target;
        for(int i=0;i<len-2;i++) {
            for(int j=i+1;j<len-1;j++) {
                int mink = Integer.MAX_VALUE;
                for(int k=j+1;k<len;k++) {
                    int sum = nums[i]+nums[j]+nums[k];
                    int dif = Math.abs(sum-target);
                    if(dif<min) {
                        min = dif;
                        res = sum;
                    }
                    if(dif<=mink) {
                        mink = dif;
                    }
                    else {
                        break;
                    }
                }
            }
        }
        return res;
    }
```

### 3.2 双指针

```java
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int len = nums.length;
        int min = Integer.MAX_VALUE;
        int res = target;
        int sum;
        int diff;
        for(int i=0;i<len-2;i++) {
            if(i>0&&nums[i]==nums[i-1]) {
                continue;
            }
            int low = i+1;
            int high = len-1;
            while(low<high) {
                sum = nums[low]+nums[high]+nums[i];
                diff = sum - target;
                if(Math.abs(diff)<min) {
                    min = Math.abs(diff);
                    res = sum;
                }
                if(diff<0) {
                    low++;
                }
                else if (diff>0) {
                    high--;
                }
                else {
                    break;
                }

            }
        }
        return res;
    }
```



