# 198. House Robber(E) 
[198. House Robber(E)](https://leetcode-cn.com/problems/house-robber/)

## 题目描述\(简单\)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night.**

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

Example 1:

```
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
             Total amount you can rob = 1 + 3 = 4.
```

Example 2:

```
Input: [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
             Total amount you can rob = 2 + 9 + 1 = 12.
```

## 思路

动态规划

$$f(k)  = max  (f(k-2) + A_k, f(k-1))$$

## 解决方法

- 状态定义：
    - 设动态规划列表 dp ，dp[i] 代表前 i 个房子在满足条件下的能偷窃到的最高金额。
- 转移方程：
    - 设： 有 n 个房子，前 n 间能偷窃到的最高金额是 dp[n] ，前 n-1 间能偷窃到的最高金额是 dp[n-1] ，此时向这些房子后加一间房，此房间价值为 num ；
    - 加一间房间后： 由于不能抢相邻的房子，意味着抢第 n+1 间就不能抢第 n 间；那么前 n+1 间房能偷取到的最高金额 dp[n+1] 一定是以下两种情况的 较大值 ：
        - 不抢第 n+1个房间，因此等于前 n 个房子的最高金额，即 dp[n+1] = dp[n]；
        - 抢第  n+1 个房间，此时不能抢第 n 个房间；因此等于前 n-1 个房子的最高金额加上当前房间价值，即dp[n+1] = dp[n-1] + num；
    - 最终的转移方程： dp[n+1] = max(dp[n],dp[n-1]+num)
- 初始状态：
    - 前 0 间房子的最大偷窃价值为 0 ，即 dp[0] = 0。
- 返回值：
    - 返回 dp 列表最后一个元素值，即所有房间的最大偷窃价值。
- 简化空间复杂度：
    - 我们dp[n] 只与 dp[n-1] 和 dp[n−2] 有关系，因此我们可以设两个变量 cur和 pre 交替记录，将空间复杂度降到 O(1)



### dp数组保存各个状态最大值

max\[i\]为遍历到第i-1家时的最大值

```java
    public int rob(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return 0;
        } else if (len == 1) {
            return nums[0];
        }
        int[] max = new int[len + 1];
        max[1] = nums[0];
        for (int i = 2; i <= len; i++) {
            max[i] = Math.max(max[i - 2] + nums[i - 1], max[i - 1]);
        }
        return max[len];
    }
```

时间复杂度：O(n)。其中 n 为房子的数量。  
空间复杂度：O(n)。

### 两个变量保存前一状态和当前状态最大值

pre 保存上一家最大值
cur 保存当前最大值
```java
    public int rob(int[] nums) {
        int pre = 0, cur = 0;
        for (int n : nums) {
            int tmp = cur;
            cur = Math.max(pre + n, cur);
            pre = tmp;
        }
        return cur;
    }
```

时间复杂度：O(n)。其中 n 为房子的数量。  
空间复杂度：O(1)。


### 两个变量保存当前位置两种状态

sum1为抢当前房子的最大值  
sum2为不抢当前房子最大值

```java
    public int rob(int[] nums) {
        int sum1 = 0;
        int sum2 = 0;
        for(int n:nums) {
            int tmp = sum1;
            sum1 = sum2+n;
            sum2 = Math.max(tmp,sum2);
        }

        return Math.max(sum1, sum2);
    }
```

时间复杂度：O(n)。其中 n 为房子的数量。  
空间复杂度：O(1)

### 3

sumEven 和 sumOdd为相邻奇偶数位置的最大值

```java
    public int rob(int[] nums) {
        int sumOdd = 0;
        int sumEven = 0;

        for (int i = 0; i < nums.length; i++)
        {
            if (i % 2 == 0)
            {
                sumEven += nums[i];
                sumEven = Math.max(sumOdd, sumEven);
            }
            else
            {
                sumOdd += nums[i];
                sumOdd = Math.max(sumOdd, sumEven);
            }
        }
        return Math.max(sumOdd, sumEven);
    }
```
时间复杂度：O(n)。其中 n 为房子的数量。  
空间复杂度：O(1)




