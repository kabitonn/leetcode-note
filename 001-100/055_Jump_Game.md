# 055. Jump Game(M)
[055. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)


## 题目描述(中等)


## 思路

1. 动态规划，存储当前位置能跳的最远距离

2. 贪心

## 解决方法


### 动态规划
若当前节点能到达时
dp[i] = max(dp[i-1],i+nums[i])

```java
    public boolean canJump(int[] nums) {
        int len = nums.length;
        if (len == 0) {
            return true;
        }
        int[] step = new int[len];
        step[0] = nums[0];
        for (int i = 1; i < len; i++) {
            if (step[i - 1] < i) {
                return false;
            }
            step[i] = Math.max(i + nums[i], step[i - 1]);
        }
        return step[len - 1] >= len - 1;
    }
```
时间复杂度是O(n)
空间复杂度是O(n)


```java
    public boolean canJump1(int[] nums) {
        if (nums == null || nums.length == 0) {
            return true;
        }
        int len = nums.length;
        int step = 0;
        for (int i = 0; i < len; i++) {
            if (step < i) {
                return false;
            }
            step = Math.max(i + nums[i], step);
        }
        return step >= len - 1;
    }
```
时间复杂度是O(n)
空间复杂度是O(1)



### 贪心 从后向前

记录一个的坐标代表当前可达的最后节点，这个坐标初始等于nums.length-1，
然后我们每判断完是否可达，都向前移动这个坐标，直到遍历结束。
如果这个坐标等于0，那么认为可达，否则不可达。

```java
    public boolean canJump2(int[] nums) {
        if (nums == null) {
            return false;
        }
        int lastPosition = nums.length - 1;
        for (int i = nums.length - 1; i >= 0; i--) {
            // 逐步向前递推
            if (nums[i] + i >= lastPosition) {
                lastPosition = i;
            }
        }
        return lastPosition == 0;
    }

```
时间复杂度是O(n)
空间复杂度是O(1)