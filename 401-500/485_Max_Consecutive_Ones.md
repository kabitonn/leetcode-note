
# 485. Max Consecutive Ones(E)

[485. 最大连续1的个数](https://leetcode-cn.com/problems/max-consecutive-ones/)

## 题目描述(中等)

给定一个二进制数组， 计算其中最大连续1的个数。

示例 1:
```
输入: [1,1,0,1,1,1]
输出: 3
解释: 开头的两位和最后的三位都是连续1，所以最大连续1的个数是 3.
```

**注意**：
- 输入的数组只包含 0 和1。
- 输入数组的长度是正整数，且不超过 10,000。


## 思路





## 解决方法

### 滑动窗口

```java
    public int findMaxConsecutiveOnes(int[] nums) {
        int i = 0, j = 0;
        int max = 0;
        while (j < nums.length) {
            if (nums[j] == 1) {
                j++;
            } else {
                max = Math.max(j - i, max);
                i = ++j;
            }
        }
        max = Math.max(j - i, max);
        return max;
    }
```

### 遍历

- 用一个计数器 count 记录 1 的数量，另一个计数器 maxCount 记录当前最大的 1 的数量。
- 当遇到 1 时，count 加一。
- 当遇到 0 时：
    - 将 count 与 maxCount 比较，maxCount 记录较大值
    - 将 count 设为 0。

```java
    public int findMaxConsecutiveOnes1(int[] nums) {
        int count = 0;
        int max = 0;
        for (int n : nums) {
            if (n == 1) {
                count++;
            } else {
                max = Math.max(count, max);
                count = 0;
            }
        }
        max = Math.max(count, max);
        return max;
    }
```