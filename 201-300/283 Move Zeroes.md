# 283. Move Zeroes(E)
[283. Move Zeroes](https://leetcode-cn.com/problems/move-zeroes/)

## 题目描述\(简单\)

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Example:

```
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

**Note:**

* You must do this in-place without making a copy of the array.
* Minimize the total number of operations.

## 思路

## 解决方法

### 暴力法

```java
    public void moveZeroes(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if(nums[i]==0) {
                int pos = i;
                for(int j=i+1;j<nums.length;j++) {
                    if(nums[j]==0) {continue;}
                    swap(nums, pos, j);
                    pos = j;
                }
            }
        }
    }
    public void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
```

### 只关注非0
只固定非0元素，末尾补0

```java
    public void moveZeroes1(int[] nums) {
        int pos = 0;
        for(int i=0;i<nums.length;i++) {
            if(nums[i]!=0) {
                nums[pos++] = nums[i];
            }
        }
        for(int i=pos;i<nums.length;i++) {
            nums[i] = 0;
        }
    }
```
时间复杂度：O(n)。但是，操作仍然是局部优化的。代码执行的总操作（数组写入）为 n（元素总数）。
空间复杂度：O(1)，只使用常量空间。
### 遍历替换

- 慢指针（pos）之前的所有元素都是非零的。
- 当前指针和慢速指针之间的所有元素都是零。

```java
    public void moveZeroes2(int[] nums) {
        int pos = 0;
        for(int i=0;i<nums.length;i++) {
            if(nums[i]!=0) {
                swap(nums, pos++, i);
            }
        }
    }
    public void swap(int[] nums, int i, int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
```
时间复杂度：O(n)。但是，操作是最优的。代码执行的总操作（数组写入）是非 0 元素的数量。
空间复杂度：O(1)，只使用了常量空间。



