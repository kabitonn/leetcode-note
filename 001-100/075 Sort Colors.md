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
        int[] colors = new int[3];
        for (int n : nums) {
            colors[n]++;
        }
        int index = 0;
        int sortedNum = 0;
        for (int i = 0; i < colors.length; i++) {
            for (; index < colors[i] + sortedNum; index++) {
                nums[index] = i;
            }
            sortedNum += colors[i];
        }
    }
```

### 双指针

两个指针p0，p2指向当前数组0的最右边界，2的最左边界；
p0, p2只会指向1

- 初始化0的最右边界：p0 = 0。在整个算法执行过程中 nums[idx < p0] = 0.

- 初始化2的最左边界 ：p2 = n - 1。在整个算法执行过程中 nums[idx > p2] = 2.

- 初始化当前考虑的元素序号 ：curr = 0.

- While curr <= p2 :

    - 若 nums[curr] = 0 ：交换第 curr个 和 第p0个 元素，并将指针都向右移。

    - 若 nums[curr] = 2 ：交换第 curr个和第 p2个元素，并将 p2指针左移 。

    - 若 nums[curr] = 1 ：将指针curr右移。



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
时间复杂度 :由于对长度 N 的数组进行了一次遍历，时间复杂度为O(N) 。

空间复杂度 :由于只使用了常数空间，空间复杂度为O(1) 。


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
