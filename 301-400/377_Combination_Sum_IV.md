
# 377. Combination Sum IV(M)

[377. 组合总和 Ⅳ](https://leetcode-cn.com/problems/combination-sum-iv/)

## 题目描述(中等)

给定一个由正整数组成且不存在重复数字的数组，找出和为给定目标正整数的组合的个数。

示例:
```
nums = [1, 2, 3]
target = 4

所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

请注意，顺序不同的序列被视作不同的组合。

因此输出为 7。
```
**进阶**：
- 如果给定的数组中含有负数会怎么样？
- 问题会产生什么变化？
- 我们需要在题目中添加什么限制来允许负数的出现？


## 思路

- 递归回溯
- 递归记忆化
- 

## 解决方法

### 递归

超时

```java
    public int combinationSum4(int[] nums, int target) {
        if (target < 0) {
            return 0;
        } else if (target == 0) {
            return 1;
        }
        int count = 0;
        for (int num : nums) {
            count += combinationSum4(nums, target - num);
        }
        return count;
    }

```

### 递归记忆化

```java
    public int combinationSum4_1(int[] nums, int target) {
        Integer[] memo = new Integer[target + 1];
        memo[0] = 1;
        return combinationSum(nums, target, memo);
    }

    public int combinationSum(int[] nums, int target, Integer[] memo) {
        if (target < 0) {
            return 0;
        }
        if (memo[target] != null) {
            return memo[target];
        }
        int count = 0;
        for (int num : nums) {
            count += combinationSum(nums, target - num, memo);
        }
        memo[target] = count;
        return count;
    }
```

### 动态规划

dp[i] ：对于给定的由正整数组成且不存在重复数字的数组，和为 i 的组合的个数。

dp[i] = sum{dp[i - num] for num in nums and if i >= num}

定义 dp[0] = 1 的，它表示如果 nums 里有一个数恰好等于 target，它单独成为 1 种可能。

```java
    public int combinationSum4_2(int[] nums, int target) {
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (int num : nums) {
                if (i >= num) {
                    dp[i] += dp[i - num];
                }
            }
        }
        return dp[target];
    }
```

### 数学法

首先计算nums中所有数的最大公约数g，如果g不能整除target，那么答案必定为0


