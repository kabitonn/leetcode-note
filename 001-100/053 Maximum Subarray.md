# 053. Maximum Subarray
[link](https://leetcode-cn.com/problems/maximum-subarray/)

## 题目描述\(简单\)

Given an integer array nums, find the contiguous subarray \(containing at least one number\) which has the largest sum and return its sum.

Example:

```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

Follow up:

> If you have figured out the O\(n\) solution, try coding another solution using the divide and conquer approach, which is more subtle.

## 思路

1. 动态规划

## 解决方法

### 动态规划1

一维数组 dp [ i ] 表示以下标 i 结尾的子数组的元素的最大的和，也就是这个子数组最后一个元素是下边为 i 的元素，并且这个子数组是所有以 i 结尾的子数组中，和最大的

有两种情况，

- 如果 dp [ i - 1 ] < 0，那么 dp [ i ] = nums [ i ]。
- 如果 dp [ i - 1 ] > = 0，那么 dp [ i ] = dp [ i - 1 ] + nums [ i ]

```java
    public int maxSubArray(int[] nums) {
        int len = nums.length;
        int[] maxSums = new int[len];
        maxSums[0] = nums[0];
        int maxSum = maxSums[0];
        for(int i=1;i<len;i++) {
            if(maxSums[i-1]<0) {
                maxSums[i] = nums[i];
            }else {
                maxSums[i] = maxSums[i-1] + nums[i];
            }
            if(maxSums[i]>maxSum) {    maxSum = maxSums[i];}
        }
        return maxSum;
    }
```
时间复杂度： O(n)。

空间复杂度：O(n)。

### 动态规划2

用一个变量表示以下标 i 结尾的子数组的元素的最大的和
```java
    public int maxSubArray(int[] nums) {
        int sum=0;
        int maxSum = Integer.MIN_VALUE;
        for(int num:nums){
            if(sum<0) {sum = num;}
            else {sum+=num;}
            if(maxSum<sum){    maxSum=sum;}
        }
        return maxSum;
    }
```
时间复杂度： O(n)。

空间复杂度：O(1)。


### 折半分治

1. 终止条件：left == right，那么 maxSubArrayPart 直接返回 nums [ left] 
2. 问题分解
    1. mid 不在我们要找的子数组中
        - 子数组的最大值要么是 mid 左半部分数组的子数组产生，要么是右边的产生，最大值的可以利用 maxSubArrayPart 求出来
    2. mid 在我们要找的子数组中
        - 我们可以分别从 mid 左边扩展，和右边扩展，找出两边和最大的时候，然后加起来就可以了。当然如果，左边或者右边最大的都小于 0 ，我们就不加了


```java
    public int maxSubArray(int[] nums) {
        return maxSubArrayPart(nums,0,nums.length-1);
    }

    private int maxSubArrayPart(int[] nums,int left,int right){
        if(left==right){
            return nums[left];
        }
        int mid=(left+right)/2;
        return Math.max(
            maxSubArrayPart(nums,left,mid),
            Math.max(
                maxSubArrayPart(nums,mid+1,right),
                maxSubArrayAll(nums,left,mid,right)
            )
        );
    }

    //左右两边合起来求解
    private int maxSubArrayAll(int[] nums,int left,int mid,int right){
        int leftSum=Integer.MIN_VALUE;
        int sum=0;
        for(int i=mid;i>=left;i--){
            sum+=nums[i];
            if(sum>leftSum){
                leftSum=sum;
            }
        }
        sum=0;
        int rightSum=Integer.MIN_VALUE;
        for(int i=mid+1;i<=right;i++){
            sum+=nums[i];
            if(sum>rightSum){
                rightSum=sum;
            }
        }
        return leftSum+rightSum;
    }
```
时间复杂度：O(n log ( n ))


