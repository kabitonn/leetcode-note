# 088. Merge Sorted Array(E)
[88. 合并两个有序数组](https://leetcode-cn.com/problems/merge-sorted-array/)

## 题目描述(简单)

Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.

**Note**:
> - The number of elements initialized in nums1 and nums2 are m and n respectively.
> - You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2.

Example:
```
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```

## 思路
1. 直接法
2. 正序插入
3. 后序插入以节省空间

## 解决方法

### 直接法

nums1 作为被插入的数组，然后遍历 nums2。用两个指针 i 和 j ，i 指向 nums1 当前判断的数字，j 指向 num2 当前遍历的数字。如果 j 指向的数字小于 i 指向的数字，那么就做插入操作。否则的话后移 i ，找到需要插入的位置 。

```java
    public void merge2(int[] nums1, int m, int[] nums2, int n) {
        int i = 0, j = 0;
        //遍历 nums2
        while (j < n) {
            //判断 nums1 是否遍历完
            //（nums1 原有的数和当前已经插入的数相加）和 i 进行比较
            if (i == m + j) {
                //将剩余的 nums2 插入
                while (j < n) {
                    nums1[m + j] = nums2[j];
                    j++;
                }
                return;
            }
            //判断当前 nums2 是否小于 nums1
            if (nums2[j] < nums1[i]) {
                //nums1 后移数组，空出位置以便插入
                for (int k = m + j; k > i; k--) {
                    nums1[k] = nums1[k - 1];
                }
                nums1[i++] = nums2[j++];
            } else {
                i++;
            }
        }
    }
``    

### 正序插入
原来的 nums1 的数字放到nums1 的末尾，
只考虑如果 nums1 遍历结束，将 nums2 直接加入。为什么不考虑如果 nums2 遍历结束，将 nums1 直接加入呢？因为最开始的时候已经把 nums1 全部放到了末尾，所以不需要再赋值了

```java
    public void merge1(int[] nums1, int m, int[] nums2, int n) {
        //将 nums1 的数字全部移动到末尾
        for (int i = 1; i <= m; i++) {
            nums1[m + n - i] = nums1[m - i];
        }
        int i = n;
        int j = 0;
        int k = 0;
        //遍历 nums2
        while (j < n) {
            //如果 nums1 遍历结束，将 nums2 直接加入
            if (i == m + n) {
                while (j < n) {
                    nums1[k++] = nums2[j++];
                }
                return;
            }
            //哪个数小就对应的添加哪个数
            if (nums2[j] < nums1[i]) {
                nums1[k++] = nums2[j++];
            } else {
                nums1[k++] = nums1[i++];
            }
        }
    }
```

### 后续插入
后序选择大者插入

```java
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;
        while (j >= 0) {
            if (i >= 0 && nums1[i] > nums2[j]) {
                nums1[k--] = nums1[i--];
            } else {
                nums1[k--] = nums2[j--];
            }
        }
    }
```

时间复杂度：O(n)。

空间复杂度：O(1)。



