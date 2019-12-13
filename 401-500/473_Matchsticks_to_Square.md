
# 473. Matchsticks to Square(M)

[473. 火柴拼正方形](https://leetcode-cn.com/problems/matchsticks-to-square/)

## 题目描述(中等)

还记得童话《卖火柴的小女孩》吗？现在，你知道小女孩有多少根火柴，请找出一种能使用所有火柴拼成一个正方形的方法。不能折断火柴，可以把火柴连接起来，并且每根火柴都要用到。

输入为小女孩拥有火柴的数目，每根火柴用其长度表示。输出即为是否能用所有的火柴拼成正方形。

示例 1:
```
输入: [1,1,2,2,2]
输出: true

解释: 能拼成一个边长为2的正方形，每边两根火柴。
```

示例 2:
```
输入: [3,3,3,3,4]
输出: false

解释: 不能用所有火柴拼成一个正方形。
```
**注意**:

1. 给定的火柴长度和在 0 到 10^9之间。
2. 火柴数组的长度不超过15。


## 思路

将数组元素分为四等份作为四条边

- 回溯+剪枝
- DFS+贪心


## 解决方法

### 回溯+剪枝

每根火柴属于一条边，所有火柴分配完后，判断是否等分

回溯判断4^n种情况

```java
    public boolean makesquare(int[] nums) {
        if (nums.length < 4) {
            return false;
        }
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if ((sum & 3) != 0) {
            return false;
        }
        int side = sum >> 2;
        Arrays.sort(nums);
        if (nums[nums.length - 1] > side) {
            return false;
        }
        int[] sides = new int[4];
        return backtrack_1(nums, nums.length - 1, sides, side);
    }

    public boolean backtrack(int[] nums, int index, int[] sides, int side) {
        if (index == -1) {
            for (int i = 0; i < 4; i++) {
                if (sides[i] != side) {
                    return false;
                }
            }
            return true;
        }
        for (int i = 0; i < 4; i++) {
            if (sides[i] + nums[index] > side) {
                continue;
            }
            sides[i] += nums[index];
            if (backtrack(nums, index - 1, sides, side)) {
                return true;
            }
            sides[i] -= nums[index];
        }
        return false;
    }
```

### 回溯+深度剪枝



```java
    public boolean backtrack_1(int[] nums, int index, int[] sides, int side) {
        if (index == -1) {
            return true;
        }
        for (int i = 0; i < 4; i++) {
            // 剪枝，即前一个桶和当前桶火柴数一致时，可以直接跳过，因为对于1根火柴来讲，两个桶如果当前大小一样，再放入时是没有区别的
            // 而前面那个桶放入失败，则如果重新放入也肯定失败，故可以直接跳过
            if (i == 0 || sides[i - 1] != sides[i]) {
                if (sides[i] + nums[index] > side) {
                    continue;
                }
                sides[i] += nums[index];
                if (backtrack_1(nums, index - 1, sides, side)) {
                    return true;
                }
                sides[i] -= nums[index];
            }
        }
        return false;
    }

```

### DFS + 贪心

数组经过排序后，从较大的开始选取，依次选出四部分相等的集合。选取较大的都能凑出一条边，剩余小的应更可能可以凑出一条边

```java
    public boolean makesquare1(int[] nums) {
        if (nums.length < 4) {
            return false;
        }
        int sum = 0;
        for (int num : nums) {
            sum += num;
        }
        if ((sum & 3) != 0) {
            return false;
        }
        int side = sum >> 2;
        Arrays.sort(nums);
        if (nums[nums.length - 1] > side) {
            return false;
        }
        boolean[] visited = new boolean[nums.length];
        int index = nums.length - 1;
        for (int i = 0; i < 4; i++) {
            while (visited[index]) {
                index--;
            }
            if (!dfs1(nums, index, 0, side, visited)) {
                return false;
            }
        }
        return true;
    }

    public boolean dfs1(int[] nums, int index, int len, int side, boolean[] visited) {
        if (len == side) {
            return true;
        }
        for (int i = index; i >= 0; i--) {
            if (visited[i] || len + nums[i] > side) {
                continue;
            }
            visited[i] = true;
            if (dfs1(nums, i - 1, len + nums[i], side, visited)) {
                return true;
            }
            visited[i] = false;
        }
        return false;
    }
```