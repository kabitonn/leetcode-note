# 154. Find Minimum in Rotated Sorted Array II(H)


[154. 寻找旋转排序数组中的最小值 II](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/)


## 题目描述(困难)

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

`(i.e.,  [0,1,2,4,5,6,7]` might become  `[4,5,6,7,0,1,2])`.

Find the minimum element.

The array may contain duplicates.

Example 1:
```
Input: [1,3,5]
Output: 1
```
Example 2:
```
Input: [2,2,2,0,1]
Output: 0
```

**Note**:


This is a follow up problem to Find Minimum in Rotated Sorted Array.
Would allow duplicates affect the run-time complexity? How and why?




## 思路

- 遍历
- 二分搜索


## 解决方法


### 遍历

```java
public int findMin(int[] nums) {
        int min = nums[0];
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] < nums[i - 1]) {
                min = nums[i];
                break;
            }
        }
        return min;
    }
```


### 二分搜索


```java
    public int getBiasByMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else if (nums[mid] == nums[right]) {
                right = right - 1;
            } else {
                right = mid;
            }
        }
        return left;
    }
```



