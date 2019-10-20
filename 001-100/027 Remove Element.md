# 027. Remove Element(E)
[027. Remove Element](https://leetcode-cn.com/problems/remove-element/)

## 题目描述\(简单\)

Given an array nums and a value val, remove all instances of that value in-place and return the new length.

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O\(1\) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example 1:

```
Given nums = [3,2,2,3], val = 3,
Your function should return length = 2, with the first two elements of nums being 2.
It doesn't matter what you leave beyond the returned length.
```

Example 2:

```
Given nums = [0,1,2,2,3,0,4,2], val = 2,
Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.
```

**Note** that the order of those five elements can be arbitrary.

> It doesn't matter what values are set beyond the returned length.

## 思路

1. 遍历数组，不为val，有效索引赋值并递增
2. 遍历数组，若为val，将数组最后移至该位置，否则继续遍历

## 解决方法

### 快慢指针 不等赋值

快指针 fast 和慢指针 slow，一直移动 fast ，如果 fast 指向的值不等于给定的 val ，我们就将值赋给 slow 指向的位置，slow 后移一位。如果 fast 指向的值等于 val 了，此时 fast 后移一位就可以了，不做其他操作

```java
    public int removeElement(int[] nums, int val) {
        int len = 0;
        for(int i=0;i<nums.length;i++) {
            if(nums[i]!=val) {
                nums[len++] = nums[i];
            }
        }
        return len;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。

### 快慢指针 相等移除

如果当前元素等于 val 了，就把它扔掉，然后将最后一个值赋值到当前位置，并且长度减去 1

```java
    public int removeElement(int[] nums, int val) {
        int len = nums.length;
        int i=0;
        while(i<len) {
            if(nums[i]==val) {
                nums[i] = nums[--len];
            }
            else {
                i++;
            }
        }
        return len;
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。



