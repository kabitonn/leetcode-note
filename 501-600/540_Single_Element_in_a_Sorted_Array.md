
# 540. Single Element in a Sorted Array(M)

[540. 有序数组中的单一元素](https://leetcode-cn.com/problems/single-element-in-a-sorted-array/)

## 题目描述()

给定一个只包含整数的有序数组，每个元素都会出现两次，唯有一个数只会出现一次，找出这个数。

示例 1:
```
输入: [1,1,2,3,3,4,4,8,8]
输出: 2
```

示例 2:
```
输入: [3,3,7,7,10,11,11]
输出: 10
```

**注意**: 您的方案应该在 O(log n)时间复杂度和 O(1)空间复杂度中运行。

## 思路

- 无序数组中，遍历取异或值得到只出现一次的值
- 有序数组中，间隔遍历，和前一个不一样即为只出现一次的值
- 二分查找，只出现一次的值的所在一半的段中数量位奇数


## 解决方法

### 间隔遍历

```java
    public int singleNonDuplicate(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int i = 0;
        for (; i + 1 < n; i += 2) {
            if (nums[i] != nums[i + 1]) {
                break;
            }
        }
        return nums[i];
    }
```

### 二分查找

判断中间(中点或中点偏左)的数字和后一个数字是否相等，来继续判断左右两段的数目的奇偶性，继续在为奇数段里搜索目标

```java
    public int singleNonDuplicate1(int[] nums) {
        int n = nums.length;
        int left = 0, right = n - 1;
        while (left < right) {
            int mid = (left + right) >>> 1;
            int midNum = nums[mid] == nums[mid + 1] ? 1 : 0;
            //左侧数目的奇偶性
            int leftNum = mid - left + 1 + midNum;
            if ((leftNum & 1) == 0) {
                left = mid + midNum + 1;
            } else {
                right = mid - midNum;
            }
        }
        return nums[left];
    }
```

判断中间元素是同一元素的右侧还是左侧，还是单个元素，然后继续判断两遍数目的奇偶性

```java
    public int singleNonDuplicate1_1(int[] nums) {
        int n = nums.length;
        int left = 0, right = n - 1;
        while (left < right) {
            int mid = (left + right) >>> 1;
            boolean leftEven = ((mid - left) & 1) == 0;
            if (nums[mid] == nums[mid + 1]) {
                if (leftEven) {
                    left = mid + 2;
                } else {
                    right = mid - 1;
                }
            } else if (nums[mid] == nums[mid - 1]) {
                if (leftEven) {
                    right = mid - 2;
                } else {
                    left = mid + 1;
                }
            } else {
                return nums[mid];
            }
        }
        return nums[left];
    }
```

### 二分查找

索引[0,n-1]中，出现一次的值必然是偶数索引，在其前面出现两次的数的索引为偶数索引和奇数索引，在其后面出现两次的数的索引为奇数索引和偶数索引，

只判断偶数索引位置的数，


```java
    public int singleNonDuplicate2(int[] nums) {
        int n = nums.length;
        int left = 0, right = n - 1;
        while (left < right) {
            int mid = (left + right) >>> 1;
            if ((mid & 1) != 0) {
                mid--;
            }
            if (nums[mid] == nums[mid + 1]) {
                left = mid + 2;
            } else {
                right = mid;
            }
        }
        return nums[left];
    }
```