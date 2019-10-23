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


求最小值的偏移点
比较中点和端点值的情况
- mid 和 start 比较
    - mid > start: 最小值在左半部分。
    - mid < start：最小值在左半部分。
  
  无论大于小于，最小值都在左半部分，所以 mid 和 start 比较是不可取的
- mid 和 end 比较
    - mid < end：最小值在左半部分(包括mid)。
    - mid > end：最小值在右半部分。

    所以我们只需要把 mid 和 end 比较，mid < end 丢弃右半部分(更新 end = mid)，mid > end 丢弃左半部分(更新 start = mid + 1)。直到 end 等于 start 时候结束就可以了。 




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

求最大值的偏移点
- mid 和 end 比较
    - mid < end：最大值在右半部分。
    - mid > end：最大值在右半部分。

  无论大于小于，最大值都在右半部分，所以 mid 和 end比较是不可取的
- mid 和 start 比较
    - mid > start: 最大值在右半部分(包括mid)。
    - mid < start：最大值在左半部分。

    所以我们只需要把 mid 和 start比较(取mid时取右端点)，mid < start 丢弃右半部分(更新 end = mid -1 )，mid < start丢弃左半部分(更新 start = mid )。直到 end 等于 start 时候结束就可以了。 



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

