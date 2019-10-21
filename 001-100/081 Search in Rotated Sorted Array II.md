# 081. Search in Rotated Sorted Array II(M)
[081. 搜索旋转排序数组 II](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/)

## 题目描述(中等)

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,0,1,2,2,5,6] might become [2,5,6,0,0,1,2]).

You are given a target value to search. If found in the array return true, otherwise return false.

Example 1:
```
Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true
```
Example 2:
```
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
```

**Follow up**:

- This is a follow up problem to Search in Rotated Sorted Array, where nums may contain duplicates.
- Would this affect the run-time complexity? How and why?


## 思路

二分搜索

## 解决方法


### 二分搜索

先找到哪一段是有序的 (只要判断端点即可)

```java
    public boolean search(int[] nums, int target) {
        int left = 0, right = nums.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) {
                return true;
            }
            if (nums[mid] > nums[left]) {
                //左半段有序
                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else if (nums[mid] == nums[left]) {
                //无法判断哪段有序
                left++;
            } else {
                //右半段有序
                if (target <= nums[right] && target > nums[mid]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }
        return false;
    }
```

时间复杂度：最好的情况，如果没有遇到 nums [ left ] == nums [ mid ]，还是 O(log(n))。当然最差的情况，如果是类似于这种 [ 1, 1, 1, 1, 2, 1 ] ，target = 2，就是 O ( n ) 了。

空间复杂度：O ( 1 )
