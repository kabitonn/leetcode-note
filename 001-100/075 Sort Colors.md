# 075. Sort Colors(M)

[075. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

## 题目描述(中等)

Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

**Note**: You are not suppose to use the library's sort function for this problem.

Example:
```
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```
Follow up:

- A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
- Could you come up with a one-pass algorithm using only constant space?



## 思路

1. 各种颜色计数后排序
2. 双指针
3. 三指针


## 解决方法


### 各色计数

```java
    public void sortColors(int[] nums) {
        int redNum = 0, whiteNum = 0, blueNum = 0;
        for (int n : nums) {
            if (n == 0) {
                redNum++;
            } else if (n == 1) {
                whiteNum++;
            } else if (n == 2) {
                blueNum++;
            }
        }
        int index = 0;
        for (; index < redNum; index++) {
            nums[index] = 0;
        }
        for (; index < redNum + whiteNum; index++) {
            nums[index] = 1;
        }
        for (; index < nums.length; index++) {
            nums[index] = 2;
        }
    }
```

### 双指针

两个指针p0，p2指向当前数组0的末尾的下一项，2的首项的前一项；
```java
    public void sortColors1(int[] nums) {
        int p0 = 0, p2 = nums.length - 1;
        int cur = 0;
        int tmp;
        while (cur <= p2) {
            if (nums[cur] == 0) {
                tmp = nums[p0];
                nums[p0++] = nums[cur];
                nums[cur++] = tmp;
            } else if (nums[cur] == 2) {
                tmp = nums[p2];
                nums[p2--] = nums[cur];
                nums[cur] = tmp;
            } else if (nums[cur] == 1) {
                cur++;
            }
        }
    }

```

### 三指针

三个指针 p0，p1，p2，分别代表已排好序的数组当前 0 的末尾，1 的末尾，2 的末尾。

```java
    public void sortColors2(int[] nums) {
        int p0 = -1, p1 = -1, p2 = -1;
        int n = nums.length;
        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                p2++;
                nums[p2] = 2;
                p1++;
                nums[p1] = 1;
                p0++;
                nums[p0] = 0;
            } else if (nums[i] == 1) {
                p2++;
                nums[p2] = 2;
                p1++;
                nums[p1] = 1;
            } else if (nums[i] == 2) {
                p2++;
                nums[p2] = 2;
            }
        }
    }
```
