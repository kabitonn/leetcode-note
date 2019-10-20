# 055. Jump Game(M)
[055. 跳跃游戏](https://leetcode-cn.com/problems/jump-game/)


## 题目描述(中等)


## 思路

1. 动态规划，存储当前位置能跳的最远距离

2. 贪心

## 解决方法


### 动态规划1



### 动态规划2

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