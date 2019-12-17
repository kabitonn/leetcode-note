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


- 旋转排序数组 nums 可以被拆分为 2 个排序数组 nums1 , nums2，并且 nums1任一元素 >= nums2任一元素；因此，考虑二分法寻找此两数组的分界点 nums[i] (即第 2 个数组的首个元素)。
- 设置 left, right 指针在 nums 数组两端，mid 为每次二分的中点：
    - 当 nums[mid] > nums[right]时，mid 一定在第 1 个排序数组中，i 一定满足 mid < i <= right，因此执行 left = mid + 1；
    - 当 nums[mid] < nums[right] 时，mid 一定在第 2 个排序数组中，i 一定满足 left < i <= mid，因此执行 right = mid；
    - 当 nums[mid] == nums[right] 时，是此题对比 153题 的难点（原因是此题中数组的元素可重复，难以判断分界点 i 指针区间）；
        - 例如 [1, 0, 1, 1, 1] 和 [1, 1, 1, 0, 1] ，在 left = 0, right = 4, mid = 2 时，无法判断 midmid 在哪个排序数组中。
        - 我们采用 right = right - 1 解决此问题，证明：
            - 此操作不会使数组越界：因为迭代条件保证了 right > left >= 0；
            - 此操作不会使最小值丢失：假设 nums[right] 是最小值，有两种情况：
                - 若 nums[right]是唯一最小值：那就不可能满足判断条件 nums[mid] == nums[right]，因为 mid < right(left != right 且 mid = (left + right) // 2 向下取整)；
                - 若 nums[right] 不是唯一最小值，由于 mid < right 而 nums[mid] == nums[right]，即还有最小值存在于 [left, right - 1] 区间，因此不会丢失最小值。



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

时间复杂度 O(logN)，在特例情况下会退化到 O(N)(例如 [1, 1, 1, 1])


另一种写法

```java
    public int getBiasByMin(int[] nums) {
        int left = 0, right = nums.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > nums[right]) {
                left = mid + 1;
            } else if (nums[mid] == nums[left] && nums[mid] == nums[right]) {
                right--;
                left++;
            } else {
                right = mid;
            }
        }
        return left;
    }

```

时间复杂度 O(logN)，在特例情况下会退化到 O(N/2)(例如 [1, 1, 1, 1])




