# 041. First Missing Positive(H)
[041. First Missing Positive](https://leetcode-cn.com/problems/first-missing-positive/)

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

如果没限制空间复杂度要求，采用等大数组顺序保存这些数字，遍历一遍数组遇到本应出现却丢失的数字即为缺失


1. 排序：时间复杂度不符合要求
2. 交换：在原数组上交换处理顺序保存
3. 标记

## 解决方法

### 交换

没有额外空间，原数组上交换处理，直到交换回来的数字小于 0，或者大于了数组的大小，或者它就是当前位置放的数字了
最后，只需要遍历 nums 数组，遇到第一次 nums [ i ] ！= i + 1，就说明缺失了 i + 1
因为nums 数组每个位置都存着比下标大 1 的数

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

时间复杂度：O(n)。

空间复杂度：O(1)。

### 标记

如果有额外空间，创建一个等大的数组 a，初始化为全部false，遍历数组对应存在下标的位置置为true，最后遍历数组a，如果 a [ i ] != true，就返回 i + 1

但当更新的时候，如果直接把数组的数赋值成 true，那么原来的数字就没了。这里有个很巧妙的技巧。
真正关心的只有正数。开始 a 数组的初始化是 false，所以把正数当做 false，负数当成 true。如果想要把 nums [ i ] 赋值成 true，如果 nums [ i ] 是正数，我们直接取相反数作为标记就行，如果是负数就不用管了。这样做的好处就是，遍历数字的时候，只需要取绝对值，就是原来的数了。

这样又带来一个问题，取绝对值的话，之前的负数该怎么办？一取绝对值的话，就会造成干扰。简单粗暴些，正数都放在前边，只考虑正数。负数和 0 就丢到最后，遍历的时候不去遍历就可以了。


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
    private void swap(int[] nums, int i, int j) {
        nums[i] ^= nums[j];
        nums[j] ^= nums[i];
        nums[i] ^= nums[j];
    }


```

时间复杂度：O(n)。

空间复杂度：O(1)。

