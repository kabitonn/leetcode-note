
# 494. Target Sum(M)

[494. 目标和](https://leetcode-cn.com/problems/target-sum/)

## 题目描述(中等)

给定一个非负整数数组，`a1, a2, ..., an`, 和一个目标数，S。现在你有两个符号 `+` 和  `-` 。对于数组中的任意一个整数，你都可以从 `+` 或 `-` 中选择一个符号添加在前面。

返回可以使最终数组和为目标数 S 的所有添加符号的方法数。

示例 1:
```
输入: nums: [1, 1, 1, 1, 1], S: 3
输出: 5
解释: 

-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3

一共有5种方法让最终目标和为3。
```

**注意**:
- 数组非空，且长度不会超过20。
- 初始的数组的和不会超过1000。
- 保证返回的最终结果能被32位整数存下。


## 思路

- DFS
- BFS
- 

## 解决方法

### DFS 

```java
    public int findTargetSumWays(int[] nums, int S) {
        return dfs(nums, S, 0, 0);
    }

    public int dfs(int[] nums, int S, int sum, int index) {
        if (index == nums.length) {
            return S == sum ? 1 : 0;
        }
        int num = 0;
        num += dfs(nums, S, sum + nums[index], index + 1);
        num += dfs(nums, S, sum - nums[index], index + 1);
        return num;
    }
```


### 递归 记忆化

memo[index] 存储的是[index,length)之间能构成的和以及构成该和的组成方法的个数

```java
        public int findTargetSumWays0(int[] nums, int S) {
        Map<Integer, Integer>[] memo = new Map[nums.length];
        for (int i = 0; i < nums.length; i++) {
            memo[i] = new HashMap<>();
        }
        return dfs(nums, S, 0, 0, memo);
    }

    public int dfs(int[] nums, int S, int sum, int index, Map<Integer, Integer>[] memo) {
        if (index == nums.length) {
            return S == sum ? 1 : 0;
        }
        if (memo[index].containsKey(sum)) {
            return memo[index].get(sum);
        }
        int num = 0;
        num += dfs(nums, S, sum + nums[index], index + 1, memo);
        num += dfs(nums, S, sum - nums[index], index + 1, memo);
        memo[index].put(sum, num);
        return num;
    }
```

### 动态规划

数组 nums 的总和为 base , 所以能组成的数字和范围为 [-base,base],将其映射到[0,base*2]

- 状态：`dp[i][sum]` ：在`[0,i]`组成sum的个数
- 状态转移
  - `sum - num >= 0 : dp[i][sum] += dp[i-1][sum-num]`
  - `sum + num <= base*2: dp[i][sum] += dp[i-1][sum+num]`

```java
    public int findTargetSumWays2(int[] nums, int S) {
        int n = nums.length;
        int base = 0;
        for (int num : nums) {
            base += num;
        }
        if (S > base) {
            return 0;
        }
        int[][] dp = new int[n + 1][base * 2 + 1];
        dp[0][base] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = -base; j <= base; j++) {
                if (j + base - nums[i - 1] >= 0) {
                    dp[i][j + base] += dp[i - 1][j + base - nums[i - 1]];
                }
                if (j + base + nums[i - 1] <= base * 2) {
                    dp[i][j + base] += dp[i - 1][j + base + nums[i - 1]];
                }
            }
        }
        return dp[n][S + base];
    }
```

### 动态规划 空间优化

观察到dp[i][sum]只和dp[i-1][sum+-num]有关

```java
    public int findTargetSumWays3(int[] nums, int S) {
        int base = 0;
        for (int num : nums) {
            base += num;
        }
        if (S > base || (S + base) % 2 == 1) {
            return 0;
        }
        int[] dp = new int[base * 2 + 1];
        dp[base] = 1;
        for (int num : nums) {
            int[] next = new int[base * 2 + 1];
            for (int sum = -base; sum <= base; sum++) {
                if (sum + num <= base) {
                    next[sum + base + num] += dp[sum + base];
                }
                if (sum + base - num >= 0) {
                    next[sum + base - num] += dp[sum + base];
                }
            }
            dp = next;
        }
        return dp[S + base];
    }
```

### 动态规划

数组 nums 的总和为 base , 所以能组成的数字和范围为 [-base,base],将其映射到[0,base*2]，所求目标S即为S+base

若(S+base)为奇数，必然无法构成该和：S和base其中有一个为奇数(base为奇数，S为偶数或base为偶数，S为奇数），对于任意一个数字取正负相差的差都为偶数，无论怎么组合正负组成的数字奇偶性和base是一致的

即其中 `+` 的数字组成的数字和为 `(S+base)/2`, 所以 `-` 的数字组成的数字和为 `base-(S+base)/2=(base-S)/2`;最后和为S。

所以题目可转化为在[0，length)中选取若干个数字相加组成`(S+base)/2`

```java
    public int findTargetSumWays4(int[] nums, int S) {
        int base = 0;
        for (int num : nums) {
            base += num;
        }
        if (S > base || (S + base) % 2 == 1) {
            return 0;
        }
        int target = (S + base) / 2;
        int[] dp = new int[target + 1];
        dp[0] = 1;
        for (int num : nums) {
            for (int i = target; i >= num; i--) {
                dp[i] += dp[i - num];
            }
        }
        return dp[target];
    }
```

### BFS 

!> 超时

```java
    //超时
    public int findTargetSumWays1(int[] nums, int S) {
        if (nums.length == 0) {
            return 0;
        }
        Queue<Integer> queue = new LinkedList<>();
        queue.add(0);
        for (int num : nums) {
            int size = queue.size();
            for (int j = 0; j < size; j++) {
                int n = queue.poll();
                queue.add(n + num);
                queue.add(n - num);
            }
        }
        int count = 0;
        for (int n : queue) {
            count += n == S ? 1 : 0;
        }
        return count;
    }
```