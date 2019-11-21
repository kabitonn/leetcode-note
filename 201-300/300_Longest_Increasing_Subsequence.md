# 300. Longest Increasing Subsequence(M)

[300. 最长上升子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

## 题目描述(中等)

给定一个无序的整数数组，找到其中最长上升子序列的长度。

示例:
```
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
```

**说明**:
- 可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
- 你算法的时间复杂度应该为 O(n^2) 。

**进阶**: 你能将算法的时间复杂度降低到 O(n log n) 吗?


## 思路

- 动态规划
- 二分查找

## 解决方法

### 动态规划

- dp[i] 表示以第 i 个数字为结尾的“最长上升子序列”的长度
- 转移方程：dp[i] = max(1 + dp[j] for j < i if nums[j] < nums[i],dp[i])

```java
    public int lengthOfLIS(int[] nums) {
        int maxLen = 0;
        int n = nums.length;
        int[] dp = new int[n];
        for (int i = 0; i < n; i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[i] > nums[j]) {
                    dp[i] = Math.max(dp[j] + 1, dp[i]);
                }
            }
            maxLen = Math.max(maxLen, dp[i]);
        }
        return maxLen;
    }

```

时间复杂度：O(n^2)
空间复杂度：O(n)，和数组等长的状态数组

### 贪心 + 二分查找

算法思想
> 如果前面的数越小，后面接上一个随机数，就会有更大的可能性构成一个更长的“上升子序列”。 

- tails 列表一定是严格递增的： 即当尽可能使每个子序列尾部元素值最小的前提下，子序列越长，其序列尾部元素值一定更大。
    - 反证法证明： 当 k < i，若 tails[k] >= tails[i]，代表较短子序列的尾部元素的值 > 较长子序列的尾部元素的值。这是不可能的，因为从长度为 i 的子序列尾部倒序删除 i-1 个元素，剩下的为长度为 k 的子序列，设此序列尾部元素值为 v，则一定有 v<tails[i] （即长度为 k 的子序列尾部元素值一定更小）， 这和 tails[k]>=tails[i] 矛盾。
    - 既然严格递增，每轮计算 tails[k] 时就可以使用二分法查找需要更新的尾部元素值的对应索引 i

算法流程
- tail[i] 表示长度为 i + 1 的所有“上升子序列”里结尾最小的元素。
- 设 len 为 tails 当前长度，代表直到当前的最长上升子序列长度。设 j∈[0,res)，考虑每轮遍历 nums[k]时，通过二分法遍历 [0,res)列表区间，找出 nums[k]的大小分界点，会出现两种情况：
    - 区间中存在 tails[i] > nums[k] ： 将第一个满足 tails[i] > nums[k] 执行 tails[i] = nums[k]；因为更小的 nums[k] 后更可能接一个比它大的数字。
    - 区间中不存在 tails[i] > nums[k] ： 意味着 nums[k] 可以接在前面所有长度的子序列之后，因此肯定是接到最长的后面（长度为 len ），新子序列长度为 len + 1。
- 返回 len ，即最长上升子子序列长度

```java
    public int lengthOfLIS1(int[] nums) {
        int len = 0;
        int n = nums.length;
        int[] tails = new int[n];
        for (int num : nums) {
            int left = 0, right = len;
            // 二分找到大于等于num的左边界，即num在tails中应该存在的位置
            while (left < right) {
                int mid = (left + right) >>> 1;
                if (tails[mid] < num) {
                    left = mid + 1;
                } else {
                    right = mid;
                }
            }
            tails[left] = num;
            if (left == len) {
                len++;
            }
        }
        return len;
    }
```

时间复杂度 O(nlogn)： 遍历 nums 列表需 O(n)，在每个 nums[i] 二分法需 O(logn)。
空间复杂度 O(n) ： tails 列表占用线性大小额外空间。

