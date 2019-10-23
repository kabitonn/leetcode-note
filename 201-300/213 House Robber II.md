# 213. House Robber II(M)

[213. 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

## 题目描述(中等)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle**. That means the **first** house is the **neighbor **of the **last **one. Meanwhile, adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

Example 1:
```
Input: [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2),
             because they are adjacent houses.
```
Example 2:
```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

## 思路

- 动态规划


## 解决方法

### 动态规划

环状排列意味着第一个房子和最后一个房子中只能选择一个偷窃，因此可以把此环状排列房间问题约化为两个单排排列房间子问题：

- 在不偷窃第一个房子的情况下（即 nums[1:]），最大金额是 p1 
- 在不偷窃最后一个房子的情况下（即 nums[:n-1]），最大金额是 p2
- 综合偷窃最大金额： 为以上两种情况的较大值，即 max(p1,p2)max(p1,p2) 。



```java
    public int rob(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        } else if (len == 1) {
            return nums[0];
        }
        int sum1 = myRob(Arrays.copyOfRange(nums, 0, len - 1));
        int sum2 = myRob(Arrays.copyOfRange(nums, 1, len));
        return Math.max(sum1, sum2);
    }
    
    public int myRob(int[] nums) {
        int sum1 = 0, sum2 = 0;
        for (int i = 0; i < nums.length; i++) {
            int tmp = sum1;
            sum1 = sum2 + nums[i];
            sum2 = Math.max(sum2, tmp);
        }
        return Math.max(sum1, sum2);
    }

    public int myRob0(int[] nums) {
        int pre = 0, cur = 0;
        for (int n : nums) {
            int tmp = cur;
            cur = Math.max(pre + n, cur);
            pre = tmp;
        }
        return cur;
    }

```

### 动态规划优化

max1存放抢第一家的dp
max2存放抢最后一家的dp


```java
    public int rob01(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        } else if (len == 1) {
            return nums[0];
        }
        int[] max1 = new int[len];
        int[] max2 = new int[len + 1];
        max1[1] = nums[0];
        for (int i = 2; i < len; i++) {
            max1[i] = Math.max(max1[i - 2] + nums[i - 1], max1[i - 1]);
        }
        for (int i = 2; i <= len; i++) {
            max2[i] = Math.max(max2[i - 2] + nums[i - 1], max2[i - 1]);
        }
        return Math.max(max1[len - 1], max2[len]);
    }
```
继续优化成一个循环

```java
    public int rob1(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        } else if (len == 1) {
            return nums[0];
        }
        int[] max1 = new int[len + 1];
        int[] max2 = new int[len + 1];
        for (int i = 2; i <= len; i++) {
            max1[i] = Math.max(max1[i - 2] + nums[i - 2], max1[i - 1]);
            max2[i] = Math.max(max2[i - 2] + nums[i - 1], max2[i - 1]);
        }
        return Math.max(max1[len], max2[len]);
    }
```