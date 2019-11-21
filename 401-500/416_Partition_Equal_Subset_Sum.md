
# 416. Partition Equal Subset Sum(M)

[416. 分割等和子集](https://leetcode-cn.com/problems/partition-equal-subset-sum/)

## 题目描述(中等)

给定一个只包含**正整数**的非空数组。是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

**注意**:
- 每个数组中的元素不会超过 100
- 数组的大小不会超过 200

示例 1:
```
输入: [1, 5, 11, 5]

输出: true

解释: 数组可以分割成 [1, 5, 5] 和 [11].
```

示例 2:
```
输入: [1, 2, 3, 5]

输出: false

解释: 数组不能分割成两个元素和相等的子集.
```

## 思路

## 解决方法

### 排序 DFS 递归



```java
    public boolean canPartition(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return true;
        }
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if ((sum & 1) != 0) {
            return false;
        }
        Arrays.sort(nums);
        sum >>= 1;
        if (nums[n - 1] > sum) {
            return false;
        }
        return combine(nums, n - 1, sum);
    }

    public boolean combine(int[] nums, int index, int sum) {
        if (sum < 0) {
            return false;
        } else if (sum == 0) {
            return true;
        }
        for (int i = index; i >= 0; i--) {
            if (combine(nums, i - 1, sum - nums[i])) {
                return true;
            }
        }
        return false;
    }

```

### 递归 记忆化

自顶向下动态规划
memo[n][sum]:

```java
    public boolean canPartition1(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return true;
        }
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if ((sum & 1) != 0) {
            return false;
        }
        sum >>= 1;
        Integer[][] memo = new Integer[n][sum + 1];
        return combine1(nums, n - 1, sum, memo);
    }

    public boolean combine1(int[] nums, int index, int sum, Integer[][] memo) {
        if (sum < 0 || index < 0) {
            return false;
        } else if (sum == 0) {
            return true;
        }
        if (memo[index][sum] != null) {
            return memo[index][sum] == 1;
        }
        memo[index][sum] = combine1(nums, index - 1, sum - nums[index], memo) || combine1(nums, index - 1, sum, memo) ? 1 : 0;
        return memo[index][sum] == 1;
    }
```

### 动态规划

dp[i][j]: 表示从数组的 [0, i] 这个子区间内挑选一些正整数，每个数只能用一次，使得这些数的和等于 j

dp[i][j] = 
- dp[i-1][j] or dp[i-1][j-nums[i]]  (nums[i] <= j )
- dp[i-1][j]                        (nums[i] >  j )

```java
    public boolean canPartition2(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return true;
        }
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if ((sum & 1) != 0) {
            return false;
        }
        sum >>= 1;
        boolean[][] dp = new boolean[n][sum + 1];
        dp[0][0] = true;
        if (nums[0] <= sum) {
            dp[0][nums[0]] = true;
        }
        for (int i = 1; i < n; i++) {
            for (int j = 0; j <= sum; j++) {
                if (j >= nums[i]) {
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[n - 1][sum];
    }
```

### 动态规划 优化

```java
    public boolean canPartition3(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return true;
        }
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if ((sum & 1) != 0) {
            return false;
        }
        sum >>= 1;
        boolean[] dp = new boolean[sum + 1];
        dp[0] = true;
        if (nums[0] <= sum) {
            dp[nums[0]] = true;
        }
        for (int i = 1; i < n; i++) {
            if (dp[sum]) {
                return true;
            }
            for (int j = sum; j >= nums[i]; j--) {
                dp[j] = dp[j] || dp[j - nums[i]];
            }
        }
        return dp[sum];
    }

```