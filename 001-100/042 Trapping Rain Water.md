# 042. Trapping Rain Water(H)

## 题目描述(困难)

Given an unsorted integer array, find the smallest missing positive integer.

Example 1:
```
Input: [1,2,0]
Output: 3
```
Example 2:
```
Input: [3,4,-1,1]
Output: 2
```
Example 3:
```
Input: [7,8,9,11,12]
Output: 1
```

**Note**:
Your algorithm should run in O(n) time and uses constant extra space.



## 思路

1. 排序
2. 交换
3. 标记

## 解决方法

### 交换

```java
    public int firstMissingPositive(int[] nums) {
        int len = nums.length;
        for (int i = 0; i < len; ) {
            if (nums[i] <= len && nums[i] > 0 && nums[i] != nums[nums[i] - 1]) {
                swap(nums, i, nums[i] - 1);
            } else {
                i++;
            }
        }
        int miss = 0;
        for (; miss < len; miss++) {
            if (nums[miss] != miss + 1) {
                return miss + 1;
            }
        }
        return miss + 1;
    }
    
    private void swap(int[] nums, int i, int j) {
        nums[i] ^= nums[j];
        nums[j] ^= nums[i];
        nums[i] ^= nums[j];
    }

```

### 标记

```java
    public int firstMissingPositive1(int[] nums) {
        int len = nums.length;
        int positiveNum = positiveNum(nums);
        for (int i = 0; i < positiveNum; i++) {
            int index = Math.abs(nums[i]) - 1;
            if (index < positiveNum) {
                int temp = Math.abs(nums[index]);
                nums[index] = temp < 0 ? temp : -temp;
            }
        }
        int miss = 0;
        for (; miss < positiveNum; miss++) {
            if (nums[miss] >= 0) {
                return miss + 1;
            }
        }
        return miss + 1;
    }

    private int positiveNum(int[] nums) {
        int i = 0, n = nums.length - 1;
        while (i <= n) {
            if (nums[i] <= 0) {
                swap(nums, i, n--);
            } else {
                i++;
            }
        }
        return i;
    }
```
