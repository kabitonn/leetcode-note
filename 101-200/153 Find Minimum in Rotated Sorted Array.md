# 153. Find Minimum in Rotated Sorted Array(M)


[153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)


## 题目描述(中等)

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

`(i.e.,  [0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2])`.

Find the minimum element.

You may assume no duplicate exists in the array.

Example 1:
```
Input: [3,4,5,1,2] 
Output: 1
```
Example 2:
```
Input: [4,5,6,7,0,1,2]
Output: 0
```


## 思路

- 遍历
- 二分

## 解决方法


### 遍历


```java
    public int findMin(int[] nums) {
        int bias = getBiasByMax(nums);
        return nums[bias];
    }
```


### 二分搜索


设置left, right指针在nums数组两端，mid为中点：
- 当nums[mid] > nums[right]时，一定满足mid < i <= right，因此left = mid + 1；
- 当nums[mid] < nums[right]时，一定满足left< i <= mid，因此right = mid；
- 当nums[mid] == nums[right]时，说明数组长度len(num) == 1（因为计算mid向下取整）；当left = right也满足，但本题left == right时跳出循环。


```java
    public int getBiasByMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }
        return left;
    }

```


```java
    public int getBiasByMax(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left + 1) / 2;
            if (nums[mid] <= nums[left]) {
                right = mid - 1;
            } else {
                left = mid;
            }
        }
        return (right + 1) % nums.length;
    }
```

