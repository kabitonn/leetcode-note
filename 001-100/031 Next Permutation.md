# 031. Next Permutation(M)
[031. Next Permutation](https://leetcode-cn.com/problems/next-permutation/)

## 题目描述\(中等\)

Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order \(ie, sorted in ascending order\).

The replacement must be in-place and use only constant extra memory.

Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.

```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

## 思路

1. 找到最靠后的前者小于后者的两个数，交换两数位置，逆序第一个数后面的部分
2. 从后往前找到第一个不再递增的数，从该位置起向后找到比该数大的最小的值，交换两数位置，逆序第一个数后面的部

## 解决方法

### 双重循环

双重循环找到靠后的前者小于后者的两个数

```java
    public void nextPermutation(int[] nums) {
        int index1 = -1;
        int index2 = -1;
        int len = nums.length;
        for(int i=0;i<len-1;i++) {
            for(int j=i+1;j<len;j++) {
                if(nums[i]<nums[j]) {
                    index1=i;
                    index2=j;
                }
            }
        }
        if(index1==-1)
            reverse(nums, 0);
        else {
            swap(nums, index1, index2);
            reverse(nums, index1+1);
        }
    }
    public void swap(int[] nums,int i,int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
    public void reverse(int[] nums,int i) {
        int j = nums.length-1;
        while(i<j) {
            swap(nums, i, j);
            i++;
            j--;
        }
    }
```

时间复杂度：$$ O(n^2) $$。

空间复杂度：O(1)。



### 单重循环

[!](asset/001-100/031-s-2-1.gif)

从右向左找到第一个数字不再递增的位置，然后从右边找到一个刚好大于当前位的数字即可

```java
    public void nextPermutation(int[] nums) {
        int i = nums.length-2;
        while(i>=0&&nums[i+1]<=nums[i]) {
            i--;
        }
        if(i<0) {
            reverse(nums, 0);
            return;
        }
        int j = i+1;
        while(j<nums.length&&nums[j]>nums[i]) {
            j++;
        }
        j--;
        swap(nums, i, j);
        reverse(nums, i+1);
    }
    public void swap(int[] nums,int i,int j) {
        int tmp = nums[i];
        nums[i] = nums[j];
        nums[j] = tmp;
    }
    public void reverse(int[] nums,int i) {
        int j = nums.length-1;
        while(i<j) {
            swap(nums, i, j);
            i++;
            j--;
        }
    }
```
时间复杂度：最坏的情况就是遍历完所有位，O(n)，倒置也是 O(n)，所以总体依旧是 O(n)。

空间复杂度：O(1)。


