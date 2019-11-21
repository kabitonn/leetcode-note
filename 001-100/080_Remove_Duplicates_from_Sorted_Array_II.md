# 080. Remove Duplicates from Sorted Array II(M)
[080. 删除排序数组中的重复项 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/)

## 题目描述(中等)

Given a sorted array nums, remove the duplicates in-place such that duplicates appeared at most twice and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

Example 1:
```
Given nums = [1,1,1,2,2,3],

Your function should return length = 5, 
with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.

It doesn't matter what you leave beyond the returned length.
```
Example 2:
```
Given nums = [0,0,1,1,1,1,2,3,3],

Your function should return length = 7, 
with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively.

It doesn't matter what values are set beyond the returned length.
```

Clarification:
```
Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

## 思路

1. 标记
2. 快慢指针

## 解决方法


### 标记

依次遍历数组，若和前者一样判断是不是已经有重复的出现

```java
    public int removeDuplicates(int[] nums) {
        if (nums.length <= 1) {
            return nums.length;
        }
        int index = 1;
        boolean duplicate = false;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != nums[i - 1]) {
                nums[index++] = nums[i];
                duplicate = false;
            } else if (!duplicate) {
                nums[index++] = nums[i];
                duplicate = true;
            }
        }
        return index;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。

### 快慢指针 计数

慢指针指向满足条件的数字的末尾，快指针遍历原数组。并且用一个变量记录当前末尾数字出现了几次，防止超过两次。


```java
public int removeDuplicates1(int[] nums) {
        int slow = 0;
        int fast = 1;
        int count = 1;
        int max = 2;
        for (; fast < nums.length; fast++) {
            //当前遍历的数字和慢指针末尾数字不同，就加到慢指针的末尾
            if (nums[fast] != nums[slow]) {
                nums[++slow] = nums[fast];
                count = 1;
            } else if (count++ < max) {
                nums[++slow] = nums[fast];
            }
            
        }
        return slow + 1;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。



### 快慢指针



当前快指针遍历的数字和慢指针指向的数字的前一个数字比较（也就是满足条件的倒数第 2 个数），如果相等，因为有序，所有倒数第 1 个数字和倒数第 2 个数字都等于当前数字，再添加就超过 2 个了，所有不添加，如果不相等，那么就添加。s 代表 slow，f 代表 fast。

```
//相等的情况
1 1 1 1 1 2 2 3
  ^   ^ 
  s   f
//不相等的情况  
1 1 1 1 1 2 2 3
  ^       ^ 
  s       f
```

```java
    public int removeDuplicates2(int[] nums) {
        if (nums.length <= 2) {
            return nums.length;
        }
        int slow = 1;
        int fast = 2;
        int max = 2;
        for (; fast < nums.length; fast++) {
            //不相等的话就添加
            if (nums[fast] != nums[slow - max + 1]) {
                nums[++slow] = nums[fast];
            }
        }
        return slow + 1;
    }
```
时间复杂度：O(n)。

空间复杂度：O(1)。